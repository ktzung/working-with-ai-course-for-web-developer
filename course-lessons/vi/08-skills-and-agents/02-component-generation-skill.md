# Tạo kỹ năng: Tạo thành phần

## Mục tiêu học tập
- Xây dựng kỹ năng tạo thành phần React
- Bao gồm TypeScript, kiểm thử và câu chuyện Storybook
- Tùy chỉnh kỹ năng cho mẫu dự án của bạn

## Tại sao tạo thành phần?

Mọi dự án React đều có hàng tá thành phần. Mỗi thành phần cần:
- Tệp thành phần
- Interface props TypeScript
- Tệp kiểm thử
- Câu chuyện Storybook
- CSS module hoặc styled components

Làm thủ công rất nhàm chán. Kỹ năng tạo thành phần tự động hóa toàn bộ quy trình.

## Định nghĩa kỹ năng

Tạo tệp tại `.github/copilot/skills/component-generation.md`:

```markdown
# Kỹ năng tạo thành phần

## Mô tả
Tạo thành phần React theo quy ước dự án với TypeScript, kiểm thử và câu chuyện Storybook.

## Trigger
Khi người dùng yêu cầu tạo thành phần mới, tạo thành phần hoặc thêm phần tử UI.

## Hướng dẫn

### Bước 1: Thu thập yêu cầu
Hỏi người dùng:
- Tên thành phần (PascalCase)
- Loại thành phần (trang, bố cục, tính năng, ui)
- Props cần có
- Có cần quản lý trạng thái không
- Cách tiếp cận样式 (CSS modules, Tailwind, styled-components)

### Bước 2: Kiểm tra mẫu hiện có
Trước khi tạo:
1. Xem các thành phần hiện có trong cùng thư mục
2. Xác định kiểu nhập (named vs default exports)
3. Kiểm tra mẫu kiểm thử được sử dụng
4. Ghi chú cách tiếp cận样式

### Bước 3: Tạo tệp
Tạo các tệp sau:

#### Tệp thành phần (ComponentName.tsx)
```tsx
import React from 'react';
import styles from './ComponentName.module.css';

interface ComponentNameProps {
  // Định nghĩa props ở đây
}

export const ComponentName: React.FC<ComponentNameProps> = ({ ...props }) => {
  return (
    <div className={styles.container}>
      {/* Nội dung thành phần */}
    </div>
  );
};
```

#### Tệp kiểm thử (ComponentName.test.tsx)
```tsx
import { render, screen } from '@testing-library/react';
import { ComponentName } from './ComponentName';

describe('ComponentName', () => {
  it('render đúng cách', () => {
    render(<ComponentName />);
    // Thêm assertions
  });
});
```

#### Tệp câu chuyện (ComponentName.stories.tsx)
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
    // Props mặc định
  },
};
```

### Bước 4: Tệp样式
Tạo ComponentName.module.css với样式 cơ bản.

## Ràng buộc
- Tối đa 200 dòng mỗi thành phần
- Sử dụng TypeScript cho tất cả tệp
- Bao gồm thuộc tính accessibility (aria-labels, roles)
- Theo dõi quy ước đặt tên dự án
- Xuất thành phần dưới dạng named exports

## Ví dụ
Xem các thành phần hiện có trong src/components/ để biết mẫu.
```

## Sử dụng kỹ năng

Khi bạn nói "Tạo thành phần UserCard显示 ảnh đại diện, tên và email của người dùng", kỹ năng:

1. Tạo `UserCard.tsx` với interface props
2. Tạo `UserCard.test.tsx` với kiểm thử cơ bản
3. Tạo `UserCard.stories.tsx` cho Storybook
4. Tạo `UserCard.module.css` với样式

## Tùy chỉnh cho dự án của bạn

### Thêm mẫu cụ thể cho dự án

```markdown
## Mẫu dự án
- Tất cả thành phần sử dụng tiện ích `cn()` để合并 class
- Interface props kế thừa `BaseComponentProps`
- Kiểm thử sử dụng trợ giúp tùy chỉnh `renderWithProviders`
- Câu chuyện bao gồm addon accessibility
```

### Thêm quy tắc xác thực

```markdown
## Xác thực
- Tên thành phần phải là PascalCase
- Tệp phải ở kebab-case
- Tối đa một thành phần mỗi tệp
- Props phải có bình luận JSDoc
```

## Nâng cao: Biến thể thành phần

```markdown
## Biến thể
Khi tạo thành phần, hỗ trợ các biến thể sau:

### Button
- Primary, Secondary, Ghost, Danger
- Kích thước: sm, md, lg
- Trạng thái: loading, disabled

### Input
- Text, Email, Password, Number
- Có nhãn, không nhãn
- Có trạng thái lỗi
```

## Prompt AI cho kỹ năng thành phần

```
Tạo kỹ năng tạo thành phần cho dự án React:
1. Tạo thành phần, kiểm thử, câu chuyện và tệp样式
2. Theo dõi thực hành tốt nhất TypeScript
3. Bao gồm thuộc tính accessibility
4. Sử dụng mẫu hiện có của dự án
5. Hỗ trợ biến thể thành phần
6. Xác thực quy ước đặt tên

Xuất kỹ năng dưới dạng tệp markdown sẵn sàng sử dụng.
```

## Bài tập thực hành

Tạo kỹ năng tạo thành phần cho dự án của bạn:
1. Định nghĩa trigger và hướng dẫn kỹ năng
2. Bao gồm mẫu cho thành phần, kiểm thử và tệp câu chuyện
3. Thêm mẫu và ràng buộc cụ thể cho dự án
4. Kiểm thử kỹ năng bằng cách tạo 3 thành phần khác nhau
5. Tinh chỉnh kỹ năng dựa trên kết quả

## Điểm chính

- Kỹ năng tạo thành phần tự động hóa mẫu lặp đi lặp lại
- Bao gồm tất cả tệp liên quan (thành phần, kiểm thử, câu chuyện,样式)
- Tùy chỉnh kỹ năng để khớp mẫu dự án của bạn
- Kỹ năng cải thiện tính nhất quán trên codebase
