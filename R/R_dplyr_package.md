## R 언어

### dplyr 패키지 함수 요약
---

#### 1. 조건에 맞는 데이터만 추출하기
```
exam %>% filter(english >= 80)
```
  
##### 여러 조건 동시 충족
```
exam %>% filter(class == 1 & math >= 50)
```

##### 여러 조건 중 하나 이상 충족
```
exam %>% filter(math >= 90 | english >=)
exam %>% filter(class %in% c(1, 3, 5))
```

#### 2. 필요한 변수만 추출하기
```
exam %>% select(math)
exam %>% select(class, math, english)
```

#### 3. 함수 조합하기, 일부만 출력하기
```
exam %>%
	select(id, math) %>%
    head(10)
```

#### 4. 순서대로 정렬하기
```
exam %>% arrange(math)				# 오름차순 정렬
exam %>% arrange(desc(math))		# 내림차순 정렬
exam %>% arrange(class, math)		# 여러 변수 기준 오름차순 정렬
exam %>% arrange(desc(class),desc(math)) # 여러 변수 기준 내림차순 정렬
```

#### 5. 파생변수 추가하기
```
exam %>% mutate(total = math + english + science)
```

##### 여러 파생변수 한 번에 추가하기
```
exam %>%
	mutate(total = math + english + science,
    mean = total/3)
```

##### mutate()에 ifelse()적용하기
```
exam %>% mutate(test = ifelse(science >= 60, "pass", "fail"))
```

##### 추가한 변수를 dplyr 코드에 바로 활용하기
```
exam %>%
	mutate(total = math + english + science) %>%
    arrange(total)
```

#### 6. 집단별로 요약하기
```
exam %>%
	group_by(class) %>%
    summarise(mean_math = mean(math))
```

##### 각 집단별로 다시 집단 나누기
```
mpg %>%
	group_by(manufacturer, drv) %>%
    summarise(mean_cty = mean(cty))
```

#### 7. 데이터 합치기
##### 가로로 합치기
```
total <- left_join(test1, test2, by = "id")
```
##### 세로로 합치기
```
group_all <- bind_rows(group_a, group_b)
```


