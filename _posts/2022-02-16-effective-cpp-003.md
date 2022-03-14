---
layout: post

title: Effective C++ 정리 3. 생성자

subtitle: 항목 5 ~ 6

author: hanabzu

categories: [Effective C++]

tags: ECpp Cpp

---

## 1. 빈 클래스

C++ 컴파일러가 빈 클래스를 훑고 지나갈 때, 컴파일러는 복사 생성자(copy constructor), 복사 대입 연산자(copy assignment operator), 소멸자(destructor)를 자동으로 선언한다. 심지어 생성자조차도 선언되어 있지 않으면 기본적으로 생성되는데 모두 `public`이며 인라인 함수이다.  

```cpp
class Empty {};
```

이러한 클래스는 다음과 같다.

```cpp
class Empty{
public:
    Empty() {...}
    Empty(const Emtpy& rhs) {...}
    ~Empty() { ... }

    Empty& operator=(const Empty& rhs) {...}
};
```

이때 소멸자는 이 클래스가 상속한 기본 클래스의 소멸자가 가상 소멸자로 되어 있지 않으면 역시 비가상 소멸자로 만들어진다. (소멸자의 가상성을 기본 클래스로부터 물려받는 경우)  

---

## 2. 복사 생성자와 복사 대입 연산자

```cpp
template<typename T>
class NamedObject {
public:
    NamedObject(const char* name, const T& value);
    NamedObject(const std::string& name, const T& value);
    ...
private:
    std::string nameValue;
    T objectValue;
};
```

이 템플릿 안에는 생성자가 선언되어 있으므로 기본 생성자는 만들어지지 않는다.  
반면, 복사 생성자나 복사 대입 연산자는 선언되어 있지 않기 때문에 두 함수의 기본형이 컴파일러에 의해 만들어진다.  

```cpp
NamedObject<int> no1("Smallest Prime Number", 2);

NamedObject<int> no2(no1); // 복사 생성자 호출
```

컴파일러가 만든 복사 생성자는 `no1.nameValue`와 `no1.objectValue`를 사용해 `no2.nameValue`와 `no2.objectValue`를 각각 초기화한다. `nameValue`의 타입은 `string`인데, 표준 `string` 타입은 자체적으로 복사 생성자를 지니고 있으므로 `string`의 복사 생성자에 `no1.nameValue`를 인자로 넘겨 호출하여 초기화한다. `objectValue`의 타입은 현재 템플릿 인스턴스화에서 `T`가 `int`로 기본 제공 타입이므로 각 비트를 그대로 복사해온다.  

이렇듯 컴파일러가 만드는 `NamedObject<int>`의 복사 대입 연산자도 근본적으로는 동작 원리가 같지만, 최종 결과 코드가 'legal'하고 'reasonable'해야한다.  
다음과 같이 정의되었다고 가정하자.  

```cpp
template<class T>
class NamedObject {
public:
    // nameValue가 비상수 string의 참조자가 되었으므로,
    // 상수 타입의 name을 받지 않는다.
    NamedObject(std::string& Name, const T& value);
    ... // operator=는 정의되지 않았다.
private:
    std::string& nameValue; // 이제 이 멤버는 참조자이다.
    const T objectValue;    // 이제 이 멤버는 상수이다.
};
```

이때 다음과 같은 코드를 실행한다면?

```cpp
std::string newDog("Persephone");
std::string oldDog("Satch");

NamedObject<int> p(newDog, 2);
NamedObject<int> s(oldDog, 36);

p = s; // ???
```

이떄 `p.nameValue`는 `s.nameValue`가 가리키는 `string`을 가리키도록 바뀔 수 없다. C++의 참조자는 원래 자신이 참조하고 있는 것과 다른 객체를 참조할 수 없기 때문이다.  
`p.nameValue`가 참조하는 `string` 객체 자체가 바뀌는 것도 컴파일러가 자동으로 만들어낸 복사 대입 연산자가 하는 일이기엔 과하다. 이 경우 그 `string`에 관련된 모든 객체가 영향을 받기 때문이다.  

