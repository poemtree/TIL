# 실습
## 가상 데이터 만들기

### 환경설정
#### Faker get 설치
[README Link](https://github.com/stympy/faker/blob/master/README.md)
##### Gemfile에 추가
```
gem 'faker', :git => 'https://github.com/stympy/faker.git', :branch => 'master'
```
``bundle install`` 실행

### 가상 데이터 생성
#### db\seeds.rb에 추가
```ruby
# 내용 추가
Post.create(title: "TitleTest", content: "ContentTest")
```
``rake db:seed`` 실행
#### random 값 생성
```ruby
# db\seeds.rb
require 'faker'

10.times do
  Post.create(title: Faker::GameOfThrones.character, content: Faker::GameOfThrones.quote)
end
```



```ruby
params.require(:post).permit(:title, :content)
```

#### Form_tag, Form_for
[Link](https://guides.rorlab.org/form_helpers.html)
**주요특징**
- 특정한 모델의 객체를(Post)조작하기 위해 사용
- 별도의 url(action="/"), method(get, post, put 등) 명시하지 안항도 됨.
- Controller의 해당 액션(new, edit)에서 반드시 @post에 Post오브젝트가 담겨야 함
	- new : @post = Post.new
	- edit : @post = Post.find(:id)
- 각 input field의 symbol은 반드시 @post의 column 명이랑 일치해야 함.

#### link_to : url helper
[Link](https://apidock.com/rails/ActionView/Helpers/UrlHelper/link_to)
```ruby
<%= %>
```
#### Simple form
[README Link](https://github.com/plataformatec/simple_form/blob/master/README.md)
1. Gemfile 설정
	```ruby
    gem 'simple_form'
    ```
2. bundle install
	```
    $ bundle install
    ```
3. 설치
	```
    $ rails generate simple_form:install
    $ rails generate simple_form:install --bootstrap
    ```
4. Bootstrap 프로젝트에 적용
	- CDN을 `application.html.erb`
5. Form helper 만들기

   ```erb
   <%= simple_form_for @post do |f| %>
     <%= f.input :title %>
     <%= f.input :content %>
     <%= f.button :submit, class: "btn-primary" %>
   <% end %>
   ```