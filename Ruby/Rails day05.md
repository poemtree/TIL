## resources


## 
```ruby
class User < ActiveRecord::Base
  has_secure_password
end

```

# cookie
연결이 지속되지 않는다는 http 통신의 단점을 보완하기 위한 수단

## ApplicationController
모든 컨트롤러가 상속받는 최상의 컨트롤러
