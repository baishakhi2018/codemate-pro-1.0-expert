---
applyTo: "**/*.component.ts,**/*.service.ts,**/*.module.ts,**/*.directive.ts,**/*.pipe.ts"
---

# Angular Development Instructions

## Component Standards

### Component Decorator
```typescript
@Component({
  selector: 'app-task-list',
  templateUrl: './task-list.component.html',
  styleUrls: ['./task-list.component.scss'],
  changeDetection: ChangeDetectionStrategy.OnPush // Use OnPush when possible
})
```

### Component Class
```typescript
export class TaskListComponent implements OnInit, OnDestroy {
  // 1. Input/Output properties
  @Input() tasks: Task[] = [];
  @Output() taskDeleted = new EventEmitter<string>();
  
  // 2. Public properties
  filteredTasks: Task[] = [];
  
  // 3. Private properties
  private destroy$ = new Subject<void>();
  
  // 4. Constructor with dependency injection
  constructor(
    private taskService: TaskService,
    private cdr: ChangeDetectorRef
  ) {}
  
  // 5. Lifecycle hooks
  ngOnInit(): void {
    this.loadTasks();
  }
  
  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
  
  // 6. Public methods
  deleteTask(id: string): void {
    this.taskDeleted.emit(id);
  }
  
  // 7. Private methods
  private loadTasks(): void {
    // Implementation
  }
}
```

## TypeScript & Typing

### Strict Typing
- Enable strict mode in tsconfig.json
- Never use `any` - use `unknown` if type is unclear
- Use interfaces for data models
- Use type guards for runtime type checking

### Access Modifiers
```typescript
public publicMethod(): void {}      // Accessible everywhere
protected protectedMethod(): void {} // Accessible in class and subclasses
private privateMethod(): void {}    // Only accessible in class
```

## Dependency Injection

### Service Injection
```typescript
constructor(
  private taskService: TaskService,
  private router: Router,
  @Optional() private config?: AppConfig
) {}
```

### Provider Configuration
```typescript
// Module level
providers: [TaskService]

// Component level (creates new instance)
@Component({
  providers: [TaskService]
})

// Injectable decorator
@Injectable({
  providedIn: 'root' // Singleton service
})
```

## Services

### Service Structure
```typescript
@Injectable({
  providedIn: 'root'
})
export class TaskService {
  private tasksSubject = new BehaviorSubject<Task[]>([]);
  public tasks$ = this.tasksSubject.asObservable();
  
  constructor(private http: HttpClient) {}
  
  getTasks(): Observable<Task[]> {
    return this.http.get<Task[]>('/api/tasks');
  }
  
  addTask(task: Task): Observable<Task> {
    return this.http.post<Task>('/api/tasks', task);
  }
}
```

### HTTP Best Practices
- Use HttpClient for all HTTP requests
- Handle errors with catchError
- Use proper TypeScript typing for responses
- Implement retry logic for failed requests

## RxJS & Observables

### Subscription Management
```typescript
// Use takeUntil pattern
private destroy$ = new Subject<void>();

ngOnInit(): void {
  this.taskService.tasks$
    .pipe(takeUntil(this.destroy$))
    .subscribe(tasks => {
      this.tasks = tasks;
    });
}

ngOnDestroy(): void {
  this.destroy$.next();
  this.destroy$.complete();
}
```

### Async Pipe Preference
```typescript
// Prefer async pipe in template (auto-unsubscribes)
tasks$ = this.taskService.getTasks();

// Template:
// <div *ngFor="let task of tasks$ | async">
```

### Common Operators
```typescript
// Transformation
map(value => value * 2)
filter(value => value > 10)
tap(value => console.log(value))

// Combination
combineLatest([obs1$, obs2$])
merge(obs1$, obs2$)
switchMap(value => this.service.getData(value))

// Error handling
catchError(error => of(defaultValue))
retry(3)
```

## Template Syntax

### Data Binding
```typescript
// Interpolation
{{ title }}

// Property binding
[disabled]="isDisabled"
[attr.aria-label]="ariaLabel"
[class.active]="isActive"
[style.color]="textColor"

// Event binding
(click)="handleClick($event)"
(submit)="handleSubmit()"

// Two-way binding
[(ngModel)]="username"
```

### Structural Directives
```typescript
// *ngIf
<div *ngIf="isVisible; else elseBlock">Content</div>
<ng-template #elseBlock>Alternative</ng-template>

// *ngFor
<div *ngFor="let item of items; trackBy: trackById; let i = index">
  {{ i }}: {{ item.name }}
</div>

// trackBy function
trackById(index: number, item: Task): string {
  return item.id;
}
```

## Forms

