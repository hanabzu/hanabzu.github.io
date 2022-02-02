---
layout: post

title: Effective C++ 정리 1. - const, enum, inline 등

subtitle: 항목 1 ~ 3

author: hanabzu

categories: Effective C++

tags: ECpp Cpp

---

## 0. Effective C++

C++의 바이블이라고 불리는 Effective C++을 읽으며 간단하게 정리하고자 한다.  
주로 사용하는 언어가 C++임에도 불구하고 아직 C++에 대해 많은 것을 모르고 있던 것 같다...  
앞으로 코딩을 할 때 이 책의 내용을 곱씹어가며 좋은 습관을 만들어가는 것이 목표.  

---

## 1. C++은 여러 언어의 연합체 (federation)

C++은 현재 **"다중 패러다임 프로그래밍 언어(multiparadigm programming language)"** 로 여겨진다.

* **C**
  
  C의 절차적 (procedural) 프로그래밍을 기본으로 한다.

* **객체 지향**
  
  클래스, 캡슐화, 상속, 다형성, 가상 함수 등 객체 지향 설계의 개념을 사용한다.

* **템플릿 C++**
  
  템플릿이 현재 C++에 미치는 영향은 매우 크기 때문에, 템플릿 메타 프로그래밍 (template metaprogramming: TMP)이라는 새로운 프로그래밍 패러다임이 탄생했다.

* **STL**
  
  매우 특별한 템플릿 라이브러리로 컨테이너, 반복자, 알고리즘, 함수 객체가 뒤섞여 돌아가고 있으며 STL의 독특한 사용규약을 따라 새롭게 프로그래밍 할 수 있다.  

> 결론적으로 C++은 프로그램의 설계에 따라 하위 언어, 즉 C++의 어떤 부분을 활용해서 프로그래밍할 지 달라지게 된다.

---

## 2. #define 대신 const, enum, inline으로

`#define`의 경우는 선행 처리자에서 처리되기 때문에 컴파일러는 그 내용을 알 수 없다.

```cpp
#define ASPECT_RATIO 1.653
```

컴파일러는 `ASPECT_RATIO`라는 이름을 알 수 없고 `1.653`만 보게 된다.  
즉 컴파일 에러가 발생했을 시 이유를 찾기가 힘들어진다.
그렇기에 매크로 대신 상수를 사용한다.  

```cpp
const double AspectRatio = 1.653;
```

또한 이렇게 했을 경우 전처리 후 상수 타입은 코드에 단 한 번만 생기기 때문에 코드의 크기도 작아진다.  

### 주의할 점

상수 포인터를 정의할 경우 포인터는 당연히 `const`로 선언하고, 포인터가 가리키는 대상도 `const`로 정의하는 것이 보통이다. 예를 들면 다음과 같이 작성해야 한다.  

```cpp
const char * const authorName = "Hanabzu";
```

물론 `string`을 사용하는 것이 좋다.

```cpp
const std::string authorName("Hanabzu");
```

### 클래스 내부 상수

또한 클래스 멤버를 상수로 정의하는 경우 유효범위를 클래스로 한정하려면 `static`멤버로 만들어야 한다.

```cpp
class GamePlayer {
private:
  static const int NumTurns = 5;
  int scores[NumTurns];
  ...
};
```

여기서 `Numturns`는 정의된 것이 아닌 '선언(declaration)'된 것이다.  
C++에서 보통의 경우 정의가 필요하지만, 정적 멤버로 만들어지는 정수류의 클래스 내부 상수는 예외로 주소값을 필요로 하지 않는 경우 선언만 해도 상관없다. 대신 일부 컴파일러가 요구하는 경우 제공해야 한다.

```cpp
const int GamePlayer::NumTurns;
```

이 때 정의는 헤더 파일이 아닌 구현 파일에 둔다. 그 이유는 클래스 상수의 초기값은 선언된 시점에서 바로 주어지는 데, 헤더 파일에 작성하는 정의에는 상수의 초기값이 있으면 안되기 때문이다.  
`#define`은 당연하게도 클래스 상수로 사용하면 안 된다. `#define`은 컴파일 이전에만 유효하기 때문.  
조금 오래된 컴파일러의 경우 정적 클래스 멤버가 선언된 시점에 초기값을 주는 것이 대개 맞지 않다고 판단하기 때문에 다음과 같이 초기값을 상수 '정의' 시점에 준다.

