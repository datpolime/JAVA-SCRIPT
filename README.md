# JAVA-SCRIPT

SERIES JAVASCRIPT SIDA – SỰ BÁ ĐẠO CỦA ASYNC/AWAIT TRONG JS
10/10/2017 PHẠM HUY HOÀNG 11 COMMENTS
Lâu rồi chưa viết bài về kĩ thuật nên hôm nay viết một bài để dân tình biết là mình vẫn còn code nhé!

Kì này, chúng ta sẽ tìm hiểu về async/await, một cặp từ khoá rất bá đạo trong JavaScript chuẩn ES2017. Biết cách dùng async/await sẽ giúp ta viết code ngắn gọn, hiệu quả và dễ dàng hơn nhiều nhé.


Nhắc lại kiến thức
JavaScript là ngôn ngữ single-thread, tức là chỉ có một thread duy nhất để thực thi các dòng lệnh. Nếu chạy theo cơ chế đồng bộ (synchonous) thì khi thực hiện tính toán phức tạp, gọi AJAX request tới server, gọi database (trong NodeJS), thread này sẽ dừng để chờ, làm toàn bộ trình duyệt bị… treo.

Để tránh điều này, hầu hết code gọi AJAX request hoặc database trong JavaScript đều chạy theo cơ chế bất đồng bộ (asynchnous). Ban đầu, việc chạy code asynchnous trong JavaScript được hiện thực nhờ callback (như đoạn code bên dưới).

// Truyền callback vào hàm ajax
var callback =  function(image){
  console.log(image);
};
ajax.get("gaixinh.info", callback);

// Có thể viết gọn như sau
ajax.get("gaixinh.info", function(image) {
  console.log(image);
})
view rawajax_callback.js hosted with ❤ by GitHub
Tất nhiên, vì callback có một số nhược điểm như code dài dòng, callback hell,… nên người ta tạo ra 1 pattern mới gọi là Promise!

Các bạn nên xem lại kiến thức về Callback trong JavaScript và Promise trong JavaScript để có thể hiểu kiến thức phía dưới bài này nhé!

Từ callback, promise đến Async/Await
Promise đã giải quyết khá tốt những vấn đề của callback. Code trở nên dễ đọc, tách biệt và dễ bắt lỗi hơn.


Code trở nên gọn đẹp khi chuyển từ callback qua promise.
Tuy nhiên, dùng promise đôi khi ta vẫn thấy hơi khó chịu vì phải truyền callback vào hàm then và catch. Code cũng sẽ hơi dư thừa và khó debug, vì toàn bộ các hàm then chỉ được tính là 1 câu lệnh nên không debug riêng từng dòng được.

May thay, trong ES7 một phép màu mang tên async/await đã ra đời. (Mình nghi 99% là phép màu này ăn cắp từ C# hay ho, vì C# đã có async/await từ thời ông địa cởi trường rồi cơ).


Xem object Promise như thể chúng là đối tượng đồng bộ
Vậy async/await có gì hay ho? Chúng giúp chúng ta viết code trông có vẻ đồng bộ (synchonous), nhưng thật ra lại chạy bất đồng bộ (asynchonous). 

Như trong hình phía trên, hàm findRandomImgPromise là hàm bất đồng bộ, trả về một Promise. Với từ khoá await, ta có thể coi hàm này là đồng bộ, câu lệnh phía sau chỉ được chạy sau khi hàm này trả về kết quả.


Bấm Run Pen để xem demo nhé!

Tại sao nên dùng async/await?
Như mình đã nói, async/await có một số ưu điểm vượt trội so với promise:

Code dễ đọc hơn rất rất nhiều, không cần phải then rồi catch gì cả, chỉ viết như code chạy tuần tự, sau đó dùng try/catch thông thường để bắt lỗi.
Viết vòng lặp qua từng phần tử trở nên vô cùng đơn giản, chỉ việc await trong mỗi vòng lặp.
Debug dễ hơn nhiều, vì mỗi lần dùng await được tính là một dòng code, do đó ta có thể đặt debugger để debug từng dòng như thường.
Khi có lỗi, exception sẽ chỉ ra lỗi ở dòng số mấy chứ không chung chung là un-resolved promise.
Với promise hoặc callback, việc kết hợp if/else hoặc retry với code asynchnous là một cực hình vì ta phải viết code lòng vòng, rắc rối. Với async/await, việc này vô cùng dễ dàng.

Async/Await làm code trở nên gọn gàng sạch đẹp như thế nào!

Một demo loop khá cool bằng async await

Bất cập của async/await
Tất nhiên, async/await cũng có một số bất cập mà các bạn cần phải lưu ý khi sử dụng:

Không chạy được trên các trình duyệt cũ. Nếu dự án yêu cầu phải chạy trên các trình duyệt cũ, bạn sẽ phải dùng Babel để transpiler code ra ES5 để chạy.
Khi ta await một promise bị reject, JavaScript sẽ throw một Exception. Do đó, nếu dùng async await mà quên try catch thì lâu lâu chúng ta sẽ bị… nuốt lỗi hoặc code ngừng chạy.
async và await bắt buộc phải đi kèm với nhau! await chỉ dùng được trong hàm async, nếu không sẽ bị syntax error. Do đó, async/await sẽ lan dần ra toàn bộ các hàm trong code của bạn.

async/await vẫn chưa chạy được trên IE, Microsoft Edge 14 và một số trình duyệt cũ hơn
Áp dụng async/await vào code
Về bản chất, một hàm async sẽ trả ra một promise, tương ứng với Task trong C#. Do vậy, để có thể dùng async/await một cách hiệu quả, các bạn phải nắm rõ cơ chế làm việc của Promise nhé!

Hiện tại các phiên bản mới nhất của Chrome, Edge và Firefox đã hỗ trợ async/await, nếu dự án không bắt hỗ trợ các trình duyệt cũ thì các bạn cứ thoải mái dùng async/await để code gọn đẹp hơn nhé.

Ngoài ra, nếu các bạn sử dụng NodeJS, có thể sử dụng combo Promisify + Async/Await như sau:

Sử dụng Bluebird hoặc util.promisify (Node 8 trở lên) để biến các hàm callback của NodeJS thành Promise.
Dùng async/await để lấy kết quả từ các Promise này.


Kết
Bài viết kì này khá phức tạp, lại cần nhiều kiến thức nền về JavaScript nên sẽ hơi khó hiểu.

Các bạn ráng đọc lại 2,3 lần, xem lại các code mẫu để hiểu nha. Nếu có thắc mắc hay có gì cần chia sẻ, các bạn cứ thoải mái comment nhé!

Bonus

Slide và clip thuyết trình của mình tại SingaporeJS

 
 

Link tham khảo thêm

https://hackernoon.com/6-reasons-why-javascripts-async-await-blows-promises-away-tutorial-c7ec10518dd9
https://tutorialzine.com/2017/07/javascript-async-await-explained
Advertisements


Đạtpolime♡