### Reactive Forms (Preferred)
```typescript
import { FormBuilder, FormGroup, Validators } from '@angular/forms';

export class TaskFormComponent implements OnInit {
  taskForm!: FormGroup;
  
  constructor(private fb: FormBuilder) {}
  
  ngOnInit(): void {
    this.taskForm = this.fb.group({
      title: ['', [Validators.required, Validators.maxLength(100)]],
      description: [''],
      dueDate: [null, Validators.required]
    });
  }
  
  onSubmit(): void {
    if (this.taskForm.valid) {
      const task = this.taskForm.value;
      // Process task
    }
  }
}
```

### Custom Validators
```typescript
function dateRangeValidator(control: AbstractControl): ValidationErrors | null {
  const start = control.get('startDate')?.value;
  const end = control.get('endDate')?.value;
  
  if (start && end && start > end) {
    return { dateRange: true };
  }
  return null;
}
```

## Change Detection

### OnPush Strategy
```typescript
@Component({
  changeDetection: ChangeDetectionStrategy.OnPush
})
export class OptimizedComponent {
  // Use immutable data patterns
  // Trigger change detection manually when needed
  constructor(private cdr: ChangeDetectorRef) {}
  
  updateData(): void {
    // Update data
    this.cdr.markForCheck();
  }
}
```

## Module Organization

### Feature Module
```typescript
@NgModule({
  declarations: [
    TaskListComponent,
    TaskItemComponent,
    TaskFormComponent
  ],
  imports: [
    CommonModule,
    ReactiveFormsModule,
    TaskRoutingModule
  ],
  exports: [
    TaskListComponent
  ]
})
export class TaskModule {}
```

### Shared Module
```typescript
@NgModule({
  declarations: [
    LoadingSpinnerComponent,
    ErrorMessageComponent
  ],
  imports: [
    CommonModule
  ],
  exports: [
    CommonModule,
    LoadingSpinnerComponent,
    ErrorMessageComponent
  ]
})
export class SharedModule {}
```

## Routing

### Route Configuration
```typescript
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'tasks', component: TaskListComponent },
  { path: 'tasks/:id', component: TaskDetailComponent },
  { 
    path: 'admin', 
    loadChildren: () => import('./admin/admin.module').then(m => m.AdminModule),
    canActivate: [AuthGuard]
  },
  { path: '**', component: NotFoundComponent }
];
```

### Route Guards
```typescript
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {
  constructor(private authService: AuthService, private router: Router) {}
  
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean> {
    return this.authService.isAuthenticated$.pipe(
      map(isAuth => {
        if (!isAuth) {
          this.router.navigate(['/login']);
          return false;
        }
        return true;
      })
    );
  }
}
```

## Pipes

### Custom Pipe
```typescript
@Pipe({
  name: 'truncate'
})
export class TruncatePipe implements PipeTransform {
  transform(value: string, length: number = 20): string {
    if (!value) return '';
    return value.length > length 
      ? value.substring(0, length) + '...' 
      : value;
  }
}
```

## Directives

### Custom Directive
```typescript
@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  @Input() appHighlight = 'yellow';
  
  constructor(private el: ElementRef) {}
  
  @HostListener('mouseenter') onMouseEnter(): void {
    this.highlight(this.appHighlight);
  }
  
  @HostListener('mouseleave') onMouseLeave(): void {
    this.highlight('');
  }
  
  private highlight(color: string): void {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

## Testing

### Component Testing
```typescript
describe('TaskListComponent', () => {
  let component: TaskListComponent;
  let fixture: ComponentFixture<TaskListComponent>;
  
  beforeEach(async () => {
    await TestBed.configureTestingModule({
      declarations: [ TaskListComponent ],
      imports: [ HttpClientTestingModule ],
      providers: [ TaskService ]
    }).compileComponents();
    
    fixture = TestBed.createComponent(TaskListComponent);
    component = fixture.componentInstance;
  });
  
  it('should create', () => {
    expect(component).toBeTruthy();
  });
  
  it('should load tasks on init', () => {
    component.ngOnInit();
    expect(component.tasks.length).toBeGreaterThan(0);
  });
});
```

## Naming Conventions

- Components: `task-list.component.ts`
- Services: `task.service.ts`
- Pipes: `truncate.pipe.ts`
- Directives: `highlight.directive.ts`
- Modules: `task.module.ts`
- Guards: `auth.guard.ts`

## Best Practices

✅ Use OnPush change detection when possible
✅ Unsubscribe from observables (use async pipe or takeUntil)
✅ Use reactive forms over template-driven forms
✅ Lazy load feature modules
✅ Use strict TypeScript settings
✅ Implement proper error handling
✅ Use trackBy with *ngFor
✅ Keep components focused and small
✅ Use services for business logic
✅ Implement proper accessibility (ARIA)

## Avoid These

❌ Subscribing without unsubscribing
❌ Logic in templates
❌ Mutating @Input() properties
❌ Using any type
❌ Nested subscriptions
❌ Missing trackBy in *ngFor
❌ Default change detection for complex components
❌ Business logic in components
