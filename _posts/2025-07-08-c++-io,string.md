---
title: C++ 기초 입출력, 문자열
date: 2025-07-08
permalink: /2025/07/08/2
tags: 
  - c++
---
C++ 원할한 사용을 위해 어떤언어들 가장 많이 쓰는 문자열, 입출력 내용을 정리하였다. GPT가 간단히 정리해준 내용을 기반으로 실제 이것이 맞는 정보인지, 이유는 무엇인지 찾아보았다.

# 📌 C++ 문자열 `std::string` 핵심 정리

C++에서 가장 자주 쓰이는 표준 클래스 `std::string`에 대해 실용성과 깊이를 모두 고려한 요약.

---

## ✅ 기본 개념

- `std::string`은 `std::basic_string<char>`의 typedef.
- `<string>` 헤더 필요. `std::` 네임스페이스에 있음.
- 내부적으로 **가변 크기 동적 메모리 + 연산자 오버로딩** 기반.
- C-스타일 문자열 (`char*`)과는 다르게 메모리 자동 관리됨.

---

## ✅ 자주 쓰이는 생성자

```cpp
std::string a;               // 기본 생성자 (빈 문자열)
std::string b("hello");      // C-string 기반 초기화
std::string c(5, 'x');       // "xxxxx"
std::string d(b);            // 복사 생성자
std::string e(std::move(b)); // 이동 생성자 (C++11~)

```

## ✅ 자주 쓰이는 멤버 함수


| 함수                     | 설명                                        |
| -------------------------- | --------------------------------------------- |
| `size()`, `length()`     | 문자열 길이 (자주씀)                        |
| `empty()`                | 비었는지 확인(이유는 모르지만 0과 1로 나옴) |
| `at(i)`                  | i번째 문자 (예외 체크 O) (자주씀)           |
| `operator[]`             | i번째 문자 (예외 체크 X)                    |
| `append(str)` / `+=`     | 문자열 이어붙이기                           |
| `insert(pos, str)`       | 삽입                                        |
| `erase(pos, len)`        | 삭제                                        |
| `replace(pos, len, str)` | 부분 교체                                   |
| `substr(pos, len)`       | 부분 문자열 추출                            |
| `find(str)`              | 처음 등장 위치 반환 (없으면`npos`)          |
| `compare(str)`           | 정렬 기준 비교                              |
| `c_str()`                | C-style 문자열 포인터 반환                  |
| `clear()`                | 내용 비우기                                 |
| `reserve(n)`             | 힙 재할당 줄이기용 버퍼 예약                |

---

## ✅ 자주 쓰이는 연산자


| 연산자                  | 설명                                   |
| ------------------------- | ---------------------------------------- |
| `+`, `+=`               | 문자열 이어붙이기                      |
| `==`, `!=`, `<`, `>` 등 | 문자열 비교                            |
| `<<`, `>>`              | 입출력 스트림 연산자 (`iostream` 기반) |

---

## ✅ 성능과 메모리 관련

- **힙에 동적 할당**, 길이 변화에 따라 재할당 발생.
- **SSO (Short String Optimization)** 적용됨
  → 짧은 문자열(`15자 내외`)은 힙 대신 스택에 저장됨.
- **`std::move` 사용 시 복사 없이 이동 가능**

```cpp
std::string s = "hello";
std::string t = std::move(s); // s는 비워지고, t가 내용 소유

```

---

## ✅ C 스타일 문자열과 차이


| C 스타일 (`char*`)         | `std::string`                         |
| ---------------------------- | --------------------------------------- |
| 널 종료 필요               | 내부적으로 널 문자 유지               |
| 수동 메모리 관리           | RAII 기반 자동 관리                   |
| `strlen`, `strcpy` 등 사용 | 멤버 함수 제공 (`size`, `append`, 등) |

> C API 함수에 넘길 땐 .c_str() 사용 필요

```cpp
printf("%s", s.c_str());

```

---

## ✅ 주의 사항 / 예외 처리

- `at(i)`는 범위 벗어나면 `std::out_of_range` 예외 발생
- `find()` 결과는 **`std::string::npos`** 와 비교 필요

```cpp
if (s.find("abc") == std::string::npos) { ... }

```

---

