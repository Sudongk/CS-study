# 페이징 (Paging)

* `Paging`은 프로세스의 가상 주소 공간을 **고정적인 크기(fixed-sized)로 나누어서 메모리에 할당**하는 방식이다. 이때 고정적인 크기 단위를 `page`라고 부른다.

* 그리고 이러한 가상공간의 page 를 실제 메모리에 할당할 때 이를 `page frame`이라고 부른다.

* 즉 가상 메모리(주소 공간)에서는 page, 실제 메모리(주소 공간)에서는 frame 이라고 불리는 고정 크기의 공간으로 프로세스를 실행하겠다는 의미이다.

<br>

## 페이징의 이점
* **유연성(Flexibility)** : 주소 공간의 추상화를 효율적으로 지원한다.

    * 얼마나 heap과 stack이 커지고 사용하는지에 대한 고민을 할 필요가 없다.

* **단순성(Simplicity)** : 여유 공간(free-space)의 관리가 쉽다.

    * 가상 메모리의 page와 page frame이 같은 크기를 가진다.

    * 할당이 쉽고 여유 공간을 유지한다.

* **외부 단편화(External Fragmentation) 문제 해결** : 고정 크기의 page로 외부 단편화 문제를 해결한다.

    * 외부 단편화란 메모리의 여유 공간이 작은 조각으로 나뉘어져 있는 상태를 의미한다.

![](/OS/img/os_paging_1.png)

* 위의 그림에서 4개의 page로 구성된 가상 주소 공간을 8개의 page frame으로 쪼갠 것을 볼 수 있다.

<br>

## Page Table

![](/OS/img/os_paging_2.png)

* Paging 기법을 사용하기 위해서 가상 주소 공간에 있는 프로세스의 `page`들이 실제 메모리의 어디에 위치를 하는 지를 기억하는 `Page table`을 가지고 있어야 한다.

* `Page table` 이란 가상 주소 공간의 page가 실제 메모리(주소 공간)에 어디에 위치해 있는 지를 기록한다.

    * Per-process structure(프로세스별 구조)

    * OS 메모리의 영역에 저장된다.

<br> 

## Paging 기법을 이용힌 주소 변환

* 가상 주소 공간에는 두 가지 요소가 있다.

    * VPN : **V**irtual **P**age **N**umber -> 상위 2개의 bit이 Page를 표현한다.

    * Offset : page 안에 있는 offset -> 나머지 4개의 bit이 offset을 표현한다.

* Page table을 통해서 Virtual Page, Physical Page frame 의 i번째를 찾는다.

    * VPN 에서 상위 2개의 bit 값이 1이므로 page 1에 해당한다.

* Offset을 통해 그 Page 안에 몇번째 메모리 주소인지 찾는다.

    * offset 값이 5이므로 16부터 시작해서 5번째인 21를 의미한다.
    
    * 따라서 가상 주소 공간의 1번째 page의 5번째 메모리 주소에 해당하는 실제 메모리 주소는 21이다.

* 가상 주소 공간에서 VPN 값을 Page Table을 통해 해당하는 값을 찾아서 실제 주소 공간의 PFN 값을 찾는다.

    * VPN 값 : 01 -> Page table 01에 해당 하는 값은 7 (= PFN 값)

    * PFN = Physical Frame Number (= PPN, Physical Page Number)

<br>

## Paging의 문제점

* **메모리 접근이 너무 잦아진다.**

    * 가상 주소 공간의 page를 실제 메모리의 frame으로 변환하는 과정이 매번 필요하다.

    * 이로 인해 메모리 접근이 너무 잦아져서 성능 저하를 일으킬 수 있다.

* **메모리 낭비가 발생할 수 있다.**

    * 실제 메모리에서 데이터를 가지고 오는데 주소 변환 정보를 알아야 하므로 추가적인 메모리 접근이 발생한다.

    * 이로 인해 메모리 낭비가 발생할 수 있다.

<br>

## TLB (Translation Lookaside Buffer)

* `TLB`는 `Page Table`의 일부를 캐시로 저장하는 하드웨어 장치이다.

* `TLB`는 가상 주소 공간의 page 번호를 실제 메모리의 frame 번호로 변환하는 데 사용된다.

* `TLB`를 통해 가상 주소 공간의 page 번호를 실제 메모리의 frame 번호로 변환하는 과정을 **TLB hit**이라고 한다.

* `TLB`를 통해 가상 주소 공간의 page 번호를 실제 메모리의 frame 번호로 변환하는 과정을 **TLB miss**이라고 한다.

<br>

## 정리

* Paging은 가변 크기 할당 방법인 segmentation의 외부 단편화 문제점을 해결하였고, 유연성을 가져와주는 장점을 가진다.

* 하지만, paging을 지나치게 사용한다면, 메모리 접근이 너무 잦아져서 성능 저하를 일으킬 수 있고,

* 실제 메모리에서 데이터를 가지고 오는데 주소 변환 정보를 알아야 하므로 추가적인 메모리 접근이 발생하여 메모리 낭비가 발생할 수 있다.

* 이러한 두 가지의 문제들을 보완하기 위해 나온 방법이 `TLB(Translation Lookaside Buffer)`가 있다.