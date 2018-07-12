## Kakao Login

### Developers Kakao

#### 1. Aap 생성

#### 2. REST API 키 복사

#### 3. config\application.yml 수정 

```ruby
# config\application.yml
KAKAO_APP_KEY: VALUE
```

#### 4. 앱 수정 



## omniauth-kakao

#### 1. 

```ruby
gem 'omniauth-kakao', :git => 'https://github.com/hcn1519/omniauth-kakao'
```

`` & bundle update&&bundle install`` 명령어 실행

#### 2. config\initializers\devise.rb

```ruby
# 260번째 줄에 추가
config.omniauth :kakao, ENV['KAKAO_APP_KEY'], redirect_path: '/users/auth/kakao/callback'
```

#### 3. app\models\user.rb

```ruby
class User < ActiveRecord::Base
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable and :omniauthable
  devise ... 
    	 ...
    	 :omniauthable, omniauth_providers: [:kakao]
  # devise 안에 :omniauthable, omniauth_providers: [:kakao] 추가.. 이미 있다면 omniauth_providers: 안에 :kakao만 추가하면 된다..
```

