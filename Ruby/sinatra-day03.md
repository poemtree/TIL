### Sinatra day03
#### Ready to sinatra
##### 필수 gem 설치
`$ gem install sinatra`

`$ gem install sinatra-contrib`

`$ gem install sinatra-reloader`

##### 디렉토리/파일 만들기
`$ touch app.rb`

`$ touch index.html`

`$ mkdir views`

`$ cd views`

`$ touch lunch.erb`


##### Routing 설정 및 view 설정
```ruby
# app.rb

require 'sinatra'
require 'sinatra/reloader'

# Wellcome sinatra study
# 1th.. we will use auto reload
# 2th.. if someone access to '/' we will send 'index.html'
# 3th.. if someone access to '/lunch' we will show 'lunch.erb' page

# gem install sinatra
# gem install sinatra-contrib
# gem install sinatra-reloader

get '/' do
    send_file 'index.html'
end

get '/lunch' do
    @lunch = ["김밥", "샌드위치", "피자", "치킨", "삼겹살", "국밥", "국수", "냉면", "돈까스", "햄버거", "웰스토리"].sample
	erb :lunch
end
```

```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <a href="/lunch">점심메뉴 추천받기</a>
</body>
</html>
```

```erb
<!-- lunch.erb -->

<p>오늘의 점심 추천 메뉴는? </p>
<%= @lunch %> 입니다.
```

