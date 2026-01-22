---
applyTo: "**/*.java"
---

# Java Development Instructions

## Code Style & Standards

### Naming Conventions
```java
// Classes: PascalCase
public class TaskManager {}

// Interfaces: PascalCase (no I prefix)
public interface TaskRepository {}

// Methods: camelCase
public void addTask() {}

// Variables: camelCase
private String taskTitle;

// Constants: UPPER_SNAKE_CASE
private static final int MAX_TASKS = 100;

// Packages: lowercase
package com.example.taskmanager;
```

### Class Structure
```java
// 1. Package declaration
package com.example.taskmanager.service;

// 2. Imports (grouped: java, javax, third-party, project)
import java.util.List;
import javax.validation.Valid;
import org.springframework.stereotype.Service;
import com.example.taskmanager.model.Task;

// 3. Class documentation
/**
 * Service for managing tasks.
 * Handles CRUD operations and business logic.
 */
// 4. Annotations
@Service
public class TaskService {
    
    // 5. Static fields
    private static final Logger logger = LoggerFactory.getLogger(TaskService.class);
    
    // 6. Instance fields
    private final TaskRepository taskRepository;
    
    // 7. Constructor
    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }
    
    // 8. Public methods
    public Task createTask(Task task) {
        // Implementation
    }
    
    // 9. Private methods
    private void validateTask(Task task) {
        // Implementation
    }
}
```

## Object-Oriented Principles

### SOLID Principles
```java
// Single Responsibility
// Each class should have one reason to change
public class TaskValidator {
    public boolean isValid(Task task) {
        return task != null && task.getTitle() != null;
    }
}

// Dependency Injection (prefer constructor injection)
@Service
public class TaskService {
    private final TaskRepository repository;
    private final TaskValidator validator;
    
    public TaskService(TaskRepository repository, TaskValidator validator) {
        this.repository = repository;
        this.validator = validator;
    }
}

// Interface Segregation
// Many specific interfaces > one general interface
public interface TaskReader {
    Task findById(Long id);
    List<Task> findAll();
}

public interface TaskWriter {
    Task save(Task task);
    void delete(Long id);
}
```

### Inheritance vs Composition
```java
// Prefer composition over inheritance
public class TaskManager {
    private final TaskRepository repository;
    private final TaskNotifier notifier;
    
    // Composition provides flexibility
}

// Use inheritance only for true "is-a" relationships
public class UrgentTask extends Task {
    // UrgentTask truly "is-a" Task
}
```

## Exception Handling

### Exception Hierarchy
```java
// Custom exceptions
public class TaskNotFoundException extends RuntimeException {
    public TaskNotFoundException(Long id) {
        super("Task not found with id: " + id);
    }
}

public class InvalidTaskException extends RuntimeException {
    public InvalidTaskException(String message) {
        super(message);
    }
}
```

### Try-Catch Best Practices
```java
// Specific exceptions first
try {
    Task task = taskRepository.findById(id);
    processTask(task);
} catch (TaskNotFoundException e) {
    logger.error("Task not found: {}", id, e);
    throw e;
} catch (Exception e) {
    logger.error("Unexpected error processing task: {}", id, e);
    throw new RuntimeException("Error processing task", e);
}

// Try-with-resources for auto-closeable resources
try (BufferedReader reader = new BufferedReader(new FileReader("tasks.txt"))) {
    String line = reader.readLine();
    // Process line
}
```

## Collections & Streams

### Collection Usage
```java
// Use interfaces for declarations
List<Task> tasks = new ArrayList<>();
Map<Long, Task> taskMap = new HashMap<>();
Set<String> tags = new HashSet<>();

// Immutable collections when possible
List<Task> tasks = List.of(task1, task2, task3);
Map<Long, String> taskMap = Map.of(1L, "Task 1", 2L, "Task 2");
```

