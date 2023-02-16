# STYLES

## SCSS
SCSS là 1 chương trình tiền xử lý CSS (CSS preprocessor), giúp viết CSS theo cách của 1 ngôn ngữ lập trình, có cấu trúc rõ ràng, dễ phát triển và bảo trì code. SCSS có phần mở rộng là .scss, ra đời sau SASS, có cú pháp viết tương tự như cách viết CSS.

Các tính năng cơ bản của SCSS:
- **Xếp chồng - Nested Rules**
```
nav {
  ul {
    margin: 0;
    padding: 0;
    list-style: none;
  }

  li { display: inline-block; }

  a {
    display: block;
    padding: 6px 12px;
    text-decoration: none;
  }
}
```
- **Biến - Variables**: Ta có thể đặt tên cho biến bắt đầu bằng `$`. Biến sẽ chứa các giá trị mà ta sử dụng nhiều lần như mã màu, font hay kiểu chữ.
```
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}
```
- **Mixins**: Tạo hàm sử dụng trong SCSS, có thể truyền các tham số vào bên trong nó để sử dụng.
```
@mixin theme($theme: DarkGray) {
  background: $theme;
  box-shadow: 0 0 1px rgba($theme, .25);
  color: #fff;
}

.info {
  @include theme;
}
.alert {
  @include theme($theme: DarkRed);
}
.success {
  @include theme($theme: DarkGreen);
}
```
- **Kế thừa - Extend**
```
.title-box {
    color: ##2EFEC8;
    text-shadow: 0px 0px 10px #6E6E6E;
    display: inline-block;
    text-transform: uppercase;
}

.navbar {
    ul.menu {
        list-style: none;

        li {
            padding: 4px;

            a {
                text-decoration: none;
                @extend .title-box;
            }
        }
    }
}
```
- **Import**: Không cần phải viết tất cả SCSS trong cùng 1 file. Ta có thể chia ra thành các module và sử dụng chúng bằng cú pháp `@import`.
```
// _base.scss
$font-stack: Helvetica, sans-serif;
$primary-color: #333;

body {
  font: 100% $font-stack;
  color: $primary-color;
}

// styles.scss
@use 'base';

.inverse {
  background-color: base.$primary-color;
  color: white;
}
```

## CSS-in-JS
`CSS-in-JS` là 1 thuật ngữ mô tả việc viết code style CSS trong code JS, tức là viết vào file `.js, .jsx` thay vì viết code CSS vào file .css như bình thường.

`CSS-in-JS` sẽ trích xuất CSS theo từng component thay vì theo document như CSS.

Khi `CSS-in-JS` này được phân tích cú pháp, CSS được tạo và được đính kèm vào DOM ngay. Chức năng này được triển khai bởi các thư viện bên thứ 3, ví dụ như `styled-components`:
```
import styled from 'styled-components'; 
const Text = styled.h2` 
color: #212121, 
` 

<Text>Hello CSS-in-JS</Text>
```
Trong browser, đoạn code này sẽ attach với DOM như sau:
```
<style>
.gZxhj123 {
    color: #212121;
}
</style>
```
Ưu điểm của `CSS-in-JS`:
- Code ngắn gọn, nhất quán
- Phạm vi CSS cục bộ (phạm vi trong component), từ đó tránh ghi đề CSS giữa các component
- Tận dụng được các kĩ thuật, syntax JS
- Có thể tái sử dụng
- Có thể viết unit test cho CSS

## Styled-components
- `Styled-components` được tạo ra hoàn toàn giống các component bình thường, sẽ có `children`, `props` truyền vào, chỉ khác 1 điểm là chúng ta được viết style bên trong nó.
- `Styled-components` hỗ trợ cú pháp tương tự cú pháp `SCSS` để tự động xử lý các style lồng nhau.
```
const StyledButton = styled.button`
  background: ${(props) => (props.primary ? "teal" : "white")};
  color: ${(props) => (props.primary ? "white" : "teal")};

  font-size: 2em;
  margin: 0.5em;
  padding: 0.25em 1em;
  border: 2px solid teal;
  border-radius: 3px;

  &:hover {
    opacity: 0.8;
    cursor: pointer;
  }
`;

const Container = styled.div`
  display: flex;
  width: 100vw;
  height: 100vh;
  align-items: center;
  justify-content: center;
  flex-direction: column;

  ${StyledButton} {
    filter: grayscale(0.8);
  }
`;

function App() {
  return (
    <Container>
      <StyledButton>Styled component without props</StyledButton>
      <StyledButton primary>Styled component with props</StyledButton>
    </Container>
  );
}
```