#### ORM : Object Relational Mapping
객체지향언어(ruby)와 데이터베이스(sqlite) 연결을 도와주는 도구
[Datamapper](http://recipes.sinatrarb.com/p/models/data_mapper)

##### 필수 gem 설치
`$ gem install datamapper`

`$ gem install dm-sqlite-adapter`


##### 소스
```ruby
# if you run code on c9, you need "gem 'json', '~> 1.6' "
require 'rubygems'
require 'sinatra'
require 'data_mapper' # metagem, requires common plugins too.

# need install dm-sqlite-adapter
DataMapper::setup(:default, "sqlite3://#{Dir.pwd}/blog.db")

class Post
  include DataMapper::Resource
  property :id, Serial
  property :title, String
  property :body, Text
  property :created_at, DateTime
end

# Perform basic sanity checks and initialize all relationships
# Call this when you've defined all your models
DataMapper.finalize

# automatically create the post table
Post.auto_upgrade!
```

##### 데이터 조작
```ruby
# 전체 조회
Post.all

# 조건 검색
Post.first
Post.last
Post.first(title: "insert title")
Post.get(insert id)

# 생성
## 첫번째 방법
Post.create(title: "title", body: "body")
## 두번째 방법
p = Post.new
p.title = "insert title"
p.body = "insert body"
p.save # saved in database

# 삭제
Post.first.destroy
Post.first(title: "insert title").destroy
Post.get(insert id ).destroy

# 업데이트
Post.last.update(title: "insert title", body: "insert body")
```

#### yield
yield 구문에 Parameter로 받은 블럭이 들어간다..
Sinatra에서는 저 부분에 다른 Html 문서가 삽입된다..
```html
<html>
	<head>
    </head>
    <body>
    	<%= yield %>
    </body>
</html>
```

#### Symbol
심볼을 사용하는 이유..
심볼형 변수는 따로 관리 하는 듯... 메모리 영역이라던지...
같은 내용을 갖는 심볼의 오브젝트 아이디를 확인하면 같은 것을 알 수 있다..
무튼 루비에서는 스트링 보다 심볼이 더 빠르다는 것...

#### params
##### variable routing
```html
<a href="/posts/<%=p.id%>">Enter</a>
```
```ruby
# app.rb
get '/posts/:id' do
    @id = params[:id]
    @post = Post.get(@id)
    erb :show
end
```
##### `form`tag를 통해서 받는 법
```html
<form action="/posts/create">           
    <input type="text" name="title" placeholder="Enter Title">
    <input type="text" name="body" placeholder="Enter Body">
    <input type="submit" value="작성하기"/>
</form>
```
```ruby
# app.rb
get '/posts/create' do
    @title = params[:title]
    @body = params[:body]
    Post.create(title: @title, body: @body )
    erb :create
end
```

#### 게시판 만들기
##### 파일구조
- app.rb
- **views/**
	- layout.erb
	- posts.erb
	- new.erb
	- create.erb
	- show.erb
- index.html

##### app.rb
```ruby
# app.rb

# c9 환경에서 구동하는 경우 필요하다..
gem 'json', '~> 1.6'

require 'sinatra'
require 'sinatra/reloader'

# DB 사용을 위한 Datamapper...
require 'data_mapper' # metagem, requires common plugins too.

# print datamapper log
DataMapper::Logger.new($stdout, :debug)

# sqlite를 사용하기 위한 설정..
DataMapper::setup(:default, "sqlite3://#{Dir.pwd}/blog.db")

# Post 객체.. vo 같은 느낌..
class Post
  include DataMapper::Resource
  property :id, Serial
  property :title, String
  property :body, Text
  property :created_at, DateTime
end

# Perform basic sanity checks and initialize all relationships
# Call this when you've defined all your models
DataMapper.finalize

# automatically create the post table
Post.auto_upgrade!

# Wellcome sinatra study
# 1th.. we will use auto reload
# 2th.. if someone access to '/' we will send 'index.html'
# 3th.. if someone access to '/lunch' we will show 'lunch.erb' page

# gem install sinatra
# gem install sinatra-contrib
# gem install sinatra-reloader

get '/' do
    send_file 'index.html'
end

# 간단한 랜덤 소스
get '/lunch' do
    @lunch = ["김밥", "샌드위치", "피자", "치킨", "삼겹살", "국밥", "국수", "냉면", "돈까스", "햄버거", "웰스토리"].sample
	erb :lunch
end

# 게시판 리스트
get '/posts' do
    @posts = Post.all.reverse
    erb :posts
end

# 글 작성 페이지
get '/posts/new' do
    erb :new
end

# 글 게시
get '/posts/create' do
    @title = params[:title]
    @body = params[:body]
    Post.create(title: @title, body: @body )
    erb :create
end

# 글 읽기
# CRUD - Read
# Using variable routing for searching
get '/posts/:id' do
    @id = params[:id]
    @post = Post.get(@id)
    erb :show
end
```

##### index.html
```html
<!-- index.html -->

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<body>
    <a href="/lunch">점심메뉴 추천받기</a><br>
    <a href="/posts">게시판</a>
</body>
</html>
```

##### posts.erb
```erb
<div class="container mt-3">
    <h1>게시판 입니다.</h1>
    <div class="text-right mb-1">
        <a class="btn btn-outline-primary btn-sm" role="button" href="/posts/new">글 쓰기</a> <br>
    </div>
    <table class="table table-hover">
      <thead>
        <tr>
          <th scope="col">id</th>
          <th scope="col">Title</th>
          <th scope="col">Body</th>
          <th scope="col">Date</th>
          <th scope="col">Enter</th>
        </tr>
      </thead>
      <tbody>
        <% @posts.each do |p| %>
            <tr>
                <th scope="row"><%= p.id %></th>
                <td><%= p.title %></td>
                <td><%= p.body %></td>
                <td><%= p.created_at %></td>
                <td><a href="/posts/<%=p.id%>">Enter</a></td>
            </tr>
        <% end %>
      </tbody>
    </table>
</div>
```

##### new.erb
```erb
<div class="container mt-3">
    <h1>게시판 입니다.</h1>
    <a href="/">홈으로 가기</a>
    <form action="/posts/create">
        <div class="form-group">
            <label>Title</label>
            <input type="text" class="form-control" name="title" placeholder="Enter Title">
        </div>
        <div class="form-group">
            <label>Body</label>
            <input type="text" class="form-control" name="body" placeholder="Enter Body">
        </div>
        <input type="submit" value="작성하기"/>
    </form>
</div>
```

##### create.erb
```erb
<p>제목 : <%= @title %></p>
<p>내용 : </p><%= @body %></p>
<p>가 작성 되었습니다.</p>
<a href="/posts">돌아가기</a>
```

##### show.erb
```erb
<p><%= @post.id %></p>
<p><%= @post.title %></p>
<p><%= @post.body %></p>
<p><%= @post.created_at %></p>
```