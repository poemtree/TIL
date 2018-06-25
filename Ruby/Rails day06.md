### [Rails 기본 라우팅 link](https://guides.rorlab.org/routing.html)
```ruby
# CRUD = C
get 'posts/new'
post 'posts/create'
# CRUD = R
get 'posts' => 'posts#index'
get 'posts/:id' => 'posts#show'
# CRUD = U
get 'posts/:id/edit' => 'posts#edit'
put 'posts/:id' => 'posts#update'
# CRUD = D
delete 'posts/:id' => 'posts#destroy'
```
```ruby
resources :posts
```
- REST API를 구성하는 기본 원칙
	- URL은 정보의 자원을 표현한다.
	- 자원에 대한 행위는 HTTP method(verb)로 표현한다.

### form에서 post 요청 보내기
#### CREATE
```erb
<!-- app\views\posts\new.html.erb -->
<form action="/posts" method="post">
	..
    <input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
    ..
</form>
```
```ruby
<!-- app\controllers\application_controller.rb -->
protect_from_forgery with: :exception
```
- form post 요청에서 token이 없으면 오류가 발생
- 토큰을 사용하는 이유는 CSRF공격을 방지하기 위함
###### [CSRF link](https://namu.wiki/w/CSRF)

#### UPDATE
```erb
<!-- app\views\posts\edit.html.erb -->
<form action="/posts/<%= @post.id %>" method="post">
	..
    <input type="hidden" name="_method" value="put">
    <input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
    ..
</form>
```

### [Database Relation(association) link](https://guides.rorlab.org/association_basics.html)
##### 1:多(1:N)
User(1) - Post(N)관계 설정.
	유저는 많은 게시글을 가지고 있고, 게시글은 특정 유저에 속하기 때문
    
##### 실제코드
객체 관계 설정
```erb
# app\model\user.rb
class User < ActiveRecord::Base
    has_secure_password
    has_many :posts
end
```
```erb
# app\models\post.rb
class Post < ActiveRecord::Base
	belongs_to :user
end
```

데이터베이스 관계 설정
```erb
# db\migrate\20180620013210_create_posts.rb
class CreatePosts < ActiveRecord::Migration
  def change
    create_table :posts do |t|
      t.integer :user_id # Foreign key
      # t.reference :user_id 도 같은 뜻?
      t.string :title
      t.text :content
      t.timestamps null: false
    end
  end
end

```

##### 실제로 관계를 활용하기
1. 유저가 가지고 있는 모든 게시글
```ruby
# 1번 유저의 모든 글
@user_posts = User.find(1).posts
# 1번 유저가 쓴 글의 갯수
User.find(1).posts.count
```
1. 특정 게시글에서 작성한 사람 정보 출력
```ruby
# 1번 글의 유저 작성자 이름
Post.find(1).user.username
```

### Login
```erb
<!-- app\controllers\sessions_controller.rb -->
class SessionsController < ApplicationController
    def new
    end

    def create
      address = :back
      user = User.find_by(email: params[:email])
      if user
        if user.authenticate(params[:password])
          session[:user_id] = user.id
          flash[:notice] = "안녕하세요 #{user.username}님!"
          address = "/"
        else
          flash[:alert] = "비밀번호가 일치하지 않습니다."
        end
      else
        flash[:alert] = "가입되지 않은 이메일입니다."
      end
      redirect_to address
    end

    def destroy
      session.clear
      flash[:notice] = "로그아웃 되었습니다."
      redirect_to "/"
    end

end
```
#### before filter : 컨트롤러
```ruby
# app\controller\posts_controller.rb
# authorize 메소드를 실행하는데, 여기 모든 액션 중에 index를 제외하고 실행
# 컨트롤러가 실행되면 무조건 먼저 실행되는 함수
before_action :authorize, except: [:index]
```
```ruby
# app\controllers\application_controller.rb
def authorize
	unless current_user
    	flash[:alert] = "로그인 해주세요"
        redirect_to "/"
end
```
#### helper method
```ruby
# app\controllers\application_controller.rb
helper_method :current_user
def current_user
	@user ||= User.find(params[:id]) if session[:user_id]
end
```
뷰에서 활용하기
```erb
<!-- app\views\layouts\application.html.erb -->
..
    <% if current_user %>
        ..
    <% els %>
        ..
    <% end %>
..
```
### Devise gem 사용하기
#### 환경설정
1. Gemfile 맨 위에 ``gem 'devise'`` 추가
2. ``bundle install`` 명령어 입력
3. devise 설치 : ``rails generate devise:install``
	- `config/initialize/devise.rb` 만들어짐.
4. user 모델 만들기 : ``rails generate devise user``
	- `db/migrate/20180625_devise_create_users.rb`
    - `model/user.rb`
    - `config/routes.rb` : devise_for :users
5. 마이그레이션 : ``rake db:migrate``

#### routes 확인
   | new_user_session_path         | GET    | /users/sign_in(.:format)       | devise/sessions#new          |
   | ----------------------------- | ------ | ------------------------------ | ---------------------------- |
   | user_session_path             | POST   | /users/sign_in(.:format)       | devise/sessions#create       |
   | destroy_user_session_path     | DELETE | /users/sign_out(.:format)      | devise/sessions#destroy      |
   | user_password_path            | POST   | /users/password(.:format)      | devise/passwords#create      |
   | new_user_password_path        | GET    | /users/password/new(.:format)  | devise/passwords#new         |
   | edit_user_password_path       | GET    | /users/password/edit(.:format) | devise/passwords#edit        |
   |                               | PATCH  | /users/password(.:format)      | devise/passwords#update      |
   |                               | PUT    | /users/password(.:format)      | devise/passwords#update      |
   | cancel_user_registration_path | GET    | /users/cancel(.:format)        | devise/registrations#cancel  |
   | user_registration_path        | POST   | /users(.:format)               | devise/registrations#create  |
   | new_user_registration_path    | GET    | /users/sign_up(.:format)       | devise/registrations#new     |
   | edit_user_registration_path   | GET    | /users/edit(.:format)          | devise/registrations#edit    |
   |                               | PATCH  | /users(.:format)               | devise/registrations#update  |
   |                               | PUT    | /users(.:format)               | devise/registrations#update  |
   |                               | DELETE | /users(.:format)               | devise/registrations#destroy |


#### 컨트롤러
- 회원가입 : `get 'users/sign_up'`
- 로그인 : `get 'users/sign_in' `
- 로그아웃 : `delete 'users/sign_out'`

#### 헬프 메소드

- `user_sign_in?`
  : 유저가 로그인 했는지 안했는지를 true/false 리턴

- `current_user`
  : 로그인되어있는 user 오브젝트를 가지고 있음

- `before_action :authenticate_user!`

  : 로그인 되어있는 유저 검증(필터)

#### view 파일 수정하기

1. view 파일 만들기 : ``$ rails generate devise:views users``
2. view 파일 scope 수정하여 연동하기
	```ruby
    # config\initializers\devise.rb
    # 232번째 줄 # config.scoped_views = false 주석 삭제 후
    # true로 수정
    config.scoped_views = true
    ```

#### Permit 수정하기
##### app\controllers\application_controller.rb 에 추가
```ruby
# :함수명, 조건
before_action :configure_permitted_parameters, if: :devise_controller?

# 함수
protected
def configure_permitted_parameters
  # :액션, [:추가할 키이름]
  devise_parameter_sanitizer.permit(:sign_up, keys: [:name])
end
```
