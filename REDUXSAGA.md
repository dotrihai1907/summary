# Redux Saga

## Khái niệm

### Middleware
Middleware là 1 lớp trung gian nằm giữa Dispatch Action và Reducers. Nó sẽ được gọi trước khi 1 action được dispatch đến Reducers.

Middleware trong Redux Saga được sử dụng để giải quyết các vấn đề liên quan đến xử lý tác vụ bất đồng bộ, như gọi API, xử lý dữ liệu từ các yêu cầu mạng, hoặc thực hiện các tác vụ đòi hỏi xử lý non-blocking. Từ đó giúp đảm bảo rằng tất cả các tác vụ được thực hiện một cách đồng bộ và đảm bảo tính nhất quán của trạng thái của ứng dụng.

### Side Effect
Side Effect có thể hiểu đơn giản là một action nào đó thực hiên một công việc tốn thời gian mà ta không định lượng được hoặc không care được, thí dụ: Đọc dữ liệu từ ổ cứng, gọi API lấy dữ liệu, ...

### Generator function
Generator function là một loại hàm đặc biệt trong JavaScript, nó có thể bị tạm dừng (pause) ở một vị trí bất kỳ trong hàm bằng cách sử dụng từ khoá `yield`. Sau khi bị tạm dừng, generator function có thể được tiếp tục thực thi từ vị trí bị dừng bằng cách gọi đến phương thức `next()`.

Chính chức năng này giúp ta giải quyết được việc bất đồng bộ, hàm sẽ dừng và đợi async chạy xong rồi tiếp tục thực thi.

### Redux Saga
Redux-saga là một middleware cho Redux, cho phép xử lý các side effect như gọi API, xử lý bất đồng bộ khi load data, ... một cách dễ dàng. Nó sử dụng generator function để xử lý các side effect.

## Cách hoạt động của Redux Saga

![image](https://user-images.githubusercontent.com/102176213/219431901-92109d90-caf8-4a29-8471-a8773fb9c4b6.png)
- Đối với logic của saga, ta cung cấp một hàm cho saga, chính hàm này là hàm đứng ra xem xét các action trước khi vào store, nếu là action nó quan tâm thì hàm sẽ được thực thi.
- 1 function trong saga là 1 `generator function` (gọi là saga function) có dạng:
```
function* simpleSagaFunction() {
  yield console.log('hello world');
}
```
- Điểm quan trọng của `redux-saga` chính là `yield`. Đây là một `helper` vô cùng hữu ích. Khi cần một thao tác nào đó tốn thời gian thì việc đồng bộ luồng hoạt động là vô cùng cần thiết, `yield` giúp ta ta xử lý vấn đề đó. Thực chất ở đây, `yield` sẽ cho dừng không chạy đoạn lệnh tiếp theo cho đến khi `next()` được gọi.

## Một số helper trong Redux Saga

- `takeEvery()`: thực thi và trả lại kết quả của mọi actions được gọi.
- `takeLatest()`: nếu chúng ta thực hiện một loạt các actions, nó sẽ chỉ thực thi và trả lại kết quả của actions cuối cùng.
- `take()`: tạm dừng cho đến khi nhận được action.
- `put()`: dispatch 1 action.
- `call()`: gọi function. Nếu nó return về 1 Promise, tạm dừng saga cho đến khi Promise đó được giải quyết.
- `race()`: chạy nhiều effects đồng thời, sau đó hủy tất cả nếu 1 trong số đó kết thúc.
- `fork()`: sử dụng cơ chế non - blocking call trên function.
- `yield()`: chạy tuần tự khi nào trả ra kết quả thì mới thực thi tiếp.
- `select()`: giúp chạy 1 selector function để lấy data từ state có trước

## Cách gọi 1 generator function trong 1 generator function khác

Các function trong redux-saga thường được thiết kế để lồng nhau, có nghĩa là một function có thể gọi một hoặc nhiều function khác và chờ đợi kết quả trả về từ chúng.

Để gọi một generator function trong một generator function khác, chúng ta có thể sử dụng từ khoá `yield`.

```
import { call, put, takeEvery } from 'redux-saga/effects';
import { fetchDataSuccess, fetchDataError } from './actions';
import { FETCH_DATA_REQUEST } from './constants';
import api from './api';

function* fetchData(action) {
  try {
    const data = yield call(api.fetchData, action.payload);
    yield put(fetchDataSuccess(data));
  } catch (error) {
    yield put(fetchDataError(error.message));
  }
}

function* watchFetchData() {
  yield takeEvery(FETCH_DATA_REQUEST, fetchData);
}

export default function* rootSaga() {
  yield watchFetchData();
}
```
Trong ví dụ trên, chúng ta có 3 generator function: `fetchData`, `watchFetchData` và `rootSaga`.
-  `fetchData` là function để gọi API và cập nhật state của ứng dụng.
-  `watchFetchData` là function để lắng nghe các action có type là `FETCH_DATA_REQUEST` và gọi `fetchData` khi nhận được action này.
-  `rootSaga` là function để kết nối tất cả các generator function lại với nhau.

Trong `fetchData`, chúng ta sử dụng `yield call` để gọi hàm `fetchData` từ api, sau đó sử dụng `yield put` để gọi action `fetchDataSuccess` hoặc `fetchDataError` nhằm cập nhật state của ứng dụng.

Trong `watchFetchData`, chúng ta sử dụng `yield takeEvery` để lắng nghe tất cả action có type là `FETCH_DATA_REQUEST`, khi nhận được action này, chúng ta sử dụng `yield call` để gọi function `fetchData`.

Cuối cùng, trong `rootSaga`, chúng ta sử dụng `yield` để gọi `watchFetchData`.

Tóm lại, để sử dụng generator function trong redux-saga, chúng ta có thể sử dụng các từ khoá yield, call, put, takeEvery, ... để xử lý các side effect và gọi các function lồng nhau.