### Stream API
```java
// Filter, map, collect pattern
List<String> completedTaskTitles = tasks.stream()
    .filter(Task::isCompleted)
    .map(Task::getTitle)
    .collect(Collectors.toList());

// Reduce operations
int totalDuration = tasks.stream()
    .mapToInt(Task::getDuration)
    .sum();

// Group by
Map<Boolean, List<Task>> tasksByCompletion = tasks.stream()
    .collect(Collectors.groupingBy(Task::isCompleted));

// Parallel streams for large datasets
tasks.parallelStream()
    .filter(Task::isActive)
    .forEach(this::processTask);
```

## Java 8+ Features

### Lambda Expressions
```java
// Use lambda for functional interfaces
tasks.forEach(task -> System.out.println(task.getTitle()));

// Method references when appropriate
tasks.forEach(System.out::println);
tasks.stream().map(Task::getTitle);

// Comparator with lambda
tasks.sort((t1, t2) -> t1.getTitle().compareTo(t2.getTitle()));
tasks.sort(Comparator.comparing(Task::getTitle));
```

### Optional
```java
// Use Optional for potentially null values
public Optional<Task> findTaskById(Long id) {
    return taskRepository.findById(id);
}

// Handle Optional properly
Optional<Task> taskOpt = findTaskById(id);
taskOpt.ifPresent(task -> processTask(task));

Task task = taskOpt.orElse(getDefaultTask());
Task task = taskOpt.orElseThrow(() -> new TaskNotFoundException(id));

// Transform Optional values
String title = taskOpt
    .map(Task::getTitle)
    .orElse("No title");
```

### Records (Java 14+)
```java
// Use records for immutable data carriers
public record TaskDto(Long id, String title, boolean completed) {}

// With validation
public record TaskDto(Long id, String title, boolean completed) {
    public TaskDto {
        if (title == null || title.isBlank()) {
            throw new IllegalArgumentException("Title cannot be blank");
        }
    }
}
```

## Spring Boot Best Practices

### Controller Layer
```java
@RestController
@RequestMapping("/api/tasks")
public class TaskController {
    
    private final TaskService taskService;
    
    public TaskController(TaskService taskService) {
        this.taskService = taskService;
    }
    
    @GetMapping
    public ResponseEntity<List<Task>> getAllTasks() {
        List<Task> tasks = taskService.findAll();
        return ResponseEntity.ok(tasks);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<Task> getTask(@PathVariable Long id) {
        return taskService.findById(id)
            .map(ResponseEntity::ok)
            .orElse(ResponseEntity.notFound().build());
    }
    
    @PostMapping
    public ResponseEntity<Task> createTask(@Valid @RequestBody Task task) {
        Task created = taskService.create(task);
        return ResponseEntity
            .created(URI.create("/api/tasks/" + created.getId()))
            .body(created);
    }
    
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteTask(@PathVariable Long id) {
        taskService.delete(id);
        return ResponseEntity.noContent().build();
    }
}
```

### Service Layer
```java
@Service
@Transactional
public class TaskService {
    
    private final TaskRepository taskRepository;
    
    public TaskService(TaskRepository taskRepository) {
        this.taskRepository = taskRepository;
    }
    
    @Transactional(readOnly = true)
    public List<Task> findAll() {
        return taskRepository.findAll();
    }
    
    @Transactional(readOnly = true)
    public Optional<Task> findById(Long id) {
        return taskRepository.findById(id);
    }
    
    public Task create(Task task) {
        validateTask(task);
        return taskRepository.save(task);
    }
    
    private void validateTask(Task task) {
        if (task.getTitle() == null || task.getTitle().isBlank()) {
            throw new InvalidTaskException("Title is required");
        }
    }
}
```

### Repository Layer
```java
@Repository
public interface TaskRepository extends JpaRepository<Task, Long> {
    
    List<Task> findByCompleted(boolean completed);
    
    @Query("SELECT t FROM Task t WHERE t.dueDate < :date AND t.completed = false")
    List<Task> findOverdueTasks(@Param("date") LocalDate date);
    
    Optional<Task> findByTitleIgnoreCase(String title);
}
```

