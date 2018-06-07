### Ruby
#### 개요
- 루비는 순수 객체 지향 언어이다.
- 모든것이 객체로 되어있다.
- Ruby on Rails 프래임 워크 등장으로 유명해졌다

#### Ruby 기초
##### puts vs print
```[Ruby]
3.times {print "Hello!"}
Hello!Hello!Hello! => 3 
3.times {puts "Hello!"}
Hello!
Hello!
Hello!
 => 3 
```


##### p vs puts
```[Ruby]
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

##### Naming conventions
- 변수
	- snake_case
- 상수
	- CONSTANT
- 클래스
	- CamelCase

##### pry
- install
	- `gem install pry`
- excute
	- `pry`

##### inline statement
```[Ruby]
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

##### case
```[Ruby]
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

