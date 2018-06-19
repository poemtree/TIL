# Rails 명령어
`rails g model model_name`
`rake db:migrate`
`rake db:drop`
`bundle install`
`rails console`
`rvenv install ruby_version ` : 해당 ruby_version으로 재설치 한다.

## migration file
모델을 생성하면 함께 생성되며, DB의 구조를 잡는다..
`rake db:migrate` 명령을 입력하면 실제 DB가 생성되며
마이그레이션 파일을 수정하고 다시 명령어를 입력해도 
DB는 변경되지 않는다.. 수정이 필요한 경우
`rake db:drop` 명령어로 드랍하고 다시 만들어야 한다..


## 실습
### Gemfile 수정
1. `gem 'rails_db'` 을 Gemfile에 삽입
```
# 36번째 줄에 있는 grop :development, :test do 안에 추가한다.
group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem 'byebug', platforms: [:mri, :mingw, :x64_mingw]
  gem 'rails_db'
end
```
1. `bundle install` 명령어를 실행하여 Gem을 설치한다.
1. `rails s` 명렁어로 서버를 실행한다.
1. http://localhost:3000/rails/db/tables/ 로 접속하면 DB를 관리할 수 있다.
1. `Ctrl+x`로 서버를 종료한다.
1. `rails console` 명령어를 입력하여 서버의 콘솔로 들어갈 수 있다.
1. `Post.all`을 통해 현재 Post 목록을 볼 수 있다.
1. `Post.create(Col_1_name: "Value_1", Col_2_name: "Value_2", ..., Col_n_name: "Value_n")` 으로 Post를 생성할 수 있다..

Rails 버전이 4.2.10일 경우`$ rails s -b 0.0.0.0` 명령으로 서버를 실행해야 한다.

## 알림 띄우기
Bootstrap을 사용..
flash 변수를 사용..

#### flash
세션을 유지하며 공유하는 변수..

### app\views\layouts\application.html.erb

```erb
<!-- application.html.erb -->
<!DOCTYPE html>
<html>
<head>
  <title>Blog</title>
  <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track' => true %>
  <%= javascript_include_tag 'application', 'data-turbolinks-track' => true %>
  <%= csrf_meta_tags %>
  <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/css/bootstrap.min.css" integrity="sha384-WskhaSGFgHYWDcbwN70/dfYBj47jz9qbsMId/iRN3ewGhXQFZCSftd1LZCfmhktB" crossorigin="anonymous">
</head>
<body>
  <%= render 'layouts/flash' %>
  <%= yield %>

  <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.3/umd/popper.min.js" integrity="sha384-ZMP7rVo3mIykV+2+9J3UJ46jBk0WLaUAdn689aCwoqbBJiSnjAK/l8WvCWPIPm49" crossorigin="anonymous"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.1.1/js/bootstrap.min.js" integrity="sha384-smHYKdLADwkXOn1EmN1qk/HfnUcbVRZyYmZ4qpPea6sjB/pTJ0euyQp0Mk8ck+5T" crossorigin="anonymous"></script>
</body>
</html>
```

### app\views\layouts\\_flash.html.erb
```erb
<% if flash[:alert] %>
  <div class="alert alert-danger" role="alert">
    <%= flash[:alert] %>
  </div>
<% elsif flash[:notice] %>
  <div class="alert alert-success" role="alert">
    <%= flash[:notice] %>
  </div>
<% end %>
```

### app\controllers\post_controller.rb
```ruby
class PostController < ApplicationController
  def index
    @posts = Post.all
  end

  def new
  end

  def create
    # 쌔로운 모델 객체를 만드는 방법...1
    # Post.create(title: params[:title], body: params[:body])
    # text 안에 변수를 넣을 때 반드시 **로 해주어야 한다.

    # 쌔로운 모델 객체를 만드는 방법...2
    post = Post.new
    post.title = params[:title]
    post.body = params[:body]
    post.save
    flash[:notice] = "글이 작성 되었습니다."
    redirect_to "/post/#{post.id}"
  end

  def show
    @post = Post.find(params[:id])
  end

  def destroy
    post = Post.find(params[:id])
    post.destroy
    flash[:alert] = "글이 삭제 되었습니다."
    redirect_to '/'
  end

  def edit
    @post = Post.find(params[:id])
  end

  def update
    post = Post.find(params[:id])
    post.update(title: params[:title], body: params[:body])
    flash[:notice] = "글이 수정 되었습니다."
    redirect_to "/post/#{post.id}"
  end
end
```