### Entity Layer
```java
@Entity
@Table(name = "tasks")
public class Task {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false, length = 200)
    private String title;
    
    @Column(columnDefinition = "TEXT")
    private String description;
    
    @Column(name = "completed")
    private boolean completed = false;
    
    @Column(name = "due_date")
    private LocalDate dueDate;
    
    @CreatedDate
    @Column(name = "created_at", updatable = false)
    private LocalDateTime createdAt;
    
    @LastModifiedDate
    @Column(name = "updated_at")
    private LocalDateTime updatedAt;
    
    // Constructors
    protected Task() {} // JPA requires no-arg constructor
    
    public Task(String title) {
        this.title = title;
    }
    
    // Getters and Setters
    public Long getId() {
        return id;
    }
    
    public String getTitle() {
        return title;
    }
    
    public void setTitle(String title) {
        this.title = title;
    }
    
    // equals, hashCode, toString
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Task)) return false;
        Task task = (Task) o;
        return Objects.equals(id, task.id);
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}
```

## Testing

### Unit Tests (JUnit 5)
```java
@ExtendWith(MockitoExtension.class)
class TaskServiceTest {
    
    @Mock
    private TaskRepository taskRepository;
    
    @InjectMocks
    private TaskService taskService;
    
    @Test
    void shouldCreateTask() {
        // Given
        Task task = new Task("Test Task");
        when(taskRepository.save(any(Task.class))).thenReturn(task);
        
        // When
        Task result = taskService.create(task);
        
        // Then
        assertNotNull(result);
        assertEquals("Test Task", result.getTitle());
        verify(taskRepository).save(task);
    }
    
    @Test
    void shouldThrowExceptionWhenTitleIsNull() {
        // Given
        Task task = new Task(null);
        
        // When & Then
        assertThrows(InvalidTaskException.class, () -> {
            taskService.create(task);
        });
    }
}
```

### Integration Tests
```java
@SpringBootTest
@AutoConfigureMockMvc
class TaskControllerIntegrationTest {
    
    @Autowired
    private MockMvc mockMvc;
    
    @Autowired
    private TaskRepository taskRepository;
    
    @BeforeEach
    void setUp() {
        taskRepository.deleteAll();
    }
    
    @Test
    void shouldCreateTask() throws Exception {
        String taskJson = """
            {
                "title": "Test Task",
                "description": "Test Description"
            }
            """;
        
        mockMvc.perform(post("/api/tasks")
                .contentType(MediaType.APPLICATION_JSON)
                .content(taskJson))
            .andExpect(status().isCreated())
            .andExpect(jsonPath("$.title").value("Test Task"));
    }
}
```

## Common Patterns

### Builder Pattern
```java
public class Task {
    private Long id;
    private String title;
    private String description;
    
    private Task(Builder builder) {
        this.title = builder.title;
        this.description = builder.description;
    }
    
    public static class Builder {
        private String title;
        private String description;
        
        public Builder title(String title) {
            this.title = title;
            return this;
        }
        
        public Builder description(String description) {
            this.description = description;
            return this;
        }
        
        public Task build() {
            return new Task(this);
        }
    }
}

// Usage
Task task = new Task.Builder()
    .title("Learn Java")
    .description("Study design patterns")
    .build();
```

## Best Practices

✅ Use final for immutable fields
✅ Prefer immutability when possible
✅ Use meaningful variable names
✅ Keep methods short (< 20 lines)
✅ Use proper access modifiers
✅ Document public APIs with Javadoc
✅ Handle exceptions appropriately
✅ Use Optional instead of null
✅ Leverage functional programming features
✅ Write unit tests

## Avoid These

❌ Catching generic Exception
❌ Empty catch blocks
❌ Returning null (use Optional)
❌ Magic numbers (use constants)
❌ God classes (too many responsibilities)
❌ Mutable static fields
❌ Nested ternary operators
❌ Complex conditionals (extract to methods)
❌ Premature optimization
