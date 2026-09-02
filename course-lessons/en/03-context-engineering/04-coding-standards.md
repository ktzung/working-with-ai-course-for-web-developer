# Coding Standards as Context

## Enforcing Consistency with AI

Coding standards ensure that AI-generated code looks like it was written by your team. When you share your standards as context, AI will follow your naming conventions, formatting rules, and architectural patterns.

## ESLint Configuration

ESLint enforces code quality rules. Share your config with AI:

```json
// .eslintrc.json
{
  "extends": [
    "next/core-web-vitals",
    "eslint:recommended",
    "@typescript-eslint/recommended",
    "prettier"
  ],
  "plugins": ["@typescript-eslint", "react", "react-hooks"],
  "rules": {
    // TypeScript rules
    "@typescript-eslint/no-unused-vars": ["error", { "argsIgnorePattern": "^_" }],
    "@typescript-eslint/explicit-function-return-type": "warn",
    "@typescript-eslint/no-explicit-any": "error",
    "@typescript-eslint/prefer-const": "error",
    
    // React rules
    "react/prop-types": "off",
    "react/react-in-jsx-scope": "off",
    "react-hooks/rules-of-hooks": "error",
    "react-hooks/exhaustive-deps": "warn",
    
    // General rules
    "no-console": ["warn", { "allow": ["warn", "error"] }],
    "prefer-template": "error",
    "no-var": "error",
    "eqeqeq": ["error", "always"]
  }
}
```

### Using ESLint as Context

```
My ESLint config enforces these rules:
- No explicit any types
- Explicit function return types
- Prefer const
- No unused variables (except prefixed with _)
- Prefer template literals
- Strict equality (===)

Generate code that follows these rules.
```

## Prettier Configuration

Prettier ensures consistent formatting:

```json
// .prettierrc
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "lf"
}
```

### Using Prettier as Context

```
My Prettier config:
- Semicolons: yes
- Single quotes: yes
- Trailing commas: ES5 style
- Print width: 80 characters
- Tab width: 2 spaces

Format all generated code to match these settings.
```

## Naming Conventions

### File Naming

```
Naming conventions for this project:

Files:
- Components: PascalCase (UserProfile.tsx, ProjectCard.tsx)
- Hooks: camelCase with 'use' prefix (useProjects.ts, useAuth.ts)
- Utils: camelCase (formatDate.ts, validateEmail.ts)
- Types: PascalCase (Project.ts, User.ts)
- Constants: camelCase (config.ts, constants.ts)
- API routes: route.ts (Next.js convention)
- Tests: {name}.test.ts or {name}.spec.ts

Directories:
- Components: kebab-case (user-profile/, project-card/)
- Features: kebab-case (authentication/, project-management/)
```

### Variable Naming

```
Variable naming conventions:

- Variables: camelCase (userName, projectList)
- Constants: UPPER_SNAKE_CASE (API_BASE_URL, MAX_RETRY_COUNT)
- Types/Interfaces: PascalCase (UserProfile, ProjectStatus)
- Enums: PascalCase name, PascalCase members (TaskStatus.InProgress)
- Boolean variables: has/is/should prefix (isLoading, hasError, shouldRefresh)
- Functions: camelCase, verb-first (getUserData, formatDate, validateInput)
- Event handlers: handle prefix (handleSubmit, handleClick)
- React components: PascalCase (UserCard, ProjectList)
```

## Component Patterns

### Standard Component Template

```typescript
// Standard component pattern for this project

import { type FC } from 'react';
import { type Project } from '@/types/Project';
import { Card } from '@/components/ui/Card';
import { Badge } from '@/components/ui/Badge';

interface ProjectCardProps {
  project: Project;
  onSelect?: (id: string) => void;
  className?: string;
}

export const ProjectCard: FC<ProjectCardProps> = ({
  project,
  onSelect,
  className,
}) => {
  const handleClick = () => {
    onSelect?.(project.id);
  };

  return (
    <Card className={className} onClick={handleClick}>
      <Card.Header>
        <h3>{project.name}</h3>
        <Badge variant={project.status}>{project.status}</Badge>
      </Card.Header>
      <Card.Body>
        <p>{project.description}</p>
        <div className="flex gap-2">
          {project.techStack.map((tech) => (
            <Badge key={tech} variant="outline">{tech}</Badge>
          ))}
        </div>
      </Card.Body>
    </Card>
  );
};
```

