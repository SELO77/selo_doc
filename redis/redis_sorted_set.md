# Implement Ranking System using by redis


## Introduction
레디스를 1년 넘게 사용했지만 자주 안쓰는 기능들은 까먹게 된다. 특히 랭킹보드와 같은 유사기능을 최소 2~3번은 구현했는텐데 기억이 나지 않는다. 그래서 간단한 기능을 정의하고 이를 구현해 보려한다.

--

간단한 게임 랭킹시스템을 구현할 것이다. 요구사항은 다음과 같다.

* 요구사항
	* 유저가 참여할 수 있는 어떤 게임이 있다. 유저는 스코어를 얻게되며, 얻은 스코어가 낮을 수록 높은 랭크이다.
	* 이 게임은 도시마다 지역 랭킹보드를 갖게된다. 랭킹보드는 100등 까지 유지한다.
	* 유저는 게임을 무한히 참여가능하다. 하지만 도시별 랭킹보드에는 유저의 최고점수만 표기되어야한다.


## Research Redis DataTypes

레디스는 몇 가지 유용한 자료형 제공 하며, 이는 개발자에게 매우 강력한 도구로 사용될수있다. 여기서는 레디스 자료형의 비교는 생략하겠다. 자세한 내용이 궁금하다면 최하단의 참조 링크를 살펴보자.

### Understanding Redis Sorted sets
> Redis Sorted Sets are, similarly to Redis Sets, non repeating collections of Strings. The difference is that every member of a Sorted Set is associated with score, that is used in order to take the sorted set ordered, from the smallest to the greatest score. While members are unique, scores may be repeated.

요약 > `Sorted Sets`은 고유한 값을 갖는 레디스의 `Sets`과 비슷한 자료형이다. 차이는 모든 집합 맴버가 `Score`를 갖으며 이는 순서를 갖게 된다. 


> Sorted sets are a data type which is similar to a mix between a Set and a Hash. Like sets, sorted sets are composed of unique, non-repeating string elements, so in some sense a sorted set is a set as well.
However while elements inside sets are not ordered, every element in a sorted set is associated with a floating point value, called the score (this is why the type is also similar to a hash, since every element is mapped to a value).

요약 > `Sorted Sets`은 레디스 자료형 집합과 해쉬의 혼합 자료형이다. 구성 맴버들(유저라 생각하면 이해가 쉽다)은 유니크하다. 그리고 각 구성 맴버들은 `Score`라 불리는 `float point`값과 관계를 갖으며, 맵핑되어있다.

정리하자면 특정 키에 해당하는 중복되지 않는 맴버들이 존재하고(`Set`과 유사), 각 맴버들은 `Score(Float Value)`를 갖는다(`Hash`와 유사).

마지막으로 요구사항과 비교 정리해보자. 도시는 특정 키로 구분하며, 게임 참여 유저를 해당 키의 맴버로 구성하며, 각 유저는 스코어를 갖는다. 마지막으로 스코어에 따라 유저는 순서(Ranking)를 갖게 된다. 와우!!! 완벽해!!!


## Tutorial of SortedSet
위 요구사항을 해결하기 위한 `SortedSet`의 기능(명령어)를 살펴보자.

### Define Key Format
Key Format은 다음과 같으며 `msqr:<city_id>`, City의 ID 값이 1인 경우 `msqr:1`이 될 것이다.


#### redis server start
```
> redis-server 
```

#### run redis-cli
```
> redis-cli
```
--

### Commands

#### `ZADD`: Adds one or more members to a sorted set, or updates its score, if it already exists
맴버를 추가 혹은 갱신. create or update

> Time complexity: O(log(N)) for each item added, where N is the number of elements in the sorted set.

이진탐색 후 삽입인듯?

```
127.0.0.1:6379> ZADD msqr:1 59 "SELO"
(integer) 1
127.0.0.1:6379> ZADD msqr:1 55 "BOB"
(integer) 1
127.0.0.1:6379> ZADD msqr:1 100 "BOT"
(integer) 1
127.0.0.1:6379> ZADD msqr:1 30 "QUZZ"
(integer) 1

```

두명이상의 맴버가 같은 `Score`를 받은 경우 이름순으로 정렬된다.

```
127.0.0.1:6379> ZADD msqr:15 55 "AA1"
(integer) 1
127.0.0.1:6379> ZADD msqr:15 55 "AA2"
(integer) 1
127.0.0.1:6379> ZADD msqr:15 55 "AA3"
(integer) 1

# 내침차순 경우
127.0.0.1:6379> ZRANGE msqr:15 0 -1 
1) "AA1"
2) "AA2"
3) "AA3"

# 오름차순 경우
127.0.0.1:6379> ZREVRANGE msqr:15 0 -1
1) "AA3"
2) "AA2"
3) "AA1"

```


#### `ZRANGE`: list rank
범위에 해당하는 값들을 반환한다.

> Time complexity: O(log(N)+M) with N being the number of elements in the sorted set and M the number of elements returned.

위치를 찾는데 O(log(N)), 범위내의 엘리먼트 갯수 O(M) 상수시간 만큼 걸린다.


```
127.0.0.1:6379> zrange msqr:1 0 -1
1) "QUZZ"
2) "BOB"
3) "SELO"
4) "BOT"

127.0.0.1:6379> zrange msqr:1 0 -1 withscores # with score
1) "QUZZ"
2) "30"
3) "BOB"
4) "55"
5) "SELO"
6) "59"
7) "BOT"
8) "100"
```

