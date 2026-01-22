---
applyTo: "**/*.py"
---

# Python Development Instructions

## Code Style (PEP 8)

### Naming Conventions
```python
# Modules: lowercase with underscores
import task_manager

# Classes: PascalCase
class TaskManager:
    pass

# Functions/Methods: snake_case
def create_task():
    pass

# Variables: snake_case
task_title = "Learn Python"

# Constants: UPPER_SNAKE_CASE
MAX_TASKS = 100

# Private: prefix with single underscore
def _internal_helper():
    pass

# Protected: prefix with single underscore
class Task:
    def __init__(self):
        self._protected_attr = None
```

### File Structure
```python
#!/usr/bin/env python3
"""
Module docstring describing the module's purpose.

This module provides task management functionality.
"""

# 1. Standard library imports
import os
import sys
from datetime import datetime
from typing import List, Optional, Dict

# 2. Third-party imports
import requests
from pydantic import BaseModel

# 3. Local imports
from .models import Task
from .utils import validate_task

# 4. Constants
MAX_RETRIES = 3
DEFAULT_TIMEOUT = 30

# 5. Module-level variables
logger = logging.getLogger(__name__)

# 6. Classes and functions
class TaskManager:
    """Manages task operations."""
    
    def __init__(self):
        """Initialize the task manager."""
        self.tasks: List[Task] = []
    
    def add_task(self, task: Task) -> None:
        """
        Add a task to the manager.
        
        Args:
            task: The task to add
            
        Raises:
            ValueError: If task is invalid
        """
        self.tasks.append(task)


def main():
    """Main entry point."""
    pass


# 7. Module execution
if __name__ == "__main__":
    main()
```

## Type Hints

### Always Use Type Hints
```python
from typing import List, Dict, Optional, Union, Tuple, Any

# Function annotations
def get_task(task_id: int) -> Optional[Task]:
    """Get a task by ID."""
    return task_repository.find(task_id)

# Multiple return types
def process_task(task: Task) -> Union[str, int]:
    """Process a task and return result."""
    if task.is_valid():
        return task.id
    return "Invalid task"

# Collections
def get_tasks() -> List[Task]:
    """Get all tasks."""
    return []

def get_task_map() -> Dict[int, Task]:
    """Get tasks as a mapping."""
    return {}

# Tuples
def get_task_info() -> Tuple[str, int, bool]:
    """Get task information."""
    return ("Task", 1, True)

# Callable
from typing import Callable

def execute_callback(callback: Callable[[Task], None]) -> None:
    """Execute a callback function."""
    task = Task()
    callback(task)

# Generic types
from typing import TypeVar, Generic

T = TypeVar('T')

class Container(Generic[T]):
    def __init__(self, value: T) -> None:
        self.value = value
    
    def get(self) -> T:
        return self.value
```

## Classes & Objects

### Class Design
```python
from dataclasses import dataclass
from typing import Optional
from datetime import datetime

# Use dataclasses for data containers
@dataclass
class Task:
    """Represents a task."""
    
    id: int
    title: str
    completed: bool = False
    created_at: datetime = datetime.now()
    
    def __post_init__(self):
        """Validate after initialization."""
        if not self.title:
            raise ValueError("Title cannot be empty")
    
    def mark_complete(self) -> None:
        """Mark the task as completed."""
        self.completed = True


# Regular class with properties
class TaskManager:
    """Manages tasks with business logic."""
    
    def __init__(self) -> None:
        """Initialize the task manager."""
        self._tasks: List[Task] = []
    
    @property
    def task_count(self) -> int:
        """Get the number of tasks."""
        return len(self._tasks)
    
    @property
    def completed_tasks(self) -> List[Task]:
        """Get completed tasks."""
        return [task for task in self._tasks if task.completed]
    
    def add_task(self, task: Task) -> None:
        """
        Add a task.
        
        Args:
            task: Task to add
            
        Raises:
            ValueError: If task is invalid
        """
        if not isinstance(task, Task):
            raise ValueError("Invalid task type")
        self._tasks.append(task)
    
    def get_task(self, task_id: int) -> Optional[Task]:
        """
        Get a task by ID.
        
        Args:
            task_id: The task ID
            
        Returns:
            The task if found, None otherwise
        """
        return next((t for t in self._tasks if t.id == task_id), None)
    
    def __len__(self) -> int:
        """Return the number of tasks."""
        return len(self._tasks)
    
    def __repr__(self) -> str:
        """Return string representation."""
        return f"TaskManager(tasks={len(self._tasks)})"
```

