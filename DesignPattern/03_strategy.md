

# Strategy

## 1. 정의
 Strategy is a behavioral design pattern that lets you define a family of algorithms, put each of them into a separate class, and make their objects interchangeable<br>
 > <details><summary>해석</summary>일련의 알고리즘을 정의하고 각 알고리즘을 별도의 클래스에 넣고 해당 객체를 상호 교환 가능하게 만드는 행동 디자인 패턴이다</details>

<br>

## 2. 실행 과정
<div align="center">
<img width="90%" alt="strategy_pattern_img2" src="https://user-images.githubusercontent.com/48194000/184570213-d1dd9d11-59ab-489e-bfaf-718f728b0802.png"></div>

1. `Context` 클래스
   - `Context` 클래스는 `하나의 전략 클래스를 참조`하고, 해당 전략 인터페이스를 통해서 전략 객체와 통신을 한다<br><br>
   
2. `Strategy`(전략) 클래스
   - 모든 `Concrete strategy` 클래스의 공통인 부분 
   - `Context`가 전략을 실행하는 데 사용하는 방법을 선언한다<br><br>
   
3. `Concrete strategy` 클래스
   - 구체적 전략 클래스는 context가 사용하는 서로 다른 알고리즘을 실행한다<br><br>

4. `Context`는 알고리즘을 실행해야 할 때마다 연결된 `전략 개체`의 실행 메서드를  호출한다<br>컨텍스트는 어떤 유형의 전략으로 작동하는지 또는 알고리즘이 어떻게 실행되는지 모른다<br><br>

5. `Client`는 특정 `전략 개체를 생성`하여 컨텍스트에 전달한다<br>`Context`는 클라이언트가 런타임에 컨텍스트와 관련된 전략 선택이 가능한 `setter`를 적용한다

<br>

## 3. 활용

### 1) 상황
휴가 때 여행자들을 위한 네비게이션 어플을 개발하였다. 개발 시 출발지부터 도착지까지 가장 빠른 운전 경로를 추천해주었다. 테스트부터 배포까지 잘 마무리하였다. 이 과정을 거치면서 개발자들을 일반 보행자를 위한 도보 경로 추천 기능의 요구가 증대한 것을 확인하고 해당 기능을 옵션으로 추가하였다. 하지만 여기서 그치지 않고 사이클을 타는 사람들마저 사이클 경로를 추천해달라는 요구를 하기 시작했고 개발자들은 이를 반영하는 옵션을 추가하기 시작했다.

<br>

### 2) 문제 발견

<p align="center">
<img width="90%" alt="startey_pattern_img1" src="https://user-images.githubusercontent.com/48194000/184569495-65baeee0-a533-4cd5-b489-b4f977755614.png">
</p>

문제는 지금부터였다. 초창기 모델이 단지 자동차 운전경로에 초첨을 맞춰 진행되었기 때문에 옵션을 추가하면서 `하나의 클래스의 규모가 커졌고`, `유지 보수를 하기 어려워졌다`. 또한 단순한 버그 수정이든 거리 점수의 약간의 조정이든 알고리즘 중 하나를 변경하면 전체 클래스에 영향을 미치므로 이미 작동하는 코드에서 `오류`가 발생할 가능성이 `높아졌다`. 어떻게 해야할까?

<br>

### 3) 해결
`전략 패턴`은 다양한 방식으로 `특정 작업을 수행하는 클래스`를 선택하고 이러한 모든 알고리즘을 전략이라는 `별도의 클래스로 추출`할 것을 제안한다.

<div align="center">
<img width="90%" alt="strategy_pattern_img3" src="https://user-images.githubusercontent.com/48194000/184572203-101e43ea-5151-4a60-86d9-6ac550302acb.png">
</div>

- `Context` class - `Navigator`<br>
  : routeStrategy 클래스(전략 클래스) 참조
- `Strategy` class - `routeStrategy`<br>
: buildRoute 메서드를 통해 각 각의 구체적 전략 객체 생성
- `Concrete Strategy` class - Road, PublicTransport, Walking<br>
: `Context`에서 필요한 알고리즘에 적합한 전략을 구체화한 클래스<br>


위와 같이 모든 단계를 추상화 클래스에 선언해 하위 클래스에 제공하여 하위 클래스에서 해당 단계에 대한 메서드를 재정의를 하도록 할 수 있다. 하위 클래스에서 파일을 열고 닫는 코드와 데이터를 추출/파싱하는 코드가 데이터 형식에 따라 다르기 때문에 굳이 건드릴 필요가 없다. 그러나 데이터 분석 및 보고서 작성과 같은 다른 단계의 구현은 `매우 유사`하므로 하위 클래스가 해당 코드를 공유할 수 있는 `기본 클래스`로 끌어올 수 있다.

정리하자면, 두가지 다른 타입의 단계로 구성된다:

- 추상화 단계는 모든 하위 클래스에서 실행되어야 한다
- 선택적 단계들은 필요에 따라 오버라이딩을 통해 하위 클래스에서 재정의하면 된다



<br>

