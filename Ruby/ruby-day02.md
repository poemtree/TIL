## Ruby
### 개요
- 루비는 순수 객체 지향 언어이다.
- 모든것이 객체로 되어있다.
- Ruby on Rails 프래임 워크 등장으로 유명해졌다

### Ruby 기초
#### puts vs print
```ruby
3.times {print "Hello!"}
Hello!Hello!Hello! => 3 
3.times {puts "Hello!"}
Hello!
Hello!
Hello!
 => 3 
```


#### p vs puts
```ruby
array = [1,2,3]
 => [1, 2, 3] 
puts array
1
2
3
 => nil 
p array
[1, 2, 3]
 => [1, 2, 3] 
a = "hello"
 => "hello" 
puts a
hello
 => nil 
p a
"hello"
 => "hello"  

```

#### Naming conventions
- 변수
	- snake_case
- 상수
	- CONSTANT
- 클래스
	- CamelCase

#### pry
- install
	- `gem install pry`
- excute
	- `pry`

#### inline statement
```ruby
# if문
a = 0
b = 1
puts "a=0"
a=0
=> nil
puts "a=0" if a == 0
a=0
=> nil
puts "a=0" if a == 1
=> nil

# while문
c = 0
result = c +=2 while c < 50
puts result
50
=> nil

puts "hi" if 0
hi
# 0 은 true이다..
```

#### case
```ruby
name = "young"
=> "young"
case name
    pry(main)* when "young" then puts "hi young"  
    pry(main)* when "mu" then pust "hi mu"  
	else puts "hi"  
end  
hi young
=> nil
```

#### method
- 대부분의 언어는..
	- class 밖에 선언되면 funcgion
- Ruby는..
	- 모든 함수가 method..

```ruby
def simple
  puts "simple!!"
end  
=> :simple
simple
simple!!
=> nil
```

##### return
```ruby
# return을 명시한 경우
def add(a,b)
  return a+b
end  
=> :add
add 1,2
=> 3

# return을 생략한 경우
def add2(a, b)
  a+b
end  
=> :add2
add2 1,2
=> 3

# return을 선택적으로 사용해야 할 경우
def divide(a,b)
  return "I don't think so" if b == 0
  a/b
end  
=> :divide
divide 1, 0
=> "I don't think so"
divide 1, 1
=> 1
```

##### parameter
- 매게변수

```ruby
def factorial(n)
    n < 2 ? n * 1 : n * factorial(m-1)
end
factorial # ArgumentError : worng number of arguments (given 0 , expected 1)
def factorial_d(n=5)
    n < 2 ? n * 1 : n * factorial(m-1)
end
factorial_d # 120
```

##### block
- { } 를 일컷는다?
- | | 는 무엇인고?

```ruby
3.times do |asdf|
    puts asdf # Hear is called block
end
```
```ruby
[2] pry(main)> def hihi
    return "No block" unless block_given?
    yield
end  
=> :hihi
hihi
=> "No block"
hihi {}
=> nil
hihi {"Hello"}
=> "Hello"
```

#### String
- single
- double

```ruby
a = "안녕하세요.\n 멋사입니다."
=> "안녕하세요.\n 멋사입니다."
b = '안녕하세요. \n 멋사입니다.'
=> "안녕하세요. \\n 멋사입니다."
puts a
안녕하세요.
 멋사입니다.
=> nil
puts b
안녕하세요. \n 멋사입니다.
=> nil
poemtree:~/workspace $ pry
c = "PoemTree"
=> "PoemTree"
d = "#{c}님 안녕하세요"
=> "PoemTree님 안녕하세요"
puts d
PoemTree님 안녕하세요
=> nil
e = '#{c}님 안녕하세요'
=> "\#{c}님 안녕하세요"
puts c
PoemTree
=> nil
puts e
#{c}님 안녕하세요
=> nil
```

#### Array

#### Hash
- key, value 로 구성
- Array와는 다르게 {}로 생성

```ruby
hash = Hash.new(0) # 빈 해쉬
hash1 = {:key1 => value1, :key2 => valuew2, ...}
hash2 = {key1: value1, key2: valuew2, ...}
hash3 = {"key1" => value1, "key2" => value2, ...}
```

##### each
-  반복하기

```ruby
hash1.each do |k, v|
	 puts "#{k} : #{v}"
 end  
name : PoemTree
age : 29
City : Seoul
```

@[](https://gist.github.com/nacyot/7624036)