# Garbage Collection
- JAVA의 특징 중 하나로 메모리 관리를 개발자가 직접 명시적으로 수행하지 않고, JVM에서 자동으로 수행해준다.
- 이와 같은 역할을 하는 프로세스를 Garbage Collector라 한다.
- Garbage Collector는 Heap 영역에 생성된 객체 중 참조되지 않는 객체를 탐지하여 제거한다.
- 이를 통해 개발자는 메모리 관리에 대한 부담을 덜 수 있고, 더욱 효율적인 프로그래밍을 할 수 있다.
- 하지만 Garbage Collector가 모든 메모리 관리를 대신해주는 것은 아니기 때문에 메모리 누수에 대한 주의가 필요하다.
- 또한 Garbage Collector가 동작하는 동안에는 프로그램이 일시적으로 멈추는 현상(Stop-The-World)이 발생할 수 있으므로 이 역시 고려해야 한다.
- Garbage Collector는 다양한 알고리즘을 사용하여 객체를 관리하는데, 이 중에서도 대표적인 알고리즘으로는 Mark-and-Sweep, Copying, Generational 등이 있다.
- 이러한 Garbage Collector의 동작 방식을 이해하고, 적절한 알고리즘을 선택하여 사용함으로써 메모리 관리에 대한 효율을 높일 수 있다.

<br>

## GC의 역할
1. 메모리 할당
2. 사용 중인 메모리 인식
3. 사용하지 않는 메모리 인식

<br>

## GC 수행 영역별 구분
#### JVM 메모리 영역
JVM의 메모리는 크게 클래스 영역, 자바 스택, 힙, 네이티브 메소드 스택 의 4가지 영역으로 이루어져있다.

GC는 JVM의 메모리 영역 중 'Heap' 메모리에서 이루어진다.
<br>(Java에서 새로운 객체가 할당될 때, Heap 메모리 영역에 할당되기 때문)

![](/Java/img/java_gc_01.png)

이 Heap 영역은 세 영역으로 나누어진다. 
- New/Young 영역 : 생성된지 얼마 안된 객체를 저장, 시간이 지남에 따라 우선순위가 낮아지면 Old 영역으로 옮겨진다.
- Old 영역 : 생성된지 오래된 객체를 저장
- Perm 영역 : Class, Method 등의 코드가 저장되는 영역, 자바 언어 레벨에서 사용되지 않음

<br>

### Minor GC
New/Young 영역의 GC를 `Minor GC`라고 부르며, 이에 사용되는 알고리즘을 **`Copy & Scavenge`** 라고 한다.
속도가 매우 빠르고, 작은 크기의 메모리를 콜렉팅하는데에 효과적이다.
<br> 자주 일어나기 때문에 시간이 짧은 알고리즘이 적합하다.

이 영역은 `Eden`/`Survivor` 이라는 두 영역으로 나뉘어진다.
- Eden : 
	- 자바 객체가 생성되자마자 저장되는 영역
	- 객체가 계속 생성되어 `Eden`영역이 다 차게 되면, `Minor GC`가 발생, 살아남은 객체는 `Survivor`영역 중 하나로 이동
- Survivor :
	- Survivor1 과 Survivor2로 나누어짐
	- 한 `Survivor`영역이 가득차게 되면 그 중 살아남은 객체를 다른 `Survivor`영역으로 이동
	- 가득찬 `Survivor`영역은 아무 데이터가 없는 상태가 됨
	- `Survivor` 영역 중 하나는 반드시 비어있는 상태이어야함
	- 더 큰 객체가 생성되거나, 더 이상 `Young`영역에 공간이 남지 않으면 `Survivor`영역에서 오래된 객체를 `Old`영역으로 이동(이를 `Promotion`이라고 칭함)

> **Object Aging**
>
> Survivor영역에서 Survivor영역으로 이동할 때 마다 객체의 살아남은 횟수를 나타내는 age값을 증가시킨다.
> Object Header에 기록하며, age를 보고 Minor GC때 Old 로 이동(Promotion)을 결정한다.

<br>

### Major GC(Full GC)
Old 영역의 GC를 `Major GC`라고 부르며, 이에 사용되는 알고리즘을 `Mark & Compact`라고 한다.
Minor GC 과정에서 삭제되지 않고, Old 영역으로 옮겨진 객체 중 미사용되는 객체를 메모리에서 삭제한다.
객체들의 참조를 확인하면서 참조가 연결되지 않은 객체를 표시하고 표시된 객체들을 삭제함
<br>
Full GC는 속도가 매우 느리며, Full GC가 일어나는 도중에는 순간적으로 자바 애플리케이션이 멈춰버리기 때문에 Full GC가 일어나는 빈도와 시간은 어플리케이션의 성능과 안정성에 아주 큰 영향을 미친다.

