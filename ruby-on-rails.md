## Chương 8: Đăng nhập

Ở chương này chúng ta sẽ thực hiện xây dựng một hệ thống đăng nhập (xác thực) cơ bản nhưng đầy đủ chức năng: **ứng dúng sẽ duy trì trạng thái đăng nhập của người dùng cho tới khi người dùng tắt trình duyệt web**.

Hệ thống xác thực (authentication system) sẽ cho phép chúng ta tùy chỉnh trang web và thực hiện xậy dựng một mô hình ủy quyền dựa trên trạng thái đăng nhập và định danh (identity) của người dùng 

Hệ thống xác thực trong chương này sẽ làm nền tảng cho hệ thống đăng nhập nâng cao hơn được triển khai trong chương 9

### 8.1 Session 

HTTP là một giao thức không lưu lại trạng thái (**stateless protocol**), mỗi request từ client tới webserver là độc lập, vì vậy request hiện tại không thể sử dụng các thông tin từ các request trước đó. Điều này có nghĩa là **không có cách nào để ghi nhớ danh tính (identity - định danh) của một người dùng từ trang này qua trang khác**. Vì vậy, các ứng dụng web (web applications) yêu cầu người dùng đăng nhập phải sử dụng 1 **session**. Dựa vào session thì webserver có thể phân biệt được trong các request gửi đến thì request nào là từ cùng 1 người dùng, request nào là của người khác.

##### Session

Session (hay còn gọi là phiên làm việc) là một cách giao tiếp giữa client và webserver. Client ở đây có thể là trình duyệt web hay ứng dụng trên thiết bị của bạn. 

Một session bắt đầu khi client gửi request tới webserver, nó **tồn tại xuyên suốt từ trang này tới trang khác, chỉ kết thúc khi hết thời gian timeout hoặc khi bạn đóng trình duyệt hoặc ứng dụng**. Giá trị của session sẽ được **lưu trong một tệp tin trên webserver**.
Chỉ nên lưu trữ những thông tin tạm thời trên session, ví dụng thông tin đăng nhập, thông tin giỏ hàng, ...

Mỗi session được cấp phát một mã số định danh duy nhất là Sesion ID. **Session ID là một chuỗi ngẫu nhiên 32 kí tự**, ví dụ: _06383951600dd0fc8713fafd63142fce_. 

Có nhiều cách sử dụng session:

- **Cách 1**: Truyền Session ID giữa các trang web thông qua query string. cách này ít được sử dụng vì mất công xử lý việc truyền qua lại session ID giữa các trang web, thường sử dụng với các trình duyệt web (web browser) không sử dụng được cookie


    http://localhost/bai-viet?sessionid=abcxyz

- **Cách 2**: Lưu session ID trong cookie. Chỉ sử dụng được với các web browser có sử dụng cookie. Cách này thuận tiện hơn vì cookie dưới web browser sẽ được tự động gửi kèm theo request tới webserver.

Trong ứng dụng web với Rails sử dụng cookie để lưu trữ session.

##### Cookie

Cookie là một đoạn dữ liệu được truyền từ webserver tới client, được **lưu trữ tại thiết bị của người dùng** khi bạn bắt đầu truy cập vào ứng dụng (website). **Từ các lần request sau cookie sẽ được tự động đính kèm vào request gửi ngược lại webserver**.

Vì cookie được lưu dưới trình duyệt người dùng nên nó không bảo mật, vì thế **để bảo mật, các dữ liệu như sessionId sẽ được mã hóa** trước khi lưu vào cookie và gửi xuống trình duyệt phía người dùng.

Dung lượng tối đa của 1 cookie là **4KB** và được **lưu trong một khoản thời gian timeout nhất định**.

Sử dụng SessionController để thực hiện đăng nhập và đăng xuất người dùng, định nghĩa các routes 

```
Rails.application.routes.draw do
  ...
  get    '/login',   to: 'sessions#new'
  post   '/login',   to: 'sessions#create'
  delete '/logout',  to: 'sessions#destroy'
  ...
end
```