```cpp
class CostEstimate {
  private:
  static const double FudgeFactor; // 헤더 파일
  ...
};

const double CostEstimate::FudgeFactor = 1.35; // 구현 파일
```

### 나열자 둔갑술

한가지 예외로 클래스 컴파일 도중 클래스 상수의 값이 필요한 경우인데, (이는 구 컴파일러에 대해서 필요한 적용이지만) **나열자 둔갑술(enum hack)** 을 사용할 수 있다.

```cpp
class GamePlayer {
private:
  enum { NumTurns = 5 };
  
  int scores[NumTurns];
}
```

이 나열자 둔갑술은 `const`보다 `#define`에 가깝지만 주소를 취할 수 없기 때문에 캡슐화 또한 보장된다. 템플릿 메타 프로그래밍의 핵심 기법이기도 하다.  

### #define의 오용

다음과 같이 `a`와 `b`중 큰 것을 `f`에 넘겨 호출하는 매크로 함수가 있다.

```cpp
#define CALL_WITH_MAX(a, b) f((a) > (b)) ? (a) : (b))
```

이런 매크로를 작성할 때에는 기본적으로 인자마다 괄호를 씌워줘야 한다. 하지만 또 다른 문제가 발생할 수 있다.

```cpp
int a = 5, b = 0;
CALL_WITH_MAX(++a, b)       // a가 두 번 증가한다.
CALL_WITH_MAX(++a, b+10)    // a가 한 번 증가한다.
```

비교 결과에 따라 다르게 작동한다.  

### 인라인 함수 템플릿

```cpp
template<typename T>
inline vid callWithMax(const T& a, const T& b) {
  f(a > b? a : b);
}
```

이 함수는 템플릿이기 때문에 동일 계열 함수군을 만들고, 인자를 여러번 평가할 걱정도 없으며 진짜 함수로 취급되기 때문에 유효범위 및 접근 규칙도 적용된다.  

* 단순한 상수를 쓸 때에는 `#define`보다 `const`객체 혹은 `enum`을 우선 생각하자.  
* 함수처럼 쓰이는 매크로를 만들 때에는 `#define` 매크로보다 인라인 함수를 먼저 생각하자.

---

## 3. const를 최대한 활용하기

포인터 자체를 상수로, 포인터가 가리키는 데이터를 상수로 지정할 수 있고, 둘 다 상수로, 혹은 아무것도 지정하지 않을 수도 있다.

```cpp
char greeting[] = "Hello";

char *p = greeting;               // 비상수 포인터
                                  // 비상수 데이터

const char *p = greeting;         // 비상수 포인터
                                  // 상수 데이터

char * const p = greeting;        // 상수 포인터
                                  // 비상수 데이터

const char * const p = greeting;  // 상수 포인터
                                  // 상수 데이터
```

`const`가 `*`의 **왼쪽**이면 **포인터가 가리키는 대상**이 상수이다.  
`const`가 `*`의 **오른쪽**이면 **포인터 자체**가 상수이다.  

---

```cpp
void f1(const Widget *pw);
void f2(Widget const *pw);
```

포인터가 가리키는 대상을 상수로 만들 때 `const`는 타입 앞과 타입 뒤(`*` 앞)에 붙일 수 있다.  

### STL iterator

기본적인 동작 원리는 `T*` 포인터와 같고, `const`로 선언하는 것 역시 `T* const`포인터와 같다.  
변경이 불가능한 객체를 가리키는 반복자가 필요하다면 `const_iterator`를 사용한다.

### 함수 선언에서의 const

`const`는 함수 반환 값, 각각의 매개변수, 멤버 함수 앞에 붙을 수 있고, 함수 전체에 대해 `const`의 성질을 붙일 수 있다.  

```cpp
class Rational { ... };

const Rational operator* (const Rational& lhs, const Rational& rhs);
```

