# JSFuck: code javascript với chỉ 6 ký tự `[]+()!`

Javascript là một ngôn ngữ lập trình kịch bản phía client, do đó mà code sẽ được chạy thông qua các file scripts (plaintext chứ không phải binary) và chạy hoàn toàn ở dưới máy client. Điều này tiềm ẩn một rủi ro đó là APIs và resources của bạn hoàn toàn có thể bị một ai đó đánh cắp. Đáng buồn là chúng ta **không thể** che giấu code đi được mà chỉ thể làm cho nó khó đọc hơn. JSFuck là một trong những phương pháp ra đời để thực hiện nhiệm vụ này.

### JSFuck là gì?
JSFuck là một style lập trình rất khó hiểu dựa trên những thành phần cốt lõi của javascript. Nó chỉ sử dụng 6 ký tự để viết và chạy code.

```javascript
[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]][([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+([][[]]+[])[+!+[]]+(![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[+!+[]]+([][[]]+[])[+[]]+([][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[+!+[]+[+[]]]+(!![]+[])[+!+[]]]((![]+[])[+!+[]]+(![]+[])[!+[]+!+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]+(!![]+[])[+[]]+(![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]]+[+!+[]]+(!![]+[][(![]+[])[+[]]+([![]]+[][[]])[+!+[]+[+[]]]+(![]+[])[!+[]+!+[]]+(!![]+[])[+[]]+(!![]+[])[!+[]+!+[]+!+[]]+(!![]+[])[+!+[]]])[!+[]+!+[]+[+[]]])()
```
Đoạn code trên tương đương với việc gọi hàm `alert(1)` trong javascript. Cũng dễ hiểu phết 😭.

Trước khi đi sâu tìm hiểu xem jsfuck hoạt động như thế nào, tôi xin nhắc lại một số kiến thức javascript cơ bản mà sẽ được jsfuck sử dụng.
### Javascript cơ bản
#### Ép kiểu (type conversion)
##### Ép kiểu rõ ràng (explicit conversion)
Javascript có thể sử dụng 2 toán tử `+` và `!` để ép kiểu rõ ràng:
* Toán tử `+` có thể ép kiểu cho toán hạng thành kiểu number.
* Toán tử `!` có thể ép kiểu cho toán hạng thành kiểu boolean.
```javascript
console.log(+"" typeof(+"")); // 0 number
console.log(+42, typeof(+42)); // 42 number
console.log(+true, typeof(+true)); // 1 number

console.log(!"", typeof(!"")); // true boolean
console.log(!42, typeof(!42)); // false boolean
console.log(!true, typeof(!true)); // false boolean
```
##### Ép kiểu ngầm (implicit conversion)
Có rất nhiều điều để nói về ép kiểu ngầm ở trong javascript. Tuy nhiên, với jsfuck, chúng ta chỉ cần hiểu tại sao:
* `[]+[]` là một string rỗng
* `+[]` bằng `0`
* `+{}` là `Nan`

Ba biểu hiện kỳ quặc này có được bởi sự nhập nhằng khi sử dụng toán tử `+`. Toán tử `+` có thể dùng để nối strings, cộng và ép kiểu.
Thực tế thì, `Array.prototype.toString()` và `Object.prototype.toString()` đã được gọi ngầm khi ép kiểu Array và Object về kiểu nguyên thủy. Vì vậy, đây là những gì thực tế xảy ra:
```javascript
console.log([].toString() + [].toString()); // ""
console.log([].toString(), +[].toString()); // "" 0
console.log({}.toString(), +{}.toString()); // [object Object] NaN
```
#### Giá trị truthy/falsy
Trong một biểu thức boolean, javascript chấp nhận các giá trị true/false, nhưng nó cũng đánh giá các giá trị truthy/falsy.
* Object, array, function v..v là truthy.
* `undefined, null, NaN, 0` luôn luôn là falsy.

```javascript
function truthyOrFalsy(arg) {
  arg ? console.log('truthy') : console.log('falsy');
}
truthyOrFalsy(''); // falsy
truthyOrFalsy('str'); // truthy
truthyOrFalsy(0); // falsy
truthyOrFalsy(1); // truthy
truthyOrFalsy([]); // truthy
truthyOrFalsy({}); // truthy
console.log(!'', !!''); // true false
console.log(!'str', !!'str'); // false true
console.log(!0, !!0); // true false
console.log(!1, !!1); // false true
console.log(![], !![]); // false true
console.log(!{}, !!{}); // false true
```
#### Ký hiệu ngoặc vuông
Trong javascript, tất cả các đối tượng đều có những thuộc tính và phương thức riêng. Chúng có thể được gọi đến bằng ký hiệu `.` hoặc ký hiệu `[]`. JSFuck sử dụng ký hiệu `[]`.
```javascript
console.log({foo: 'Foo'}.foo); // Foo
console.log({foo: 'Foo'}['foo']); // Foo
console.log([].length); // 0
console.log([]['length']); // 0
console.log(function fn() {}.name); // fn
console.log(function fn() {}['name']); // fn
```
#### Thứ tự ưu tiên và chiều thực hiện.
* Toán tử một ngôi (unary operator) có mức độ ưu tiên cao hơn toán tử hai ngôi (binary operator)
* Toán tử một ngôi thực hiện từ phải sang trái, toán thử hai ngôi thực hiện từ trái sang phải

