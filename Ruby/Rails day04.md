# POST 방식 사용하기
## HTTP 메서드
- get - URL(URI) 형식으로 웹서버측 리소스(데이터)를 요청
- post - 클라이언트 데이터를 서버로 보냄
- put/patch - 서버로 보낸 데이터를 저장/지정 리소스의 부분만을 수정
- delete - 서버의 데이터를 삭제할때 사용

### config\routes.rb
```ruby
Rails.application.routes.draw do
  root 'posts#index'
  get 'posts/index'

  get 'posts/new'

  post 'posts/create'

  get 'posts/:id' => 'posts#show'

  get 'posts/:id/edit' => 'posts#edit'

  put 'posts/update'

  delete 'posts/:id' => 'posts#destroy'
end
```

### app\controllers\application_controller.rb
보안을 위한 사용자 인증 코드.. 이 때문에 그냥 post 방식을 사용할 수 없다.
```ruby
class ApplicationController < ActionController::Base
  # Prevent CSRF attacks by raising an exception.
  # For APIs, you may want to use :null_session instead.
  protect_from_forgery with: :exception
end
```
## CREATE
### app\views\posts\new.html.erb
post 방식으로 변경하면 오류가 난다.. 사용자 인증을 하기 위해 hidden 타입으로 **authenticity_token** 을 같이 보내야 한다.
```erb
<div class="mt-3 ml-5">
    <h1>Insert new post</h1>
</div>
<hr>
<div class="mt-3 ml-5">
  <form action="/posts/create" method="post">
    <input type="text" name="title" placeholder="Enter title"><br>
    <input type="text" name="username" placeholder="Enter your name"><br>
    <textarea name="content" rows="8" cols="80" placeholder="What do you want to write"></textarea><br>
    <button type="submit" class="btn btn-success">작성하기</button>
    <a class="btn btn-primary ml-1" href="/" role="button">돌아가기</a>
    <input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
  </form>
</div>
```

## UPDATE
update는 PUT 방식을 이용한다.

```
```
