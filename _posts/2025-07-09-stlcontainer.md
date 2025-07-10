---
title: C++ sequence ontainers
date: 2025-07-09
permalink: /2025/07/09/1
tags:
        - c++
---
c++ 표준라이브러리에서 제공하는 자료구조 중 sequences container에 대해 정리하였다.
과거에 배웠던 객체지향설계의 수업 내용을 일부 참고하여 작성하였다. 부족하면 그때 수업 자료를 뒤져보자.

---

c++에서는 standard template library에서 각종 유용한 알고리즘, 자료구조를 제공
그중 자료구조 > container.

## array

고정길이, 연속된 공간.

**기존 방식(c)**

배열을 argument로 > 배열 시작 포인터
길이를 모름.
배열 길이를 넘어가는 인덱스 접근 > Undefined behavior(위험)

```c++
#include <array>

std::array<int, 3> arr;
		  <type, length>

arr.size()

```

길이 알고 있음.
"[]" 연산자 (array subscription), at(index)로 원소 접근
전자는 잘못된 인덱스 접근 시 오류 x 후자는 o(런타임에러 후 종료)
why? > 자신있으면 앞에거 쓰라고. (오버헤드 없음.)
fixed length. > 고정길이로 뭔가를 다룬다면 최고.

템플릿 길이에 변수 못넣음 (템플릿은 스태틱)

## vector

= expandable 한 array = 넣으면 늘어남.

```c
#include <vector>

std::vector<int> vec;
           <type>//length > expandable
vec.push_back(3); //add 3 at end
vec.begin(); // *vec
ver.capacity(); //available size
ver.size(); //count of element

```

연속적인 공간 사용
장점:
(+)randon access > 상수시간
따라서 array subscription 사용가능.
\*(a+4) 같은 접근도 가능 (완전히 연속적 나열 포인터로도 ㅇㅋ).
단점:
길이 부족 > 길이만큼 공간 확보해서 다시 옮겨야 (overhead!)

(-)잦은 크기수정, reordering은 성능 bad.
앞이나 중간 삽입 > 뒷 원소 다 뒤로 밀어야(너무큰 overhead) > 자체 연산 지원 x.
저장해야하는 값의 범위가 너무 넓을때(ex)1에서 1억개 언저리?) > 곤란

처음부터 크기할당하고 쓰는경우 굿

### deque

=리스트 벡터 중간

c++에서는 잘 안씀.

```c
#include <deque>

std::deque<int> deque;
deque.push_front(3);
deque.push_back(4);
deque[3];

```

청크단위 연속공간 점유, 청크차면 다른 청크공간 찾기.(청크들간 링크드 리스트 ~= 어레이들의링크드 리스트 )
청크들 청크 테이블로 관리.

- vector처럼 옮길일 없음.
- 완벽x 랜덤 access는 가능.(벡터 이하)
  \>array subscription 오버로딩해서 구현됨, 실제 연속 나열아니니 \*(a+4) 처럼은 사용불가.
- 리오더링과 리사이징 중간 삽입에서의 vector의 더나은 대안.
- 앞뒤 삽입 O(1) (단순히 앞, 뒤용 청크 새로확보 후 삽입)

### list

```c++
#include <list>

std::list<int> vec;
        <type>//without capacity
vec.push_back(3); //add 3 at end
vec.push_front(3); //add 3 at front
std::next(vec) // next pointer of vec

```

=자바 linkedlist
list를 이용 (자바의 linkedlist에 해당.)

확장이 굉장히 빈번한 경우 사용. 그러나 랜덤억세스 불가(트레버서 필요)

double linked list로 구현됨.

이터레이팅 할때 양방향이동이 필요하다면 굿
장점:
vector처럼 옮기고 저장 필요 x. >capacity랄 것이 없음
앞 뒤 원소 추가 상수시간.
단점:
random access 부적합 (맨앞,뒤 차례로 찾아가기 O(n))
\> array subscription.(오버라이딩x, 비효율)  \*(a+4)  모두 불가.

gpt에게 계속 묻기도 좀 믿음이 안가서 c++에게 검색어를 질문해서 이에 검색했다.
the most used c++ STL container

container도 종류가 잇다.

sequences container로  array, vector, deque, foward_list, list. (혹은 문자열까지)
associative container > 셋 맵,

continer adaptor > 시퀸스 컨테이너를 특정용도로 쓰려고 특정방법으로 작동하도록 강요한 것. stack , queue

unordered associative containers.
순서없는 associative containers 순서가 없으니 속도가 빠르다

### 스택,큐

데크의 억세스를 일부로 제한 시킨 것
속도를 올리고 싶다면 다른 컨테이너로 만들수 있음.

[각 컨테이너별 지원하는 연산 목록](https://en.cppreference.com/w/cpp/container)

이곳에서 어떤 자료구조가 어떤 것을 가지는지 , 그리고 자료구조의 overview 에 대한 정보 + 함수 정보로 특정 자료구조에서 다른 자료구조로 어떻게 바꿀지.

![image.png](/postPic/25-07-09/1.png)

[참고한 유튜브 영상](https://www.youtube.com/watch?v=-SheBq9XY3s)

?트리에 대한 복습.(레드블랙 트리…)

## 각 자료 구조들의 사용 경향

벡터가 가장 대중적으로 쓰인다. (양끝단에서의 삽입이 가장 빠르다)

크기가 매우 가변적이면 deque가 쓰인다.

중앙에서의 삽입이 빈번한 매우 소수의 경우에만 리스트가 쓰인다.

링크드 리스트는 modern hardware 환경에 매우 부적합하다.
캐시 친화적이지 못함 > 모든 데이터가 메모리 여기저기에 흩뿌려져 있다. > locality bad.

그럼 링크드리스트는 정말 쓸모가 없는가?
modern 하드 웨어에서 bad 일뿐. 또 다른 성격의 하드웨어기기에서 사용될 수 있음. (c++은 다양한 환경에서 사용할 것을 고려해 설계.

invalidated iterator? 에관해서는 list가 쓰인다고함.

언제 벡터 대신 덱을 사용하는가?
크기를 너무 예상 할 수 없음.
안정적인 삽입/삭제 성능을 원함.
vector는 중간 삽입, 크기가 커질 시 재할당이 필요함.
