## Facebook login

### Developers facebook 가입

> [Link](https://developers.facebook.com/)

#### 1. facebook login API 추가 

#### 2. 유효한 OAuth 리디렉션 URI 

아래 내용 추가

`` http://localhost:3000/users/auth/facebook/callback``

#### 3. config\application.yml 수정

```ruby
# 가입 후 발급 받은 키 값을 value에 넣는다
# 키 값에 숫자만 있다면 ""으로 감싸야 한다
FB_APP_ID: value
FB_APP_SECRET: value
```



### OmniAuth Facebook

> [Link](https://github.com/mkdynamic/omniauth-facebook)



#### 1. Gemfile 수정

```ruby
# facebook Login
gem 'omniauth-facebook'
```

#### 2. app\models\user.rb 수정

```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise ...,
      	 ...,
         :omniauthable, omniauth_providers: [:facebook]
    # devise 안에 :omniauthable, omniauth_providers: [:facebook] 추가
```

#### 3. config\routes.rb 수정

```ruby
Rails.application.routes.draw do
    # devise_for 안에 controllers: 안에 omniauth_callback: 'users/omniauth_callbacks' 추가
    devise_for :users, controllers: { sessions: 'users/sessions', omniauth_callback: 'users/omniauth_callbacks' }
    ...
```

#### 4. config\initializers\devise.rb 수정

```ruby
# 260번째 줄에 아래 내용 추가
config.omniauth :facebook, ENV['FB_APP_ID'], ENV['FB_APP_SECRET']
```

#### 5. Identity model 생성 

```
$ rails g model identity user:references provider uid
$ rake db:migrate
```