- Khi người dùng truy cập trang đăng nhập sẽ hiển thị 1 form để người dùng điền thông tin đăng nhập (email + password), form này sẽ được  

```
<% provide(:title, "Log in") %>
<h1>Log in</h1>

<div class="row">
  <div class="col-md-6 col-md-offset-3">
    <%= form_for(:session, url: login_path) do |f| %>

      <%= f.label :email %>
      <%= f.email_field :email, class: 'form-control' %>

      <%= f.label :password %>
      <%= f.password_field :password, class: 'form-control' %>

      <%= f.submit "Log in", class: "btn btn-primary" %>
    <% end %>

    <p>New user? <%= link_to "Sign up now!", signup_path %></p>
  </div>
</div>
```

- Người dùng nhập thông tin đăng nhập vào form và submit lên webserver. Webserver sau khi nhận thông tin sẽ thực hiện tìm kiếm email của người dùng trong database sử dụng `User.find_by`, nếu có sẽ thực hiện xác thực với password bằng hàm `authenticate` được cung cấp bởi `has_secure_password`

    + nếu không hợp lệ sẽ hiển thị view `new` và thông báo lỗi sử dụng `flash` 
    
    + nếu hợp lệ tạo session cho người dùng sử dụng phương thức `session` sau đó điều hướng người dùng sang trang profile của người dùng.
    
    ```
    # app/controllers/application_controller.rb
    class ApplicationController < ActionController::Base
      protect_from_forgery with: :exception
      include SessionsHelper
    end
    ```
    
    ```
    # app/helpers/sessions_helper.rb
    module SessionsHelper
    
      # Đăng nhập người dùng
      def log_in(user)
        session[:user_id] = user.id
      end
    
      # Trả về thông tin người dùng hiện tại
      def current_user
        if session[:user_id]
          @current_user ||= User.find_by(id: session[:user_id])
        end
      end
    
      # Trả về true nếu người dùng đã đăng nhập, false nếu chưa
      def logged_in?
        !current_user.nil?
      end
      
    end
    
    ```
    
    ```
    # app/controllers/sessions_controller.rb
    class SessionsController < ApplicationController
    
      def new
      end
    
      def create
        user = User.find_by(email: params[:session][:email].downcase)
        if user && user.authenticate(params[:session][:password])
           log_in user
           redirect_to user
        else
          flash[:danger] = 'Invalid email/password combination' # Not quite right!
          render 'new'
        end
      end
    
      def destroy
      end
    end
    ```

- Nếu người dùng đăng nhập hợp lệ thì header của website sẽ ẩn link đăng nhập và thay vào đó là link đăng xuất, và link tới profile

```
<!-- app/views/layouts/_header.html.erb -->
<header class="navbar navbar-fixed-top navbar-inverse">
  <div class="container">
    <%= link_to "sample app", root_path, id: "logo" %>
    <nav>
      <ul class="nav navbar-nav navbar-right">
        <li><%= link_to "Home", root_path %></li>
        <li><%= link_to "Help", help_path %></li>
        <% if logged_in? %>
          <li><%= link_to "Users", '#' %></li>
          <li class="dropdown">
            <a href="#" class="dropdown-toggle" data-toggle="dropdown">
              Account <b class="caret"></b>
            </a>
            <ul class="dropdown-menu">
              <li><%= link_to "Profile", current_user %></li>
              <li><%= link_to "Settings", '#' %></li>
              <li class="divider"></li>
              <li>
                <%= link_to "Log out", logout_path, method: :delete %>
              </li>
            </ul>
          </li>
        <% else %>
          <li><%= link_to "Log in", login_path %></li>
        <% end %>
      </ul>
    </nav>
  </div>
</header>
```

- Đăng xuất người dùng: thực hiện xóa id người dùng khỏi session và đặt thông tin người dùng hiện tại là nil

```
# app/helpers/sessions_helper.rb
module SessionsHelper

  # Đăng xuất người dùng
  def log_out
    session.delete(:user_id)
    @current_user = nil
  end
end
```

```
# app/controllers/sessions_controller.rb
class SessionsController < ApplicationController

  def destroy
    log_out
    redirect_to root_url
  end
end
```



