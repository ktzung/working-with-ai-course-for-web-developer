# Xây dựng Component Vue với AI

## Vue 3 với Composition API

Composition API Vue 3 với cú pháp `<script setup>` là cách hiện đại để xây component Vue. AI có thể tạo component Vue có cấu trúc tốt khi bạn chỉ định pattern đúng.

## Component Vue cơ bản

### Component Button

```
Tạo component nút Vue 3 dùng Composition API và TypeScript.

Component: UiButton
File: src/components/ui/UiButton.vue

Props:
- variant: 'primary' | 'secondary' | 'outline' | 'ghost' | 'danger'
- size: 'sm' | 'md' | 'lg'
- isLoading?: boolean
- isDisabled?: boolean
- leftIcon?: string
- rightIcon?: string

Tính năng:
- Slot cho nội dung nút
- Spinner loading
- Trạng thái hover và active
- Trạng thái disabled
- Hỗ trợ dark mode

Dùng <script setup> với defineProps và defineEmits.
Dùng Tailwind CSS cho styling.
```

### Component Card

```
Tạo component card Vue 3 dùng Composition API.

Component: UiCard
File: src/components/ui/UiCard.vue

Sub-component:
- UiCard (wrapper)
- UiCardHeader (tiêu đề + slot hành động)
- UiCardBody (slot mặc định)
- UiCardFooter (slot hành động)

Props cho UiCard:
- padding: 'sm' | 'md' | 'lg'
- hoverable: boolean

Tính năng:
- Hiệu ứng shadow khi hover
- Slot đặt tên cho header, mặc định, footer
- Hỗ trợ dark mode

Dùng <script setup> với defineProps.
Dùng Tailwind CSS cho styling.
```

## Component Vue phức tạp

### Tìm kiếm với Autocomplete

```
Tạo component tìm kiếm Vue 3 với autocomplete.

Component: SearchAutocomplete
File: src/components/ui/SearchAutocomplete.vue

Props:
- placeholder: string
- suggestions: SearchResult[]
- isLoading: boolean

Emits:
- search: (query: string) => void
- select: (item: SearchResult) => void

Tính năng:
- Input debounce (300ms)
- Dropdown gợi ý
- Điều hướng bàn phím (lên, xuống, enter, escape)
- Trạng thái loading
- Nút xóa
- Highlight chữ khớp

Interface SearchResult:
interface SearchResult {
  id: string;
  title: string;
  description?: string;
}

Dùng <script setup> với defineProps, defineEmits, ref, computed, watch.
Dùng Tailwind CSS cho styling.
```

### Bảng Kanban

```
Tạo component bảng Kanban Vue 3.

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

Tính năng:
- Kéo thả giữa các cột
- Thêm task mới inline
- Card task với badge ưu tiên
- Đếm task mỗi cột
- Responsive (xếp chồng trên mobile)

Dùng @vueuse/core cho kéo thả.
Dùng Pinia cho quản lý state.
Dùng Tailwind CSS cho styling.
```

## Pattern riêng Vue

### Composable (Hook Vue)

```
Tạo composable Vue 3 để fetch dự án.

Composable: useProjects
File: src/composables/useProjects.ts

Trả về:
- projects: Ref<Project[]>
- isLoading: Ref<boolean>
- error: Ref<Error | null>
- fetchProjects: () => Promise<void>
- createProject: (data: CreateProjectInput) => Promise<Project>

Tính năng:
- State reactive với ref
- Xử lý lỗi
- Trạng thái loading
- Abort controller để cleanup
- Cache kết quả

Dùng hàm Composition API (ref, reactive, computed, watch).
```

### Pinia Store

```
Tạo Pinia store cho quản lý dự án.

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

Dùng defineStore của Pinia với setup syntax.
Bao gồm kiểu TypeScript cho tất cả state và actions.
```

## Best Practice Component Vue

### Props và Emits
```
Luôn dùng defineProps và defineEmits:

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
Dùng template ref để truy cập DOM:

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

## Bài tập thực hành

Xây các component Vue sau cho WebDevHub:

1. **ProjectCard** — Card với thông tin dự án, badge tech, hành động
2. **SnippetEditor** — Trình chỉnh sửa code với syntax highlighting
3. **DashboardStats** — Thẻ thống kê với biểu đồ

Viết prompt riêng cho Vue, tạo code, sau đó review.

## Tiếp theo là gì?

Trong bài tiếp theo, chúng ta sẽ tập trung vào styling với Tailwind CSS — dùng AI để tạo thiết kế đẹp, responsive.