<br>

### GC 발생 시나리오

![](/Java/img/java_gc_02.png)
1. 객체가 생성되면 `Eden`영역에 위치하게 된다.
<br>

![](/Java/img/java_gc_03.png)
2. `Eden`영역이 가득차게 되면 `Minor GC`가 발생하여 참조가 없는 객체는 삭제되고, 참조 중인 객체는 `Survivor` 영역으로 이동한다.
<br>

![](/Java/img/java_gc_04.png)
3. `Survivor`영역이 가득차게 되면, `Minor GC`가 발생하여 참조가 없는 객체는 삭제되고, 참조 중인 객체는 다른 `Survivor`영역으로 이동한다.
<br>

![](/Java/img/java_gc_05.png)
4. `Survivor`영역에서 GC과정을 반복하며, 계속 참조 중인 객체는 `Old`영역으로 이동한다.
<br>

![](/Java/img/java_gc_06.png)
5. `Eden`영역에서 `Survivor`영역으로 이동할 때 객체가 남아있는 영역보다 클 경우 `Old`영역으로 이동한다.
<br>


## GC의 동작방식

`Young`영역과 `Old`영역은 서로 다른 메모리 구조로 되어있기 때문에, 세부적인 동작 방식은 다르다.
하지만 기본적으로 GC가 수행된다고 하면 다음의 2가지 공통적인 단계를 따르게 된다.

### Stop The World(STW)
- GC 실행을 위해 JVM이 애플리케이션의 실행을 멈추는 작업

- GC가 실행될 때는 GC를 실행하는 Thread를 제외한 모든 Thread들의 작업이 중단되고, GC가 완료되면 작업이 재개된다.

모든 Thread의 작업이 중단되면 애플리케이션이 멈추기 때문에, GC의 성능 개선을 위해 튜닝을 한다고 하면 보통 STW의 시간을 줄이는 작업을 하는 것이다. 또한, JVM에서도 이러한 문제를 해결하기 위해 다양한 실행 옵션을 제공하고 있다.

### Mark and Sweep
- 참조되는 변수를 구별(Mark), 참조되지 않는 변수를 해제(Sweep)
- STW를 통해 모든 작업이 중단되면, GC는 스택의 모든 변수 또는 reachable 객체를 스캔해 각각이 어떤 객체를 참고하고 있는지 탐색한다. 그리고 사용되고 있는 메모리를 식별(Mark), Mark되지 않은 객체들을 메모리에서 제거(Sweep).

> Mark : 사용되는 메모리와 사용되지 않는 메모리를 식별하는 작업
>
> Sweep : Mark 단계에서 사용되지 않음으로 식별된 메모리를 해제하는 작업

### Compact
> Compaction : 객체들을 가까운 곳으로 모으고, heap 메모리의 아래쪽으로 보냄

![](/Java/img/java_gc_07.png)

- 한 객체가 더 이상 필요 없게 되어 가비지 컬렉션의 대상이 되더라도, 그 바로 옆에 있는 객체가 여전히 사용 중일 수 있다.
- GC과정에서 여러 객체가 메모리의 다른 위치에서 해제되면, 메모리는 여러 개의 작은 빈 공간, 즉 '파편'으로 나뉘게 되는데 이러한 파편화는 메모리를 비효율적으로 사용하게 만들 수 있다.
- 파편화를 최소화하기 위하여 JVM은 Major GC 때마다 Old 영역에서 Compaction작업을 수행한다.

<br>

## GC가 중요한 이유
Minor GC의 경우 보통 대략 0.5초에서 1초 이내에 끝나기 때문에 큰 문제가 되지 않지만, 
<br>Full GC의 경우 시간이 많이 소요되며, 자바 애플리케이션이 멈춰버릴 수 있기 때문에 문제가 생긴다.

멈춘 시간동안 사용자의 요청이 Queue에 들어있다가, 순간적으로 요청이 한꺼번에 들어오기 때문에 과부하에 의해 여러 장애를 만들 수 있다.

따라서 원활한 서비스를 위해서 GC가 어떻게 일어나게 하느냐가 시스템의 안정성과 성능에 큰 변수로 작용한다.

<br>

## GC 알고리즘
- JVM 버전 변화에 따라 여러가지 GC방식이 추가되고 발전되어 왔다.
- 버전 별로 지원하는 GC는 차이가 존재하며, 서비스 상황에 따라 필요한 GC방식을 JVM 옵션을 통해 설정으로 사용 가능하다.

## Serial GC