이렇게 함수 반환 값을 `const`로 지정하는 경우,

```cpp
Rational a, b, c;

(a * b) = c;

if (a * b = c) ...
```

이러한 실수를 미연에 방지할 수 있다. (기본제공 타입의 경우는 당연히 문법 위반)  
`const` 매개 변수의 경우에도 변수 혹은 지역 객체를 수정하는 것이 목적이라면 **반드시** 사용하도록 한다.  

### 상수 멤버 함수

멤버 함수에 붙는 `const`의 역할은 해당 멤버 함수가 상수 객체에 대해 호출될 함수임을 알려주는 역할을 한다.  
클래스의 인터페이스를 이해하기 좋게 만들고, 이 키워드를 통해 상수 객체를 사용할 수 있도록 한다.  
C++ 프로그램의 실행 성능을 높이는 핵심 기법 중 하나가 객체 전달을 **'상수 객체 대한 참조자(reference-to-`const`)** 로 진행하는 것이다. 이를 위해서 상수 멤버 함수가 준비되어 있어야 한다.  
`const` 키워드가 있고 없고의 차이만 나는 멤버 함수들은 오버로딩이 가능하다.  

### 비트수준 상수성

물리적 상수성 (physical constness) 라고도 하며 어떤 멤버 함수가 정적 멤버를 제외한 그 객체의 어떤 데이터 멤버도 건드리지 않아야 그 멤버 함수가 '`const`'임을 인정하는 개념이다. C++에서 정의하고 있는 상수성이 비트수준 상수성이다.

```cpp
class CTextBlock {
public:
  ...
  char& operator[] (std::size_t position) const // Wrong!!!
  {
    return pText[position];
  }

private:
  char *pText;
};

const CTextBlock cctb("Hello");

char *pc = &cctb[0];

*pc = 'J'; // 상수성이 지켜지지 않음
```

하지만 이렇게 `const` 멤버 함수임에도 객체 수정이 가능해진다.  

### 논리적 상수성

위와 같은 상황을 보완하기 위해서 나온 개념으로 상수 멤버 함수임에도 일부 몇 비트는 바꿀 수 있되, 그것을 사용자측에서 모르도록 하면 상수 멤버 자격이 있다는 개념이다.  
`mutable` 키워드를 사용하여 비트수준 상수성의 족쇄를 풀고 상수 멤버 함수 안에서도 수정이 가능하도록 할 수 있다.

```cpp
class CTextBlock {
public:
  ...
  std::size_t length() const;

private:
  char *pText;
  mutable std::size_t textlength;
  mutable bool lengthIsValid;         // 언제든 수정 가능
};

std::size_t CTextBlock::length() const {
  if (!lengthIsValid){
    textlength = std::strlen(pText); 
    lengthIsValid = true;             // No problem
  }
  return textlength;
}
```

### 코드 중복 피하기

상수 멤버 및 비상수 멤버 함수에서 여러 작업을 진행하면 오버로딩을 할 시 코드의 중복이 나타나게 되는데 이를 줄이기 위해서 **캐스팅**을 사용한다.

```cpp
class TextBlock {
public:
  ...
  // 상수 멤버 함수
  const char& operator[] (std::size_t position) const {
    ...
    ...
    return text[position];
  }

  // 비상수 멤버 함수
  char& operator[] (std::size_t position) {
    return 
      const_cast<char&>(
        static_cast<const TextBlock&>(*this)[position]
      ); // casting twice
  }
  ...
};
```

상수 멤버 함수를 부르기 위해 `*this`에 `const`를 붙이는 캐스팅과 결과인 `operator[]`의 반환값에서 `const`를 떼는 캐스팅을 한다.  
위 캐스팅을 반대로 하고 상수 버전 함수가 비상수 버전 함수를 호출하도록 설계하는 것은 좋지 않다. 비상수 함수에서는 객체를 수정하도록 설계되어 있을 수 있기 때문이다.

---

아... 어렵다.  

내용 출처 : 스콧 마이어스, 『이펙티브 C++』, 프로텍미디어(2015)