### Inheritance & Composition
```python
from abc import ABC, abstractmethod

# Abstract base class
class BaseRepository(ABC):
    """Base repository interface."""
    
    @abstractmethod
    def save(self, entity: Any) -> None:
        """Save an entity."""
        pass
    
    @abstractmethod
    def find(self, id: int) -> Optional[Any]:
        """Find an entity by ID."""
        pass


# Concrete implementation
class TaskRepository(BaseRepository):
    """Repository for tasks."""
    
    def __init__(self) -> None:
        self._tasks: Dict[int, Task] = {}
    
    def save(self, task: Task) -> None:
        """Save a task."""
        self._tasks[task.id] = task
    
    def find(self, task_id: int) -> Optional[Task]:
        """Find a task by ID."""
        return self._tasks.get(task_id)


# Prefer composition over inheritance
class TaskService:
    """Service layer for task operations."""
    
    def __init__(self, repository: TaskRepository) -> None:
        """
        Initialize with dependencies.
        
        Args:
            repository: The task repository
        """
        self._repository = repository
        self._validator = TaskValidator()
    
    def create_task(self, title: str) -> Task:
        """
        Create and save a new task.
        
        Args:
            title: Task title
            
        Returns:
            The created task
            
        Raises:
            ValueError: If validation fails
        """
        task = Task(id=1, title=title)
        self._validator.validate(task)
        self._repository.save(task)
        return task
```

## Error Handling

### Exception Hierarchy
```python
class TaskError(Exception):
    """Base exception for task errors."""
    pass


class TaskNotFoundError(TaskError):
    """Raised when a task is not found."""
    
    def __init__(self, task_id: int) -> None:
        super().__init__(f"Task not found: {task_id}")
        self.task_id = task_id


class InvalidTaskError(TaskError):
    """Raised when a task is invalid."""
    pass
```

### Try-Except Best Practices
```python
# Specific exceptions first
try:
    task = task_repository.find(task_id)
    if task is None:
        raise TaskNotFoundError(task_id)
    process_task(task)
except TaskNotFoundError as e:
    logger.error(f"Task not found: {e.task_id}")
    raise
except ValueError as e:
    logger.error(f"Invalid value: {e}")
    raise
except Exception as e:
    logger.exception("Unexpected error")
    raise

# Context managers for resource management
from contextlib import contextmanager

@contextmanager
def task_transaction():
    """Context manager for task transactions."""
    try:
        begin_transaction()
        yield
        commit_transaction()
    except Exception:
        rollback_transaction()
        raise

# Usage
with task_transaction():
    create_task(title="Test")
```

## Functions & Decorators

### Function Best Practices
```python
def calculate_completion_rate(tasks: List[Task]) -> float:
    """
    Calculate the completion rate of tasks.
    
    Args:
        tasks: List of tasks to analyze
        
    Returns:
        Completion rate as a percentage (0-100)
        
    Raises:
        ValueError: If tasks list is empty
        
    Examples:
        >>> tasks = [Task(1, "A", True), Task(2, "B", False)]
        >>> calculate_completion_rate(tasks)
        50.0
    """
    if not tasks:
        raise ValueError("Tasks list cannot be empty")
    
    completed = sum(1 for task in tasks if task.completed)
    return (completed / len(tasks)) * 100


# Use *args and **kwargs appropriately
def create_tasks(*titles: str, **metadata: Any) -> List[Task]:
    """
    Create multiple tasks with metadata.
    
    Args:
        *titles: Task titles
        **metadata: Additional metadata for all tasks
        
    Returns:
        List of created tasks
    """
    return [
        Task(id=i, title=title, **metadata)
        for i, title in enumerate(titles, 1)
    ]
```

### Common Decorators
```python
from functools import wraps
from time import time
import logging

# Timing decorator
def timer(func):
    """Measure function execution time."""
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time()
        result = func(*args, **kwargs)
        end = time()
        logger.info(f"{func.__name__} took {end - start:.2f}s")
        return result
    return wrapper


# Retry decorator
def retry(max_attempts: int = 3):
    """Retry a function on failure."""
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            for attempt in range(max_attempts):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    if attempt == max_attempts - 1:
                        raise
                    logger.warning(f"Attempt {attempt + 1} failed: {e}")
            return None
        return wrapper
    return decorator


# Usage
@timer
@retry(max_attempts=3)
def fetch_tasks() -> List[Task]:
    """Fetch tasks from API."""
    return api.get_tasks()
```

## Comprehensions & Generators

### List Comprehensions
```python
# Basic comprehension
completed_tasks = [task for task in tasks if task.completed]

# With transformation
task_titles = [task.title.upper() for task in tasks]

# Nested comprehension (use sparingly)
task_pairs = [
    (task1, task2)
    for task1 in tasks1
    for task2 in tasks2
    if task1.id != task2.id
]

# Dictionary comprehension
task_map = {task.id: task for task in tasks}

# Set comprehension
unique_titles = {task.title for task in tasks}
```

### Generator Expressions
```python
# Generator for memory efficiency
def get_large_dataset():
    """Yield tasks one at a time."""
    for i in range(1000000):
        yield Task(id=i, title=f"Task {i}")

# Generator expression
completed_ids = (task.id for task in tasks if task.completed)

# Generator function
def process_tasks_in_batches(tasks: List[Task], batch_size: int):
    """Process tasks in batches."""
    for i in range(0, len(tasks), batch_size):
        yield tasks[i:i + batch_size]

# Usage
for batch in process_tasks_in_batches(tasks, batch_size=100):
    process_batch(batch)
```

## Context Managers

