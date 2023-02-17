# TypeScript

## Mô tả kiểu dữ liệu - Type Annotation

- Kiểu dữ liệu đã biết bên JS
  + **Primitive**: number, boolean, string, null, undefined, symbol.
  + **Reference**: array, object, function
- Còn với TS, ta sẽ có thêm: any, unknown, void, never, ...
```
const n: number = 42
const isMember: boolean = false
const username: string = 'kcjpop'
```
Mô tả kiểu cho tham số hàm:
```
function say(name: string) {
  return `Hello ${name}`
}

say('kcjpop') // → Hello kcjpop
say(42) // Error: Argument of type 'number' is not assignable to parameter of type 'string'.
```
Mô tả giá trị trả về của một hàm:
```
// Hàm trả về string
function getGreeting(name: string): string {
  return `Hello ${name}`
}

// Hàm trả về number
function double(x: number): number {
  return x + x
}
```
Đối với các hàm không trả về kết quả nào, bạn có thể dùng `void`.
```
// Thật ra hàm này trả về `undefined`
function printGreeting(name: string): void {
  console.log(`Hello ${name}`)
}
```
`never` dành cho hàm không bao giờ trả về kết quả.
```
function doSomething(message: string): never {
  throw new Error(message)
}
```
### Mảng và tuple
Với **mảng (array)**, ta dùng cú pháp `type[]` hoặc `Array<type>`. Các phần tử trong mảng phải có cùng 1 kiểu.
```
const evens: number[] = [0, 2, 4, 6, 8]

// Khai báo hàm nhận vào một mảng chuỗi
function joinWithComma(arr: string[]) {
  return arr.join(', ')
}
```
Với **tuple**, ta có thể cho phép mảng chứa các phần tử có các kiểu dữ liệu khác nhau.
```
const pair: [string, number] = ['kcjpop', 123]
```
### Enum
Cho phép khai báo một tập hợp các biến không đổi (constant).
```
enum Direction {
  UP,
  DOWN,
  LEFT,
  RIGHT,
}

console.log(Direction.UP) // 0
console.log(Direction.DOWN) // 1
console.log(Direction.LEFT) // 2
console.log(Direction.RIGHT) // 3
```
Ta có thể thay đổi giá trị trong `enum`.
```
enum Direction {
  UP = 1,
  DOWN,
  LEFT = 6,
  RIGHT,
}

console.log(Direction.UP) // 1
console.log(Direction.DOWN) // 2
console.log(Direction.LEFT) // 6
console.log(Direction.RIGHT) // 7
```
Ta có thế sử dụng chuỗi làm giá trị cho enum, nhưng trong trường hợp này phải gán giá trị cho tất cả lựa chọn trong chuỗ `enum`.
```
enum Direction {
  UP = 'Up',
  DOWN = 'Down',
  LEFT = 'Left',
  RIGHT = 'Right',
}
```
### Union type
Cho phép kết hợp hai hay nhiều kiểu dữ liệu lại với nhau.
```
function printId(id: string | number) {
  console.log(`Your ID is ${id}`)
}
```
### Type alias
Đặt tên cho các kiểu dữ liệu bằng từ khóa `type`
```
type Username = string;

type User = {
  readonly id: number; // not allow updating value of a prop
  name: Username;
  role?: string; // optional
}
```
### Interface
Là 1 cách để khai báo kiểu cho object.
```
interface User = {
  id: string | number;
  name: string;
  role?: string;
}

function printUser(user: User) {
  console.log(`Hello ${user.name}`)
}
```

## Generics
Là kiểu dữ liệu mà có nhận tham số và trả về kiểu dữ liệu tương ứng.
```
interface Student {
  id: number;
  name: string;
}

const numberList: Array<number> = [1, 2, 3];
const wordList: Array<number> = ['one', 'two];

const studentList: Array<Student> = [
  {id: 1, name: 'One'},
  {id: 2, name: 'Two'},
];
```
```
function createPair<S, T>(v1: S, v2: T): [S, T] {
  return [v1, v2];
}
console.log(createPair<string, number>('hello', 42)); // ['hello', 42]
```
```
type Wrapped<T> = { value: T };

const wrappedValue: Wrapped<number> = { value: 10 };
```

## Utility Types
### `Partial<Type>`
Chuyển tất cả props của `Type` thành `optional`.
```
interface Point {
  x: number;
  y: number;
}

let pointPart: Partial<Point> = {};
```

### `Required<Type>`
Chuyển tất cả props của `Type` thành `required`.
```
interface Car {
  make: string;
  model: string;
  mileage?: number;
}

let myCar: Required<Car> = {
  make: 'Ford',
  model: 'Focus',
  mileage: 12000 // `Required` forces mileage to be defined
};
```

### `Readonly<Type>
Chuyển tất cả props của `Type` thành `readonly`, không update được.

### `Record<Keys, Type>`
Giúp tạo ra 1 object type với `key: Keys` và `value: Type`.
```
const nameAgeMap: Record<string, number> = {
  'Alice': 21,
  'Bob': 25
};
```

### `Pick<Type, Keys>`
Tạo ra 1 kiểu dữ liệu mới bằng cách lấy một vài props `Keys` từ kiểu `Type`.
```
interface Todo {
  title: string;
  description: string;
  completed: boolean;
}
 
type TodoPreview = Pick<Todo, "title" | "completed">;
 
const todo: TodoPreview = {
  title: "Clean room",
  completed: false,
};
```

### `Omit<Type, Keys>`
Tạo ra 1 kiểu dữ liệu mới bằng cách lấy tất cả props từ `Type` trừ `Keys`.
```
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Omit<Person, 'age' | 'location'> = {
  name: 'Bob'
};
```

### `ReturnType<Type>`
Lấy kiểu dữ liệu trả về của 1 function.
```
type PointGenerator = () => { x: number; y: number; };
const point: ReturnType<PointGenerator> = {
  x: 10,
  y: 20
};
```

### `Exclude<UnionType, ExcludeMembers>`
Lấy về kiểu dữ liệu từ `UnionType` mà đã loại bỏ các `ExcludeMembers`.
```
type T1 = Exclude<"a" | "b" | "c", "a" | "b">;  // type T1 = "c"

type T2 = Exclude<string | number | (() => void), Function>;  //type T2 = string | number
```

### `Extract<Type, Union>`
Lấy về kiểu dữ liệu từ `Type` mà có chứa các `Union`.
```
type T0 = Extract<"a" | "b" | "c", "a" | "f" | "b">; // type T0 = "a' | "b" 

type T1 = Extract<string | number | (() => void), Function>; // type T1 = () => void
```








