# Create Skill: Component Generation

## Learning Objectives
- Build a skill that generates React components
- Include TypeScript, tests, and Storybook stories
- Customize the skill for your project's patterns

## Why Component Generation?

Every React project has dozens of components. Each one needs:
- The component file
- TypeScript props interface
- Test file
- Storybook story
- CSS module or styled components

Doing this manually is tedious. A component generation skill automates the entire process.

## The Skill Definition

Create a file at `.github/copilot/skills/component-generation.md`:

```markdown
# Component Generation Skill

## Description
Generate React components following project conventions with TypeScript, tests, and Storybook stories.

## Trigger
When user asks to create a new component, generate a component, or add a UI element.

## Instructions

### Step 1: Gather Requirements
Ask the user for:
- Component name (PascalCase)
- Component type (page, layout, feature, ui)
- Props needed
- Whether it needs state management
- Styling approach (CSS modules, Tailwind, styled-components)

### Step 2: Check Existing Patterns
Before generating:
1. Look at existing components in the same directory
2. Identify the import style (named vs default exports)
3. Check the testing patterns used
4. Note the styling approach

### Step 3: Generate Files
Create the following files:

#### Component File (ComponentName.tsx)
```tsx
import React from 'react';
import styles from './ComponentName.module.css';

interface ComponentNameProps {
  // Define props here
}

export const ComponentName: React.FC<ComponentNameProps> = ({ ...props }) => {
  return (
    <div className={styles.container}>
      {/* Component content */}
    </div>
  );
};
```

#### Test File (ComponentName.test.tsx)
```tsx
import { render, screen } from '@testing-library/react';
import { ComponentName } from './ComponentName';

describe('ComponentName', () => {
  it('renders correctly', () => {
    render(<ComponentName />);
    // Add assertions
  });
});
```

#### Story File (ComponentName.stories.tsx)
```tsx
import type { Meta, StoryObj } from '@storybook/react';
import { ComponentName } from './ComponentName';

const meta: Meta<typeof ComponentName> = {
  title: 'Components/ComponentName',
  component: ComponentName,
};

export default meta;
type Story = StoryObj<typeof ComponentName>;

export const Default: Story = {
  args: {
    // Default props
  },
};
```

### Step 4: Style File
Create ComponentName.module.css with basic styles.

## Constraints
- Maximum 200 lines per component
- Use TypeScript for all files
- Include accessibility attributes (aria-labels, roles)
- Follow project naming conventions
- Export components as named exports

## Examples
See existing components in src/components/ for patterns.
```

## Using the Skill

When you say "Create a UserCard component that displays user avatar, name, and email", the skill:

1. Creates `UserCard.tsx` with props interface
2. Creates `UserCard.test.tsx` with basic tests
3. Creates `UserCard.stories.tsx` for Storybook
4. Creates `UserCard.module.css` with styles

## Customizing for Your Project

### Add Project-Specific Patterns

```markdown
## Project Patterns
- All components use the `cn()` utility for class merging
- Props interfaces extend `BaseComponentProps`
- Tests use the custom `renderWithProviders` helper
- Stories include accessibility addon
```

### Add Validation Rules

```markdown
## Validation
- Component names must be PascalCase
- Files must be in kebab-case
- Maximum one component per file
- Props must have JSDoc comments
```

## Advanced: Component Variants

```markdown
## Variants
When generating components, support these variants:

### Button
- Primary, Secondary, Ghost, Danger
- Sizes: sm, md, lg
- States: loading, disabled

### Input
- Text, Email, Password, Number
- With label, without label
- With error state
```

## AI Prompt for Component Skill

```
Create a component generation skill for a React project that:
1. Generates component, test, story, and style files
2. Follows TypeScript best practices
3. Includes accessibility attributes
4. Uses the project's existing patterns
5. Supports component variants
6. Validates naming conventions

Output the skill as a markdown file ready to use.
```

## Practice Exercise

Create a component generation skill for your project:
1. Define the skill trigger and instructions
2. Include templates for component, test, and story files
3. Add project-specific patterns and constraints
4. Test the skill by generating 3 different components
5. Refine the skill based on the results

## Key Takeaways

- Component generation skills automate repetitive boilerplate
- Include all related files (component, test, story, styles)
- Customize skills to match your project's patterns
- Skills improve consistency across the codebase
