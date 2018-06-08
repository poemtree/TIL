### Sinatra day02
#### RandomGame 예제
```ruby
# app.rb
get '/randomgame/:name' do
    array = ["이상해씨", "꼬부기", "피카츄", "고라파덕"]
    hash = {"이상해씨" => "https://ncache.ilbe.com/files/attach/new/20150803/377678/569613677/6317036203/5f3fcdb3e4036599540d72925632d9e9.jpg", "꼬부기" => "https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQIXuVkIlabTSpZhs8YDnuJjAtUaNYEqZDeOoKyiQ_KldeT_3NK", "피카츄" => "http://img.sbs.co.kr/newsnet/etv/upload/2013/04/07/30000259831_700.jpg", "고라파덕" => "http://image.chosun.com/sitedata/image/201402/04/2014020403444_0.jpg"}
    @name = params[:name]
    @result = array.sample
    @img = hash[@result]
    erb :randomgame
end
```
```erb
<!-- randomgame.erb file -->
<%=@name%>과 함께할 포켓몬은... <%=@result%> 입니다!
<img src='<%=@img%>'>

```

#### lotto 예제
```ruby
# app.rb
get '/lotto-sample' do
    
    #@lotto = (1..45).to_a.sample(6).sort
    @lotto = [1, 2, 15, 17, 23, 39]
    # rest-client를 사용하여 로또 당첨번호 받아오기
    url = "http://www.nlotto.co.kr/common.do?method=getLottoNumber&drwNo=809"
    @lotto_info =  RestClient.get(url)
    
    # 제이슨으로 파싱..
    @lotto_hash =  JSON.parse(@lotto_info)
    
    #
    @array = Array.new
    @bnus = nil
    @lotto_hash.each do |k, v|
        if k.include?("drwtNo")
            @array << v
        elsif k.include?("bnusNo")
            @bnus = v
        end
    end
    @matchNum = (@lotto&@array).length
    
    # 등수 매기기..
    # if문으로 작성 시..
    if @matchNum==6
        @rank=1
    elsif @matchNum > 2
        @rank = 8-@matchNum-(@lotto.include?(@bnus)?@matchNum/5:0)
    else
        @rank = '꽝'
    end
    
    # case 문으로 작성 시..
    @result = 
    case [@matchNum, @lotto.include?(@bnus)]
    when [6,false] then "1등"
    when [5,true] then "2등"
    when [5,false] then "3등"
    when [4,false] then "4등"
    when [3,false] then "5등"
    else "꽝"
    end
                        
    erb :lotto
end
```
```erb
<!-- looto.erb file -->
<h1>Hello world</h1>
<%= @lotto %> <br>
<%= @array.sort %> ... <%= @bnus %> <br>
맞은 갯수는 <%= @matchNum %>, 등수는 <%= @rank %> 입니다.
```

#### Redirect 예제
```ruby
# app.rb
get '/form' do
    erb :form
end

get '/search' do
    @keyword = params[:keyword]
    redirect "https://search.naver.com/search.naver?where=nexearch&sm=top_hty&fbm=1&ie=utf8&query=<%=@keyword%>"
end
```

#### Crawling 에제
```ruby
# app.rb
get '/opgg' do
	# 사용자 이름 검색 페이지
    erb :opgg
end

get '/opggresult' do
    uri = "http://www.op.gg/summoner/userName="
    @userName = params[:userName]
    
    # 한글 인코딩
    @encodeName = URI.encode(@userName)
    
    @res = HTTParty.get(uri+@encodeName)
    @doc = Nokogiri::HTML(@res.body)
    
    @win = @doc.css("#SummonerLayoutContent > div.tabItem.Content.SummonerLayoutContent.summonerLayout-summary > div.SideContent > div.TierBox.Box > div.SummonerRatingMedium > div.TierRankInfo > div.TierInfo > span.WinLose > span.wins").text
    @lose = @doc.css("#SummonerLayoutContent > div.tabItem.Content.SummonerLayoutContent.summonerLayout-summary > div.SideContent > div.TierBox.Box > div.SummonerRatingMedium > div.TierRankInfo > div.TierInfo > span.WinLose > span.losses").text
    @rank = @doc.css("#SummonerLayoutContent > div.tabItem.Content.SummonerLayoutContent.summonerLayout-summary > div.SideContent > div.TierBox.Box > div.SummonerRatingMedium > div.TierRankInfo > div.TierRank > span").text
    
    # File로 출력하기
    File.open('opgg.txt', 'w') do |f|
        f.write("#{@userName} : #{@win}, #{@lose}, #{@rank}\n")
    end
 
    # CSV로 출력하기
    CSV.open('opgg.csv', 'a+') do |c|
        c << [@userName, @win, @lose, @rank]
    end
    
    erb :opggresult
    
end
```
```erb
<!-- opgg.erb file -->
<form action="/opggresult">
    소환사 명 : <input name="userName"></input>
    <input type="submit" value="검색"></input>
</form>
```
```erb
<!-- opggresult.erb file -->
<pre>승리 : <%=@win%></pre>
<pre>패배 : <%=@lose%></pre>
<pre>랭크 : <%=@rank%></pre>
```
