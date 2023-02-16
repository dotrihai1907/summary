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
- **Biến - Variables**: Ta có thể đặt tên cho biến bắt đầu bằng $. Biến sẽ chứa các giá trị mà ta sử dụng nhiều lần như mã màu, font hay kiểu chữ.
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
- **Import**: Không cần phải viết tất cả SCSS trong cùng 1 file. Ta có thể chia ra thành các module và sử dụng chúng bằng cú pháp @import.
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
