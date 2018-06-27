# Scaffold

### 사용법
##### 프로젝트 만들기
```
$ rails new scaffold_test
$ cd scaffold_test
$ rails g scaffold post title:string content:text
$ rake db:migrate
```
##### routes.rb 수정
```ruby
# config\routes.rb
# root 경로 설정
root 'posts#index'
```
``$ rails s -b 0.0.0.0`` 명렁어 입력하여 서버 실행
