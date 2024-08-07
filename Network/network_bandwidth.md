# 대역폭 (Bandwidth)

- 네트워크에서 대역폭은 네트워크의 성능을 나타내는 중요한 지표 중 하나이다.
- 네트워크에서 대역폭이란 단위시간당 전송할 수 있는 데이터의 최대 용량을 의미로, 초당 전송되는 데이터의 양을 나타낸다.
- 네트워크의 대역폭이 클수록 더 많은 데이터를 빠르게 전송할 수 있다.
- 네트워크 속도가 대역폭값에 근접할 경우 대역폭을 늘리면 속도가 빨라질 가능성이 있으나 무조건적인 조건은 아니다.
- 데이터 처리량과 네트워크 성능, 속도에 큰 영향을 미치지만 실제로 대역폭은 `용량Capacity`과 더 밀접한 관계를 가지고 있다.

<br>

## 1. 단위 [bps]

- **`bps`** bits per second 초당 '비트'수를 나타내는 단위
> - BPS, Bps는 Bytes per second

<br>

## 2. 유/무선 인터넷에서 대역폭

- 유선 인터넷에서 주파수 대역폭

> - 인터넷 회선 약정시 500M급 100M급 인터넷이라고 표현하는데 이는 데이터 전송 대역폭을 표현
> - 10Gbit 이더넷을 사용하더라도 네트워크 상황에 따라 성능이 저하될 가능성이 있다.

- 무선 인터넷에서 주파수 대역폭

> - 흔히 사용하는 Wifi는 통상 2.4GHz(2.4\~2.462GHz), 5GHz(5.180\~5.850GHz) 두가지 주파수를 사용한다.
> - 간혹 미디어나 일상에서 2.4GHz 대역폭, 5GHz 대역폭이라고 표현하는데 이는 틀린 표현

<br>

## 3. 네트워크 처리량과 대역폭 관계

- 처리량은 단위시간당 실제로 처리되는 데이터의 양을 나타내고 대역폭 용량을 초과할 수 없다.

> - 물탱크에서 수도까지 펌프를 통해 물이 흐르는 파이프가 있다고 가정했을때 흐르는 물의 양은 처리량, 파이프를 통해 최대로 흐를 수 있는 물의 양이 대역폭 
> - 파이프를 100% 다 사용하는 경우 파이프 내경이 클수록 더 많은 물이 흐르지만 언제나 100% 다 사용하지 못하는것과 같은 것

<br>

## 4. 대역폭에 따른 병목현상 발생 가능성

- 대역폭이 좁은 경우 데이터 전송이 느려지는 현상을 `병목현상`이라고 한다.
- 100Mbps 대역폭을 가진 네트워크에서 1Gbps 데이터를 전송하려고 할 때 병목현상이 발생할 수 있다.
- 다수 노드로부터 데이터를 받아야하는 서버의 경우 대역폭이 좁아 병목현상이 발생할 수 있다.

> **노드**
> - 네트워크에서 데이터를 보내거나 받는 장치
> - 서버, 클라이언트, 라우터, 스위치 등

**해결방법**
- 네트워크 업그레이드 -> 대역폭을 늘려 데이터 전송 속도를 높인다.
- 데이터를 받는 서버를 여러대로 분산

> 모바일/웹 사용자 인증을 위해 사용되는 JWT 토큰의 길이가 길어질수록 매 통신마다 큰 데이터를 주고 받아야하므로 대역폭의 낭비가 심화될 수 있다.