# Building Vue Components with AI

## Vue 3 with Composition API

Vue 3's Composition API with `<script setup>` syntax is the modern way to build Vue components. AI can generate well-structured Vue components when you specify the right patterns.

## Basic Vue Components

### Button Component

```
Create a Vue 3 button component using Composition API and TypeScript.

Component: UiButton
File: src/components/ui/UiButton.vue

Props:
- variant: 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger'
- size: 'sm' | 'md' | 'lg'
- isLoading?: boolean
- isDisabled?: boolean
- leftIcon?: string
- rightIcon?: string

Features:
- Slot for button content
- Loading spinner
- Hover and active states
- Disabled state
- Dark mode support

Use <script setup> with defineProps and defineEmits.
Use Tailwind CSS for styling.
```

### Card Component

```
Create a Vue 3 card component using Composition API.

Component: UiCard
File: src/components/ui/UiCard.vue

Sub-components:
- UiCard (wrapper)
- UiCardHeader (title + actions slot)
- UiCardBody (default slot)
- UiCardFooter (actions slot)

Props for UiCard:
- padding: 'sm' | 'md' | 'lg'
- hoverable: boolean

Features:
- Hover shadow effect
- Named slots for header, default, footer
- Dark mode support

Use <script setup> with defineProps.
Use Tailwind CSS for styling.
```

## Complex Vue Components

### Search with Autocomplete

```
Create a Vue 3 search component with autocomplete.

Component: SearchAutocomplete
File: src/components/ui/SearchAutocomplete.vue

Props:
- placeholder: string
- suggestions: SearchResult[]
- isLoading: boolean

Emits:
- search: (query: string) => void
- select: (item: SearchResult) => void

Features:
- Debounced input (300ms)
- Dropdown suggestions
- Keyboard navigation (up, down, enter, escape)
- Loading state
- Clear button
- Highlight matching text

SearchResult interface:
interface SearchResult {
  id: string;
  title: string;
  description?: string;
}

Use <script setup> with defineProps, defineEmits, ref, computed, watch.
Use Tailwind CSS for styling.
```

### Kanban Board

```
Create a Vue 3 Kanban board component.

Component: KanbanBoard
File: src/components/dashboard/KanbanBoard.vue

Props:
- columns: KanbanColumn[]
- onTaskMove: (taskId: string, fromColumn: string, toColumn: string) => void

Types:
interface KanbanColumn {
  id: string;
  title: string;
  tasks: KanbanTask[];
}

interface KanbanTask {
  id: string;
  title: string;
  description?: string;
  priority: 'low' | 'medium' | 'high';
  assignee?: { name: string; avatar: string };
}

Features:
- Drag and drop between columns
- Add new task inline
- Task cards with priority badges
- Column task counts
- Responsive (stacked on mobile)

Use @vueuse/core for drag and drop.
Use Pinia for state management.
Use Tailwind CSS for styling.
```

## Vue-Specific Patterns

### Composables (Vue Hooks)

```
Create a Vue 3 composable for fetching projects.

Composable: useProjects
File: src/composables/useProjects.ts

Returns:
- projects: Ref<Project[]>
- isLoading: Ref<boolean>
- error: Ref<Error | null>
- fetchProjects: () => Promise<void>
- createProject: (data: CreateProjectInput) => Promise<Project>

Features:
- Reactive state with ref
- Error handling
- Loading state
- Abort controller for cleanup
- Cache results

Use Composition API functions (ref, reactive, computed, watch).
```

### Pinia Store

```
Create a Pinia store for project management.

Store: useProjectStore
File: src/stores/projectStore.ts

State:
- projects: Project[]
- currentProject: Project | null
- isLoading: boolean
- error: string | null

Getters:
- activeProjects: Project[]
- projectsByStatus: Record<string, Project[]>

Actions:
- fetchProjects(): Promise<void>
- fetchProject(id: string): Promise<void>
- createProject(data: CreateProjectInput): Promise<Project>
- updateProject(id: string, data: UpdateProjectInput): Promise<Project>
- deleteProject(id: string): Promise<void>

Use Pinia's defineStore with setup syntax.
Include TypeScript types for all state and actions.
```

## Vue Component Best Practices

### Props and Emits
```
Always use defineProps and defineEmits:

<script setup lang="ts">
interface Props {
  title: string;
  count?: number;
}

interface Emits {
  (e: 'update', value: string): void;
  (e: 'delete', id: string): void;
}

const props = withDefaults(defineProps<Props>(), {
  count: 0,
});

const emit = defineEmits<Emits>();
</script>
```

### Template Refs
```
Use template refs for DOM access:

<script setup lang="ts">
import { ref, onMounted } from 'vue';

const inputRef = ref<HTMLInputElement | null>(null);

onMounted(() => {
  inputRef.value?.focus();
});
</script>

<template>
  <input ref="inputRef" />
</template>
```

## Practice Exercise

Build these Vue components for WebDevHub:

1. **ProjectCard** — Card with project info, tech badges, actions
2. **SnippetEditor** — Code editor with syntax highlighting
3. **DashboardStats** — Statistics cards with charts

Write Vue-specific prompts, generate the code, then review.

## What's Next

In the next lesson, we'll focus on styling with Tailwind CSS — using AI to create beautiful, responsive designs.