Ví dụ:
```javascript
// Ép kiểu thực hiện từ phải sang trái
// Phép cộng thực hiện từ trái sang phải
// Ép kiểu được thực hiện trước phép cộng
console.log(+'41' + +true); // 42
```


### JSFuck hoạt động ntn?
Hàm **Function constructor** - `Function("code")` chính là ý tưởng chủ đạo của jsfuck: nó nhận vào một String làm tham số và trả về một hàm nặc danh (anonymous) trong đó chuỗi tham số đầu vào sẽ là phần thân của hàm này. Nói một cách khác, nó giúp bạn thực thi mọi code dưới dạng một String. Do đó, nếu như jsfuck có thể thực hiện được 2 việc sau:
* Encode mọi đoạn code javascript chỉ với 6 ký tự `[]+()!`.
* Gọi được đến hàm Function constructor.

thì jsfuck có thể thực thi mọi đoạn code javascript với chỉ 6 ký tự `[]+()!`.

####JSFuck basics

JSFuck giới thiệu các basics sau để thực hiện 2 nhiệm vụ trên ([list đầy đủ](https://github.com/aemkei/jsfuck/blob/master/jsfuck.js)):
* **false**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `![]`
* **true**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `!![]`
* **undefined** => `[][[]]`
* **NaN**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `+[![]]`
* **0**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `+[]`
* **1**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `+!+[]`
* **2**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `!+[]+!+[]`
* **10**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `[+!+[]]+[+[]]`
* **Array**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `[]`
* **Number**&nbsp;&nbsp;&nbsp;&nbsp;=> `+[]`
* **String**&nbsp;&nbsp;&nbsp;&nbsp;=> `[]+[]`
* **Boolean**&nbsp;&nbsp;&nbsp;=> `![]`
* **Function**&nbsp;&nbsp;=> `[]["filter"]`
* **eval**&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;=> `[]["filter"]["constructor"] (CODE)()`
* **window**&nbsp;&nbsp;&nbsp;&nbsp;=> `[]["filter"]["constructor"]("return this")()`

Với kiến thức đã được nhắc lại ở trên, chúng ta sẽ cùng đi giải thích những basics này.

**false** => `![]`
Array là một mảng. JS array là một đối tượng và đối tượng thì luôn luôn truthy. Do đó mà `!truthy = false`.

**undefined** => `[][[]]`
Mỗi array sẽ có các thuộc tính built-in. Ví dụ `length` là một thuộc tính hợp lệ của array. Do đó mà `[]["length"]` sẽ trả về `0`. Nhưng `[]` hay `""` không phải là một thuộc tính hợp lệ nên `[][""]` sẽ trả về `undefined`.

**NaN** => `+[![]]`
`![] = false`, `[false].toString() = "false"`  (ép kiểu ngầm) và do đó `+[![]] = NaN`

**1** => `+!+[]`
`!+[]` => `!0` => `true` nên `+!+[]` => `+true` => `1`

**Function** => `[]["filter"]`
`[]` là một array nên nó được phép gọi đến các phương thức trong `Array.prototype`. Do đó mà chúng ta có thể gọi tới `Array.prototype.filter` (một function).

**eval** => `[]["filter"]["constructor"] (CODE)()`
`[]["filter"]` là một function. Trong javascript, mỗi function là một thể hiện (instance) của Function do đó nó có thể gọi đến các thuộc tính của `Function.prototype`. `Function.prototype.constructor` là một trong các thuộc tính này. Thuộc tính `.constructor` sẽ trả về tham chiếu đến hàm mà tạo ra thể hiện đó. Do đó, `[]["filter"]["constructor"]` tương đương với `Function()` và sẽ trả về một phương thức mới. Chúng ta có thể làm một phép tính cộng đơn giản như sau:
```javascript
Function('a', 'b', 'return a + b')(3, 7) // returns 10
```
**window** => `[]["filter"]["constructor"]("return this")()`
Khi code không ở strict mode thì trong các phương thức thông thường `this` sẽ là `window`.

#### Áp dụng
Đọc đến đây chắc là các bạn cũng đã có thể mường tượng được cách thức jsfuck hoạt động rồi phải không.
* **eval** => `[]["filter"]["constructor"] (CODE)()` đây chính là cách mà jsfuck thực hiện yêu cầu thứ 2 như đã nêu ở phía trên.
* Yêu cầu thứ nhất được thực hiện theo cách sau: từ `![] = false` ta có thể thu được `"false"` bằng cách `![]+[]`. Và `"false"[0]` sẽ trả về `"f"`. Cùng với các làm như trên áp dụng với các basics trên ta có thể thu được các kí tự sau: `0, 1, 2, 3, 4, 5, 6, 7, 8, 9, f, a, l, s, e, t, r, u, n, d, i`.

Chi tiết có thể tham khảo [tại đây](https://github.com/aemkei/jsfuck)

### Puzzle
Giải mã đoạn code sau và comment kết quả ở bên dưới bài viết.
```javascript
(!![]+[])[!+[]+!+[]+!+[]]+([][[]]+[])[+!+[]]+([][[]]+[])[!+[]+!+[]]
```
### Tài liệu tham khảo
* [Understanding JSFuck](https://badacadabra.github.io/Understanding-JSFuck/)
* [JSFuck source code](https://github.com/aemkei/jsfuck)