## ✅ 고급 키워드 및 특성

- **SSO (Short String Optimization)**
  → 짧은 문자열은 힙 사용하지 않고 내부 버퍼에 저장
- **`std::move()`**
  → 복사 비용 줄이기, 이동 생성자/대입자 호출
- **`std::string_view` (C++17~)**
  → 복사 없이 문자열 읽기 전용 처리 가능 (수명 관리 주의)
- **`std::allocator`**
  → 커스텀 메모리 정책 적용 가능 (고급 사용자용)

---

## ✅ 학습 및 활용 팁

- 자주 쓰는 메서드 위주로 미니 실험 코드 작성 권장
- `string_view`, `vector<char>`, `char[]`와의 성능 비교 실습 추천
- 문자열 관련 문제(예: 부분 문자열 검색, 패턴 매칭)로 실전 감각 키우기
- 구현체 내부 분석(libstdc++, libc++)도 실력 향상에 도움

---

## ✅ 참고 링크

- 📘 [cppreference: std::string](https://en.cppreference.com/w/cpp/string/basic_string)
- 📘 [C++ 표준 제안서 (ISO Draft)](https://eel.is/c++draft/string)
- 📘 [C++ Weekly, Fluent C++, ModernesCpp 등 블로그]

---




# 📌 C++ 입출력 핵심 정리

---

### 1. `std::cin`

- C++ 스타일의 대표적인 표준입력
- `iostream` 헤더 필요
- 공백(스페이스, 탭, 줄바꿈)을 기준으로 끊어서 입력 받음

### 2. `std::getline(std::cin, 값)`

- 한 줄 전체를 문자열로 입력받고 싶을 때 사용(띄어쓰기 같은)
- 공백 포함 문자열을 받을 때 유용
- cin 사용후 쓸경우 \n 받을 수 있음. (미리 cin.ignore()로 처리)
- std::cin 말고 다른 걸 넣어 파일, 문자열 버퍼로 변경 가능.

### 3. `scanf()` (c style)

- `cstdio` 헤더 필요
- 포맷 지정자를 사용하여 입력받음
- 빠르지만 타입 안정성이 떨어짐

```
cpp
코드 복사

```

## ✅ `scanf`와 비교해서 `std::cin`이 타입 안정적인 이유


| 항목                 | `scanf` (C 스타일)                     | `std::cin` (C++ 스타일)            |
| ---------------------- | ---------------------------------------- | ------------------------------------ |
| **입력 대상**        | 변수 주소 필요 (`&x`)                  | 변수 자체 전달                     |
| **타입 확인**        | 개발자가`%d`, `%s` 등 직접 지정해야 함 | 컴파일러가 변수 타입을 직접 체크함 |
| **타입 불일치 처리** | **런타임 오류/UB 발생 가능**           | **컴파일러가 타입 체크해줌**       |
| **오류 발생 시**     | undefined behavior 가능                | `fail()` 등으로 명확하게 확인 가능 |

### 출력 스트림으로 한꺼번에 출력하기

ostringstream 객체 이용하기.

```cpp
#include <sstream>
#include <iostream>
#include <string>

std::string s;
std::ostringstream os;  

os << "문자열1";
os << "문자열2";
os << 123;

std::cout << os.str() << std::flush; 
```

### 입력 스트림으로 받은 문자열 delimeter 기준으로 나누기

istringstream 객체 이용하기

```cpp
#include <sstream>
#include <iostream>
#include <string>

std::string s = "apple,banana,orange";
std::istringstream is(s);
std::string token;

// 쉼표(,) 기준으로 분리
while (std::getline(is, token, ',')) {
    std::cout << token << std::endl;
}
// 출력: apple, banana, orange (각각 새 줄에)

```

**우선순위가 떨어져서 확실하게 조사하지 않은 것들**

?std::string 과 char*(c 스타일 스트링의)  차이는 무엇인가

?왜 scanf에서는 &으로 주소를 받는가 (call by ...)

?기본 자료구조 (진행중)

?스트림이란? (곧할 것)

?iterator (곧할것)

적당히 사용법을 익히고 전체 내용을 확실하게 파악할 필요성은 적어 보여서 일단 필요할때 확인하는 쪽으로.