## 4. 코드
```python
from __future__ import annotations
from abc import ABC, abstractmethod
from typing import List


class Context():
    """
    The Context defines the interface of interest to clients.
    """

    def __init__(self, strategy: Strategy) -> None:
        """
        Usually, the Context accepts a strategy through the constructor, 
        but also provides a setter to change it at runtime.
        """

        self._strategy = strategy

    @property
    def strategy(self) -> Strategy:
        """
        The Context maintains a reference to one of the Strategy objects. 
        The Context does not know the concrete class of a strategy. 
        It should work with all strategies via the Strategy interface.
        """

        return self._strategy

    @strategy.setter
    def strategy(self, strategy: Strategy) -> None:
        """
        Usually, the Context allows replacing a Strategy object at runtime.
        """

        self._strategy = strategy

    def do_some_business_logic(self) -> None:
        """
        The Context delegates some work to the Strategy object 
        instead of implementing multiple versions of the algorithm on its own.
        """

        # ...

        print("Context: Sorting data using the strategy (not sure how it'll do it)")
        result = self._strategy.do_algorithm(["a", "b", "c", "d", "e"])
        print(",".join(result))

        # ...


class Strategy(ABC):
    """
    The Strategy interface declares operations common to 
    all supported versions of some algorithm.

    The Context uses this interface to call 
    the algorithm defined by Concrete Strategies.
    """

    @abstractmethod
    def do_algorithm(self, data: List):
        pass


"""
Concrete Strategies implement the algorithm while following the base Strategy interface. 
The interface makes them interchangeable in the Context.
"""


class ConcreteStrategyA(Strategy):
    def do_algorithm(self, data: List) -> List:
        return sorted(data)


class ConcreteStrategyB(Strategy):
    def do_algorithm(self, data: List) -> List:
        return reversed(sorted(data))


if __name__ == "__main__":
    # The client code picks a concrete strategy and passes it to the context.
    # The client should be aware of the differences between strategies in order
    # to make the right choice.

    context = Context(ConcreteStrategyA())
    print("Client: Strategy is set to normal sorting.")
    context.do_some_business_logic()
    print()

    print("Client: Strategy is set to reverse sorting.")
    context.strategy = ConcreteStrategyB()
    context.do_some_business_logic()
```
[출력 결과]
```text
Client: Strategy is set to normal sorting.
Context: Sorting data using the strategy (not sure how it'll do it)
a,b,c,d,e

Client: Strategy is set to reverse sorting.
Context: Sorting data using the strategy (not sure how it'll do it)
e,d,c,b,a
```

[파이썬-데코레이터](https://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0-decorator)

<br>

## 5. 장단점
|장점|단점|
|:------------------------:|:------------------------:|
|런타임에 개체 내부에서 사용되는 <br>알고리즘을 교환할 수 있다|알고리즘이 몇 개만 있고 거의 변경되지 않는다면<br>패턴과 함께 제공되는 새로운 클래스와 인터페이스로 <br>프로그램을 지나치게 복잡하게 만들 이유가 없습니다|
알고리즘을 사용하는 코드에서<br>구현 세부 정보를 분리할 수 있습니다|클라이언트는 적절한 전략을 선택할 수 있도록<br>전략 간의 차이점을 알고 있어야 한다|
|컨텍스트를 변경하지 않고도 <br>새로운 전략을 도입할 수 있습니다|많은 현대 프로그래밍 언어에는 익명 함수 집합 내에서 <br>다양한 버전의 알고리즘을 구현할 수 있는 함수형 지원이 있다<br>이를 통해 전략 객체를 사용했을 때와 똑같이 이 함수를 <br>사용가능하고 추가 클래스와 인터페이스로 코드를 부풀리지 않을 수 있다|

<br>

## 6. Template method 와의 차이
템플릿 메서드 패턴은 기본적으로 상속에 기반한다. 따라서 메인 알고리즘의 부분을 하위 클래스의 부분으로 확장하는 방식이다. 전략 패턴은 기본적으로 Composition(구성)에 기반한다. 객체의 동작 부분을 서로 다른 전략을 제공함으로써 해당 전략에 맞는 동작을 하도록 한다. 템플릿 메서드 패턴은 클래스 레벨에서 동작하기에 정적이라 할 수 있다. 전략 패턴은 객체 레벨에서 동작하므로 런타임에서 원하는 동작을 선택하도록 한다.

<br>

## 7. Quiz
1) 런타임 때 로직을 변경해야 하는 경우 - `_(A)_` 패턴<br>
    > <details><summary>답 : </summary>전략 패턴은 컨텍스트 클래스가 전략(Strategy)을 속성(필드, 인스턴스 변수)으로 가지고 있다보니, setter 메서드를 통해 런타임 도중에 전략(Strategy)을 변경할 수 있다</details>

<br>

2) 템플릿 메서드 패턴은 로직을 변경하려면 클래스로부터 `_(B)_`를 새로 생성해야 한다. 따라서 런타임 도중에 로직을 변경할 수가 없다.
    > <details><summary>답 :</summary>인스턴스</details>
<br>

3) 다양한 컨텍스트 클래스에서 특정 알고리즘을 공유하고 싶을 때 - `_(C)_` 패턴<br> 
   > <details><summary>답 : </summary>전략</details>
<br>

4) 템플릿 메서드 패턴은 로직의 순서가 `_(D)_`되어 있어서, 로직 순서와 일치하는 컨텍스트 클래스에서 밖에 사용을 못한다
   > <details><summary>답 : </summary>고정</details>
<br>

<br>

## 8. 참조
https://refactoring.guru/design-patterns/strategy 


