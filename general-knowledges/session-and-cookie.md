

### Session 

HTTP là một giao thức phi trạng thái (**stateless protocol**), mỗi request từ client tới webserver là độc lập, vì vậy 
request hiện tại không thể sử dụng các thông tin từ các request trước đó. Điều này có nghĩa là **không có cách nào để
 ghi nhớ danh tính (identity) của một người dùng từ trang này qua trang khác**. Vì vậy, các ứng dụng web 
 (web applications) yêu cầu người dùng phải đăng nhập sử dụng 1 **session**. Dựa vào session thì webserver có thể
  phân biệt được trong các request gửi đến thì request nào là từ cùng 1 người dùng, request nào là của người khác.

Session (hay còn gọi là phiên làm việc) là một cách giao tiếp giữa webserver và client (có thể là trình duyệt web hay ứng dụng trên thiết bị của bạn).

Một session bắt đầu khi client gửi request tới webserver, **chỉ kết thúc khi hết thời gian timeout hoặc khi bạn đóng 
trình duyệt hoặc ứng dụng**. Giá trị của session mặc định sẽ được **lưu trong một tệp tin trên webserver**. Ngoài ra,
 session có thể được lưu trong database, redis, ... tùy theo ý đồ của người lập trình.
Chỉ nên lưu trữ những thông tin tạm thời trên session, ví dụ: thông tin đăng nhập, thông tin giỏ hàng, ...

Mỗi session được cấp phát một mã số định danh duy nhất là Sesion ID. Session ID là **một chuỗi ngẫu nhiên 32 kí tự**,
 ví dụ: _`06383951600dd0fc8713fafd63142fce`_

Có nhiều cách sử dụng session:

- **Cách 1**: Truyền Session ID giữa các trang web thông qua query string trên URL. 

    Ít được sử dụng vì mất công xử lý
 việc truyền qua lại session ID giữa các trang web, thường sử dụng với các trình duyệt web (web browser) không sử dụng được cookie

    ```
    http://example.com/bai-viet?sessionid=06383951600dd0fc8713fafd63142fce
    ```

- **Cách 2**: Lưu session ID trong cookie. 

    Cách này thuận tiện hơn vì cookie dưới web browser sẽ được tự động gửi kèm theo request tới webserver. Tuy nhiên,
     chỉ sử dụng được với trình duyệt có sử dụng cookie

### Cookie

Cookie là một đoạn dữ liệu được truyền từ webserver tới client, được **lưu trữ tại thiết bị của người dùng** khi bạn bắt đầu truy cập vào ứng dụng (website). **Từ các lần request sau cookie sẽ được tự động đính kèm vào request gửi ngược lại webserver**.

Vì cookie được lưu dưới trình duyệt người dùng nên nó không bảo mật, **để bảo mật, các dữ liệu sẽ được mã hóa** trước
 khi lưu vào cookie và gửi xuống trình duyệt phía người dùng.

Dung lượng tối đa của 1 cookie là **4KB** và được **lưu trong một khoản thời gian nhất định** - Expires / Max-Age.




