### 암호화
`$ gem install 'bcrypt'`
```ruby
@password = BCrypt::Password.create(params[:password])
@password == params[:password]
=> true
params[:password] == @passwrod
=> false
```

### 배포하기
#### Heroku 
1. **[Homepage](https://www.heroku.com/)**에 접속하여 회원가입을 한다.
2. 배포할 git 레파지토리에서 `$ touch config.ru` 를 입력한다.
```ruby
# config.ru
require './app'
run Sinatra::Application
```
3. heroku에 로그인하여 배포할 서버를 push 한다.

```
poemtree:~/workspace (master) $ heroku login
Enter your Heroku credentials:
Email: yym1232@gmail.com
Password: *********
Logged in as yym1232@gmail.com
poemtree:~/workspace (master) $  heroku git:remote -a sinatra-board-poemtree
set git remote heroku to https://git.heroku.com/sinatra-board-poemtree.git
poemtree:~/workspace (master) $ git status
On branch master
nothing to commit, working tree clean
poemtree:~/workspace (master) $ git add .
poemtree:~/workspace (master) $ git commit -m "서버 배포"
poemtree:~/workspace (master) $ git push heroku master
```