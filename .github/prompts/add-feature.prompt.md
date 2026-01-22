---
title: Add New Feature
description: Guide for adding a new feature to the task manager
---

# Add New Feature Prompt

When adding a new feature to the task manager, follow this process:

1. **Define Requirements**
   - What is the feature?
   - What user problem does it solve?
   - What are the acceptance criteria?

2. **Plan Implementation**
   - What components need to be created/modified?
   - What new types/interfaces are needed?
   - What state management changes are required?

3. **Create Types First**
   - Define all TypeScript interfaces and types
   - Ensure type safety across the feature

4. **Implement UI Components**
   - Create new components following project structure
   - Use Tailwind CSS for styling
   - Ensure responsive design

5. **Add Business Logic**
   - Implement hooks if needed
   - Add utility functions
   - Handle edge cases and errors

6. **Update Storage Layer**
   - Modify localStorage interactions if needed
   - Ensure data persistence works correctly

7. **Test Manually**
   - Test happy path
   - Test edge cases
   - Test error scenarios
   - Test on mobile viewport

8. **Code Review Checklist**
   - Follows TypeScript guidelines?
   - Components under 200 lines?
   - Proper error handling?
   - Accessible markup?
   - Responsive design?

## Example Feature Request

"Add a feature to filter tasks by completion status"

Expected output:
- FilterBar component
- Filter state management
- Filtered task list display
- Toggle buttons for All/Active/Completed