그렇기 때문에 이러한 경우 C++은 **컴파일 거부**를 한다. 그러므로 참조자를 데이터 멤버로 갖고 있는 클래스에 대해 대입 연산을 지원하려면 직접 복사 대입 연산자를 정의하여야 한다. 데이터 멤버가 상수 객체일 때에도 C++ 컴파일러가 비슷하게 동작한다.  
추가적으로 복사 대입 연산자를 `private`로 선언한 기본 클래스로부터 파생된 클래스의 경우, 이 클래스는 암시적 복사 대입 연산자를 가질 수 없다. 컴파일러가 거부해버리기 때문이다.

> 컴파일러는 경우에 따라 클래스에 대해 기본 생성자, 복사 생성자, 복사 대입 연산자, 소멸자를 암시적으로 만들어 둔다.

---

## 3. 자동 생성 금지

일반적인 경우에, 어떤 클래스에서 특정한 종류의 기능을 지원하지 않고 싶다면 그런 기능을 제공하는 함수를 선언하지 않으면 된다. 하지만 이 전략은 복사 생성자와 복사 대입 연사자에 대해서는 적용할 수 없다. 컴파일러가 이러한 함수들이 없다면 자동으로 생성해버리기 때문이다.  

이를 해결하는 방법은 `private` 멤버로 복사 생성자와 복사 대입 연산자를 선언해버리는 것이다. 컴파일러가 생성하는 함수는 모두 `private` 멤버로 선언되기 때문에 외부에서 참조가 가능한데, `private` 멤버로 선언해버리면 명시적으로 일단 함수가 선언되었기 때문에 컴파일러는 기본 버전을 만들지 않고, 외부에서 참조도 불가능하다.  

여기엔 한가지 문제점이 있는데, `private` 멤버 함수는 그 클래스의 멤버 함수 혹은 `friend` 함수가 호출할 수 있다. 이를 막기 위해서 함수를 선언만 해두고, 아예 정의하지 않으면 누군가 이 함수를 호출하면 링크 시점에 오류가 나므로 이러한 기법은 자주 사용된다. (C++의 `iostream` 라이브러리의 몇몇 클래스에서도 사용한다.)  

예를 들면 다음과 같다.

```cpp
class HomeForSale {
public:
    ...
private:
    ...
    HomeForSale(const HomeForSale&); // 선언만 존재한다.
    HomeForSale& operater=(const HomeForSale&);
};
```

### 에러 시점 변경

추가로, 위 코드에서 선언만 해둔 함수를 호출할때 일어나는 링크 시점 에러를 컴파일 시점 에러로 옮길 수 있다. 복사 생성자와 복사 대입 연산자를 `private`로 선언하되, 이것을 별도의 기본 클래스에 넣고 이것으로부터 `HomeForSale`을 파생시키는 것이다. 그리고 그 별도의 기본 클래스는 복사 방지만 맡는다는 특별한 의미를 부여한다.  

```cpp
class Uncopyable {
protected:
    Uncopyable() {} // 생성, 소멸 허용
    ~Uncopyable() {}

private:
    Uncopyable(Const Uncopyable&); // 복사 방지
    Uncopyable& operater=(const Uncopyable&);
};

class HomeForSale: private Uncopyable {
    ...
}
```

이렇게 작성하면 컴파일러가 `HomeForSale` 클래스만의 복사 생성자와 복사 대입 연산자를 만들려고 할 때, 기본 클래스의 대응 버전을 호출하게 되는데 그러한 함수들이 `private`이기 때문에 오류가 발생한다.  

### Uncopyable 추가사항

1. `Uncopyable`로부터의 상속은 `public`일 필요가 없다.
2. `Uncopyable`의 소멸자는 가상 소멸자가 아니어도 된다.
3. `Uncopyable` 클래스는 데이터 멤버가 전혀 없기 때문에 공백 기본 클래스 최적화(empty base class optimization) 기법이 사용될 수도 있다. (추후 소개)
4. 이 경우 `Uncopyable` 클래스는 기본 클래스이기 때문에 다중 상속으로 갈 수 있어 이 경우 공백 기본 클래스 최적화가 돌아가지 못할 때가 종종 있다.
5. **부스트** 라이브러리에는 `Uncopyable`과 같은 기능을 하는 `noncopyable` 클래스가 있다.

---

내용 출처 : 스콧 마이어스, 『이펙티브 C++』, 프로텍미디어(2015)
