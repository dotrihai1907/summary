# Atomic Design
Theo Atomic Design, các component sẽ được chia làm 5 loại khác nhau: atoms, molecules, organisms, templates và pages.

Việc phân chia component sẽ giúp dễ dàng quản lí component của dự án 1 cách có hệ thống.

Cách chia component theo Atomic Design cho feature LoginPage:
- `Atoms`: các thành phần nhỏ nhất.
  + Input: dùng để nhập thông tin (username, password)
  + Button: dùng để tạo nút nhấn (đăng nhập)v
  + Checkbox: dùng để tạo các ô chọn (nhớ mật khẩu)
- `Molecules`: kết hợp các atoms thành 1 thành phần lớn hơn.
  + LoginForm: component bao gồm các atoms như Input, Button để tạo thành 1 form đăng nhập đầy đủ.
- `Organisms`: kết hợp các molecules và atoms thành 1 thành phần lớn hơn.
  + LoginCard: component bao gồm molecule LoginForm và atom Checkbox để tạo thành 1 card đăng nhập đầy đủ.
- `Templates`: nhóm các organisms thành 1 phần của trang.
  + LoginPageTemplate: component bao gồm organisms LoginCard và Header, tạo thành 1 trang đăng nhập đầy đủ.
- `Pages`: chứa các templates và hiển thị cho người dùng.
  + LoginPage: component chứa template LoginPageTemplate, hiển thị cho người dùng trang đăng nhập.

Về phần CSS và logic, ta có thể thể tổ chức theo cách mà mình cảm thấy phù hợp nhất.
- CSS: viết ở phần nào cũng được.
- logic: nên viết ở những tầng cao, để các thành phần tầng bên dưới vẫn có thể tái sử dụng được.