![](/Java/img/java_gc_10.png)
<br>
- 주로 32bit JVM에서 돌아가는 `Single Thread` 애플리케이션에서 사용 (별도로 지정하지 않을 경우 기본GC)
- `Minor GC`, `Major GC` 모두 올스탑(`stop-the-world`)하는 Single Thread 방식
- `Mark-sweep-compact` 알고리즘
	- Mark : 'Old' 영역으로 이동된 객체들 중 살아있는 객체를 식별한
	- Sweep : 'Old' 영역의 객체들을 훑는 작업을 수행하여 쓰레기 객체 식별
	- Compaction : 살아남은 객체들이 연속되도록 heap의 가장 앞부분부터 채워서 객체가 존재하는 부분과 없는 부분으로 나눈다.

<br>

## Parallel GC (=Throughput GC)

![](/Java/img/java_gc_11.png)
<br>

- 기본적으로 Serial GC와 동작과정은 동일, Minor GC에서 사용되는 Thread 수를 늘릴 수 있음
- 일반적으로 Minor GC만 멀티 스레딩, Major GC는 싱글 스레딩으로 수행한다.
- Multi Thread 사용
- GC의 오버헤드는 상당히 줄여주었지만, 애플리케이션이 멈추는 것은 피할 수 없음
- 메모리가 충분하고 코어의 개수가 많을 때 유리
- Java 8의 기본 GC

<br>

## Parallel Old GC
- Parallel GC와 비교하여 Old 영역의 알고리즘만 다름(`Major GC`도 멀티 스레딩)
- Mark-Summary-Compaction 단계
	- Summary 단계는 앞서 GC를 수행한 영역에 대해 별도로 살아있는 객체를 식별한다는 점에서 Mark-Sweep-Compaction 단계와 다르며, 조금 더 복잡함
	- Mark-Sweep-Compact :  단일 스레드가 Old 영역 검사
	- Makr-Summary-Compact : 여러 스레드가 Old 영역 검사
	- Mark : Old 영역을 region별로 나누고 region별로 살아있는 객체를 식별
	- Summary : region별 통계정보로 살아있는 객체의 밀도가 높은 부분이 어디까지 인지 dense prefix를 정한다. 오랜 기간 참조된 객체는 앞으로 사용할 확률이 높다는 가정하에 dense prefix를 기준으로 compact영역을 줄인다.
	-  Compact : destination과 source로 나누며 살아있는 객체는 destination으로 이동시키고 참조되지 않는 객체는 제거

<br>

## CMS Collector (Concurrent Mark-Sweep)

![](/Java/img/java_gc_12.png)
<br>

`Major GC`에 소요되는 작업을 애플리케이션을 멈추고 진행하는 것이 아니라, 일부는 애플리케이션이 돌아가는 단계에서 수행하고 최소한의 작업만을 애플리케이션이 멈췄을 때 수행하는 방식

- Initial Mark 단계 : 클래스 로더에서 가장 가까운 객체 중 살아있는 객체만 찾는 것으로 끝냄(멈추는 시간이 매우 짧다)
- Concurrent Mark 단계 : 참조상태 객체를 확인, `stop-the-world 없이` 다른 스레드가 실행 중인 상태에서 동시에 실행된다.
- Remark 단계 : Concurrent 단계에서 새로 추가되거나 참조가 끊긴 객체를 확인
- Concurrent Sweep 단계 : 참조 되지 않은 객체를 정리(쓰레기 정리), 다른 스레드가 실행되고 있는 상태에서 진행 됨
- stop-the-world 시간이 매우 짧음
	- 응답속도가 매우 중요할 때 사용
	- `Low Latency GC`라고도 불림
- 단점 :
	- GC 과정이 매우 복잡 메모리와 CPU 많이 사용
	- 메모리 파편화

<br>

## G1 Collector (Garbage First)

![](/Java/img/java_gc_08.png)
<br>

- Heap 메모리 영역을 나누어 관리
- 기존 Young, Old 영역의 개념과 다른 Heap에 `Region` 개념을 도입함
- 상황에 따라 Eden, Survivor, Old 동적으로 부여
- 메모리 전체 탐색이 아닌 가장 많은 가비지를 포함하는 Region부터 GC 시작
- CPU 리소스 및 메모리 파편화 단점 해결
- Java 9부터 기본 GC

<br>

## ZGC (Z Garbage Collector)
대용량 힙 메모리를 효율적으로 관리하고, STW 시간을 최소화를 목표로 Java 11부터 공개	
- 대용량 힙 메모리 지원(8MD ~ 16TB)
- G1의 Region 처럼, ZGC는 ZPage라는 영역을 사용하며, G1의 Region은 크기가 고정인데 비해, ZPage는 2mb 배수로 동적으로 운영
- 처리 시간이 10ms를 초과하지 않아 짧은 지연시간을 보장