### Using Context Managers
```python
# File operations
with open('tasks.json', 'r') as f:
    data = json.load(f)

# Multiple context managers
with open('input.txt', 'r') as infile, \
     open('output.txt', 'w') as outfile:
    content = infile.read()
    outfile.write(content)

# Custom context manager
class TaskSession:
    """Context manager for task operations."""
    
    def __enter__(self):
        """Enter the context."""
        self.start_time = time()
        logger.info("Starting task session")
        return self
    
    def __exit__(self, exc_type, exc_val, exc_tb):
        """Exit the context."""
        duration = time() - self.start_time
        logger.info(f"Task session completed in {duration:.2f}s")
        return False  # Don't suppress exceptions

# Usage
with TaskSession() as session:
    process_tasks()
```

## Async/Await

### Async Functions
```python
import asyncio
from typing import List

async def fetch_task(task_id: int) -> Task:
    """Asynchronously fetch a task."""
    await asyncio.sleep(0.1)  # Simulate API call
    return Task(id=task_id, title=f"Task {task_id}")


async def fetch_all_tasks(task_ids: List[int]) -> List[Task]:
    """Fetch multiple tasks concurrently."""
    tasks = [fetch_task(task_id) for task_id in task_ids]
    return await asyncio.gather(*tasks)


# Run async code
async def main():
    """Main async function."""
    task_ids = [1, 2, 3, 4, 5]
    tasks = await fetch_all_tasks(task_ids)
    print(f"Fetched {len(tasks)} tasks")


if __name__ == "__main__":
    asyncio.run(main())
```

## Testing (pytest)

### Unit Tests
```python
import pytest
from unittest.mock import Mock, patch

def test_create_task():
    """Test task creation."""
    task = Task(id=1, title="Test Task")
    assert task.id == 1
    assert task.title == "Test Task"
    assert not task.completed


def test_mark_complete():
    """Test marking task as complete."""
    task = Task(id=1, title="Test Task")
    task.mark_complete()
    assert task.completed


def test_invalid_task_raises_error():
    """Test that invalid task raises ValueError."""
    with pytest.raises(ValueError, match="Title cannot be empty"):
        Task(id=1, title="")


@pytest.fixture
def task_manager():
    """Fixture providing a task manager."""
    return TaskManager()


def test_add_task(task_manager):
    """Test adding a task."""
    task = Task(id=1, title="Test")
    task_manager.add_task(task)
    assert task_manager.task_count == 1


@patch('task_manager.api.fetch_tasks')
def test_fetch_tasks_from_api(mock_fetch):
    """Test fetching tasks from API."""
    mock_fetch.return_value = [Task(id=1, title="Test")]
    tasks = fetch_tasks_from_api()
    assert len(tasks) == 1
    mock_fetch.assert_called_once()
```

## FastAPI Best Practices

### Router Structure
```python
from fastapi import APIRouter, HTTPException, Depends
from typing import List
from pydantic import BaseModel

router = APIRouter(prefix="/api/tasks", tags=["tasks"])


class TaskCreate(BaseModel):
    """Schema for creating a task."""
    title: str
    description: Optional[str] = None


class TaskResponse(BaseModel):
    """Schema for task response."""
    id: int
    title: str
    completed: bool
    
    class Config:
        from_attributes = True


@router.get("/", response_model=List[TaskResponse])
async def get_tasks(
    skip: int = 0,
    limit: int = 100,
    task_service: TaskService = Depends(get_task_service)
) -> List[Task]:
    """
    Get all tasks.
    
    Args:
        skip: Number of tasks to skip
        limit: Maximum number of tasks to return
        task_service: Injected task service
        
    Returns:
        List of tasks
    """
    return task_service.get_tasks(skip=skip, limit=limit)


@router.get("/{task_id}", response_model=TaskResponse)
async def get_task(
    task_id: int,
    task_service: TaskService = Depends(get_task_service)
) -> Task:
    """Get a task by ID."""
    task = task_service.get_task(task_id)
    if not task:
        raise HTTPException(status_code=404, detail="Task not found")
    return task


@router.post("/", response_model=TaskResponse, status_code=201)
async def create_task(
    task_data: TaskCreate,
    task_service: TaskService = Depends(get_task_service)
) -> Task:
    """Create a new task."""
    return task_service.create_task(task_data)


@router.delete("/{task_id}", status_code=204)
async def delete_task(
    task_id: int,
    task_service: TaskService = Depends(get_task_service)
) -> None:
    """Delete a task."""
    task_service.delete_task(task_id)
```

## Best Practices

✅ Use type hints everywhere
✅ Write docstrings (Google or NumPy style)
✅ Keep functions small and focused
✅ Use dataclasses for data containers
✅ Prefer composition over inheritance
✅ Use context managers for resources
✅ Handle exceptions specifically
✅ Use list comprehensions appropriately
✅ Write unit tests with pytest
✅ Follow PEP 8 style guide

## Avoid These

❌ Bare except clauses
❌ Mutable default arguments
❌ Global variables
❌ Star imports (from module import *)
❌ Nested functions more than 2 levels
❌ Long functions (>50 lines)
❌ Complex list comprehensions
❌ Ignoring type hints
❌ Missing docstrings on public APIs
❌ Not using virtual environments