#### `ZRANK`: return the rank of member

> Time complexity: O(log(N))

```
127.0.0.1:6379> zrank msqr:1 "BOB"
(integer) 1
127.0.0.1:6379> zrank msqr:1 "SELO"
(integer) 2
```

#### `ZREMRANGEBYRANK `: removes all elements with rank between start and stop

> Time complexity: O(log(N)+M) with N being the number of elements in the sorted set and M the number of elements removed by the operation.
역시나 이진탐색 후 삭제 갯수(M)로 보임


```
127.0.0.1:6379> ZADD msqr:1 99 "PAUL"
(integer) 1
127.0.0.1:6379> ZADD msqr:1 80 "JAY"
(integer) 1
127.0.0.1:6379> ZRANGE msqr:1 0 -1 # 현재 SortedSet 맴버
1) "QUZZ"
2) "BOB"
3) "SELO"
4) "JAY"
5) "PAUL"
6) "BOT"
127.0.0.1:6379> ZREMRANGEBYRANK msqr:1 1 2 # 1~2 삭제
(integer) 2 # 삭제된 맴버 수 반환
127.0.0.1:6379> ZRANGE msqr:1 0 -1
1) "QUZZ"
2) "JAY"
3) "PAUL"
4) "BOT"
127.0.0.1:6379> ZREMRANGEBYRANK msqr:1 1 1 # 1~1 삭제
(integer) 1 # 삭제된 맴버 수 반환
127.0.0.1:6379> ZRANGE msqr:1 0 -1
1) "QUZZ"
2) "PAUL"
3) "BOT"
```

#### `ZREVRANGE `: returns keys of members with range ordered by descending

요청된 키의 검색 순위에 해당하는 맴버를 내림차순으로 정렬된 상태로 반환

> Time complexity: O(log(N)+M) with N being the number of elements in the sorted set and M the number of elements returned.

```
127.0.0.1:6379> ZREVRANGE msqr:1 0 -1 WITHSCORES
1) "BOT"
2) "100"
3) "PAUL"
4) "99"
5) "QUZZ"
6) "30"
```


## Implement
위에서 살펴본 기능들을 기반으로 요구사항을 구현해보자.
Key Foramt은 위와 같은 방식(`msqr:<city_id>`)으로 사용하겠다.

PS. 해당 구현은 Best Practice 가 아니며, 이해를 위한 예시입니다.


#### 기본 기능 구현
* 맴버가 게임참여 스코어를 받는다.
* 검색된 도시의 게임 참여자와 점수를 100등까지 출력한다.

```
# 서울의 City ID 는 11이다.
# CHULSU가 서울에서 게임을 참여 했고, 5점의 스코어를 받았다.
> ZADD msqr:11 5 CHULSU
(integer) 1

# SELO가 서울에서 게임을 참여 했고, 15점을 받았다.
> ZADD msqr:11 15 SELO
(integer) 1

# PAUL이 서울에서 게임을 참여 했고, 15점을 받았다.
> ZADD msqr:11 50 PAUL
(integer) 1

# 위와 같이 유저가 게임에 참여, 유저이름과 스코어가 저장된다.
(중략)
...

# 서울의 100등까지의 맴버와 받은 스코어 출력
> ZRANGE msqr:11 0 99
```


#### 상세 기능 구현
* 유저의 최고점수 갱신
* 불필요한 랭킹 제거


```
# CHUSU가 서울에서 게임에 참여, 1점의 스코어를 받았다.
# 이전 스코어 확인
> ZSCORE msqr:11 CHULSU
"5"
# 현재 스코어가 1이며, 기존 스코어 5를 갱신한다. (값이 낮을 수록 높은 랭크이다.)
> ZADD msqr:11 1 CHULSU
(integer) 0

# 랭킹보드의 맴버가 101개 이상인 경우에는 101등 이후의 맴버는 랭킹보드에서 삭제한다.
> ZREMRANGEBYRANK msqr:11 100 -1
(integer) <number of member removed>
```


## 정리
`SortedSet`를 사용해서 간단한 랭킹 시스템을 구현해 보았다. 너무 간단하지 않은가??!! 그렇다. 간단하게 생각하면 간단하게 구현된다. 하지만 실제 Application Level 에서의 구현은 **`레디스서버로 요청횟수를 줄이기 위한 파이프라인 기능 사용`**, **`데이터 혹은 맴버 크기에 따른 시간 복잡도`** 등 과 같이 고려해야할 사항이 많아진다.

그래도 레디스를 캐쉬로 쓰면서 항상느끼는 점은 **"레디스가 없다면 얼마나 고통스러울까?"** 라는 겪고 싶지 않은 의문과 **"레디스 짱짱맨!!"** 이라는 결론이 난다. 정리하자면 레디스라는 강력한 도구를 잘 사용하면 복잡도가 높은 기능들을 매우빠르고 쉽게 구현할 수 있다. ***"I LOVE YOU REDIS SO MUCH!!"*** 순도 100프로의 진심어린 외침과 함께 포스팅을 마친다.


## Reference
* [An introduction to Redis data types](https://redis.io/topics/data-types-intro)
* [Tutorials Point: Redis Sotred Sets](https://www.tutorialspoint.com/redis/redis_sorted_sets.htm)


**잘못된 부분은 언제든지 댓글로 혼내주세요!!**