### Standard Hook Pattern

```typescript
// Standard hook pattern for this project

import { useState, useEffect, useCallback } from 'react';
import { type Project } from '@/types/Project';
import { projectService } from '@/services/projectService';

interface UseProjectsOptions {
  page?: number;
  limit?: number;
  search?: string;
}

interface UseProjectsResult {
  projects: Project[];
  total: number;
  isLoading: boolean;
  error: Error | null;
  refetch: () => void;
}

export const useProjects = (options: UseProjectsOptions = {}): UseProjectsResult => {
  const { page = 1, limit = 10, search } = options;
  
  const [projects, setProjects] = useState<Project[]>([]);
  const [total, setTotal] = useState(0);
  const [isLoading, setIsLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);

  const fetchProjects = useCallback(async () => {
    setIsLoading(true);
    setError(null);
    
    try {
      const response = await projectService.getProjects({ page, limit, search });
      setProjects(response.data);
      setTotal(response.pagination.total);
    } catch (err) {
      setError(err instanceof Error ? err : new Error('Failed to fetch projects'));
    } finally {
      setIsLoading(false);
    }
  }, [page, limit, search]);

  useEffect(() => {
    fetchProjects();
  }, [fetchProjects]);

  return { projects, total, isLoading, error, refetch: fetchProjects };
};
```

## Import Organization

```
Import order for this project:

1. React and Next.js imports
2. Third-party libraries
3. Internal components (ui, layout, features)
4. Hooks
5. Services
6. Types
7. Utils
8. Styles

Example:
import { useState, useEffect } from 'react';
import { useRouter } from 'next/navigation';

import { z } from 'zod';
import { format } from 'date-fns';

import { Button } from '@/components/ui/Button';
import { Card } from '@/components/ui/Card';
import { ProjectCard } from '@/components/features/projects/ProjectCard';

import { useProjects } from '@/hooks/useProjects';

import { projectService } from '@/services/projectService';

import { type Project } from '@/types/Project';

import { formatDate } from '@/utils/formatDate';

import styles from './ProjectList.module.css';
```

## Error Handling Patterns

```
Error handling conventions:

1. API errors: Use try/catch with typed error responses
2. Component errors: Use Error Boundaries
3. Form errors: Use Zod validation with user-friendly messages
4. Network errors: Show retry option
5. Auth errors: Redirect to login

Standard error response:
interface ApiError {
  code: string;
  message: string;
  details?: { field: string; message: string }[];
}
```

## Testing Patterns

```
Testing conventions:

1. Test file location: Same directory as source, named {name}.test.ts
2. Test structure: Arrange, Act, Assert
3. Mock external dependencies
4. Test both success and error cases
5. Use descriptive test names

Example:
describe('ProjectCard', () => {
  it('renders project name and description', () => {
    // Arrange
    const project = { id: '1', name: 'Test', description: 'Desc' };
    
    // Act
    render(<ProjectCard project={project} />);
    
    // Assert
    expect(screen.getByText('Test')).toBeInTheDocument();
    expect(screen.getByText('Desc')).toBeInTheDocument();
  });
});
```

## Creating a Standards Document

Combine all standards into a single document:

```markdown
# Coding Standards

## Quick Reference
- TypeScript strict mode
- Functional components with hooks
- Named exports only
- PascalCase components, camelCase functions
- Single quotes, semicolons, trailing commas
- Mobile-first responsive design
- Error boundaries for component errors
- Zod for validation

## Detailed Rules
[Include full ESLint, Prettier, and pattern documentation]
```

## Practice Exercise

1. Set up ESLint and Prettier for your project
2. Create a coding standards document
3. Generate a component using AI with your standards as context
4. Verify the generated code follows all standards

## What's Next

In the next lesson, we'll create the current-task.md file — a focused context document for what you're building right now.
