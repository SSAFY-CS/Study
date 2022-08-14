
# Template Method

## 1. 정의
 `Defines the skeleton of an algorithm in a method, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithms structure.`<br>by GoF Design Patterns<br>
 > <details><summary>해석</summary>메서드의 알고리즘의 뼈대를 정의하고 몇 단계를 하위 클래스에서 바꾸는 것. 템플릿 메서드는 하위 클래스들이 몇몇의 단계 상의 알고리즘을 알고리즘 구조적 변화 없이 재정의하도록 해준다.</details>
 > <details><summary>GoF ?</summary>&nbsp&nbsp&nbsp&nbsp1. 디자인 패턴 분야의 사인방(Gang of Four, 줄여 GoF)으로 불리는<br>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp에리히 감마(Erich Gamma),리처드 헬름(Richard Helm)<br>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp랄프 존슨(Ralph Johnson), 존 블리시데스(John Vlissides)를 일컬음.<br>&nbsp&nbsp&nbsp&nbsp2. 소프트웨어 개발 영역에서 디자인 패턴을 구체화하고 체계화한 사람들<br>&nbsp&nbsp&nbsp&nbsp3. 23가지의 디자인 패턴을 정리하고 각각의 디자인 패턴을 <br>&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp생성(Creational), 구조(Structural), 행위(Behavioral) 3가지로 분류했다.

<br>

## 2. 실행 과정

1. 대상 알고리즘을 분석하여 단계로 나눌 수 있는지 확인한다
   - 모든 하위 클래스에 공통적인 단계와 항상 고유한 단계를 고려해야 한다<br><br>
2. `추상 기본 클래스`를 만들고 `템플릿 메서드`와 알고리즘의 단계를 나타내는 `추상 메서드 집합`을 선언한다
   - 해당 단계를 실행하여 템플릿 메서드에서 알고리즘의 구조를 설명한다 
   - 서브 클래스가 이를 재정의하지 못하도록 템플릿 메소드를 `final`로 만드는 것을 고려해야한다<br><br>
3. 추가 확장 기능이 필요 시 `추상 기본 클래스`에서 `hook` 용 메서드를 선언하여 활용한다
   - 서브 클래스에서 해당 `hook` 재정의를 통해 필요한 기능을 확장할 수 있다

<br>

## 3. 활용

### 1) 상황
통합 document들을 분석하는 데이터 마이닝 어플리케이션을 만들고 있다고 가정해보자. 사용자들은 앱 document 입력으로 다양한 확장자 파일들(PDF, DOC, CSV)을 넣을 것이고 어플리케이션은 이들로 부터 균일한 형태로 의미있는 데이터를 뽑아내는 역할을 하는 것을 목표로 설계했다.

앱의 첫 번째 버전은 DOC 파일에서만 작동할 수 있었고, 다음 버전에서는 CSV 파일을 지원할 수 있었다. 한 달 후 PDF 파일에서 데이터를 추출하도록 했다. 이러한 과정들은 아래의 그림들에서 확인가능하다.

<br>

<p align="center">
<img width="550" alt="template_method_img1" src="https://user-images.githubusercontent.com/48194000/184536152-88fc6c8e-b933-4c3a-99a1-f3b488e556c0.png" >
</p>


<br>

### 2) 문제 발견
어느 시점에서 세 클래스 `모두 유사한 코드를 많이 가지고 있음`을 알아차렸다. 다양한 데이터 형식을 처리하는 코드는 모든 클래스에서 완전히 다르지만 데이터 처리 및 분석 코드는 거의 동일하다. `알고리즘 구조를 그대로 두고 코드 중복을 제거`하는 것이 좋지 않을까? 추가로 이 클래스를 사용하는 클라이언트 코드와 관련된 또 다른 문제가 있었다. `처리 대상 클래스`에 따라 적절한 조치를 취하는 `조건문이 많이 있다`는 것이었다. 어떻게 해야될까?

<br>

### 3) 해결
세 가지 처리 클래스 모두에 `공통 인터페이스`나 `기본 클래스`가 있는 경우 클라이언트 코드에서 조건을 제거하고 처리 개체에서 `메서드를 호출할 때 다형성`을 사용할 수 있다.

`템플릿 메소드 패턴`은 알고리즘을 `일련의 단계`로 나누고, 이러한 `단계를 메소드로` 전환하고, `단일 템플릿 메소드 내부에` 이러한 메소드에 대한 `일련의 호출`을 넣을 것을 제안한다.

이것이 데이터 마이닝 앱에서 어떻게 작동하는지 보자. 세 가지 구문 분석 알고리즘 모두에 대한 기본 클래스를 만들 수 있다. 이 클래스는 다양한 문서 처리 단계에 대한 일련의 호출로 구성된 템플릿 메서드를 정의할 수 있다. 이를 그림으로 나타내면 아래와 같이 표현할 수 있다.

<br>

<p align="center">
<img width="550" alt="template_method_img2" src="https://user-images.githubusercontent.com/48194000/184537702-e857ca58-bbca-445e-93be-ee548a0cbf82.png"></p>

<br>

위와 같이 모든 단계를 추상화 클래스에 선언해 하위 클래스에 제공하여 하위 클래스에서 해당 단계에 대한 메서드를 재정의를 하도록 할 수 있다. 하위 클래스에서 파일을 열고 닫는 코드와 데이터를 추출/파싱하는 코드가 데이터 형식에 따라 다르기 때문에 굳이 건드릴 필요가 없다. 그러나 데이터 분석 및 보고서 작성과 같은 다른 단계의 구현은 `매우 유사`하므로 하위 클래스가 해당 코드를 공유할 수 있는 `기본 클래스`로 끌어올 수 있다.

정리하자면, 두가지 다른 타입의 단계로 구성된다:

- 추상화 단계는 모든 하위 클래스에서 실행되어야 한다
- 선택적 단계들은 필요에 따라 오버라이딩을 통해 하위 클래스에서 재정의하면 된다



<br>

## 4. 코드
```python
from abc import ABC, abstractmethod


class DataMiner(ABC):
    """
    The Abstract Class defines a template method that contains a skeleton of
    some algorithm, composed of calls to (usually) abstract primitive
    operations.

    Concrete subclasses should implement these operations, but leave the
    template method itself intact.
    """

    def mine(self) -> None:
        """
        The template method defines the skeleton of an algorithm.
        """

        self.openfile(path='파일 경로')
        self.extractData(path='파일 경로')
        self.parseData(path='파일 경로')
        self.hook1()
        self.hook2()
        self.analyze_data()
        self.send_report()

    # These operations already have implementations.

    def openfile(self, path) -> None:
        print(f"{path}의 데이터를 불러옵니다")

    def analyze_data(self) -> None:
        print("데이터 분석 시작합니다")

    def send_report(self) -> None:
        print("분석 결과 보고 시작합니다")

    # These operations have to be implemented in subclasses.

    @abstractmethod
    def extractData(self, path) -> None:
        pass

    @abstractmethod
    def parseData(self, path) -> None:
        pass

    # These are "hooks." Subclasses may override them, but it's not mandatory
    # since the hooks already have default (but empty) implementation. Hooks
    # provide additional extension points in some crucial places of the
    # algorithm.

    def hook1(self) -> None:
        pass

    def hook2(self) -> None:
        pass


class DocDataMiner(DataMiner):
    """
    Concrete classes have to implement all abstract operations of the base
    class. They can also override some operations with a default implementation.
    """

    def extractData(self, path) -> None:
        print(f"{path}에 Doc이 있네요, Doc 데이터 추출 진행할게요 ")

    def parseData(self, path) -> None:
        print(f"{path}에 Doc이 있네요, Doc 데이터 파싱 진행할게요 ")


class CSVDataMiner(DataMiner):
    """
    Usually, concrete classes override only a fraction of base class'
    operations.
    """

    def extractData(self, path) -> None:
        print(f"{path}에 CSV가 있네요, csv 데이터 추출 진행할게요")

    def parseData(self, path) -> None:
        print(f"{path}에 CSV가 있네요, csv 데이터 파싱 진행할게요")

    def hook1(self) -> None:
        print("CSV전용 추가 기능을 추가했습니다")

class PDFDataMiner(DataMiner):
    """
    Usually, concrete classes override only a fraction of base class'
    operations.
    """

    def extractData(self, path) -> None:
        print(f"{path}에 PDF가 있네요, pdf 데이터 추출 진행할게요")

    def parseData(self, path) -> None:
        print(f"{path}에 PDF가 있네요, pdf 데이터 파싱 진행할게요")

    def hook1(self) -> None:
        print("PDF전용 추가 기능을 추가했습니다")


def client_code(abstract_class: DataMiner) -> None:
    """
    The client code calls the template method to execute the algorithm. Client
    code does not have to know the concrete class of an object it works with, as
    long as it works with objects through the interface of their base class.
    """

    # ...
    abstract_class.mine()
    # ...


if __name__ == "__main__":
    print("Same client code can work with different subclasses:")
    client_code(DocDataMiner())
    print("")

    print("Same client code can work with different subclasses:")
    client_code(CSVDataMiner())
    print("")

    print("Same client code can work with different subclasses:")
    client_code(PDFDataMiner())
```
[출력 결과]
```text
Same client code can work with different subclasses:
파일 경로의 데이터를 불러옵니다
파일 경로에 Doc이 있네요, Doc 데이터 추출 진행할게요 
파일 경로에 Doc이 있네요, Doc 데이터 파싱 진행할게요 
데이터 분석 시작합니다
분석 결과 보고 시작합니다

Same client code can work with different subclasses:
파일 경로의 데이터를 불러옵니다
파일 경로에 CSV가 있네요, csv 데이터 추출 진행할게요
파일 경로에 CSV가 있네요, csv 데이터 파싱 진행할게요
CSV전용 추가 기능을 추가했습니다
데이터 분석 시작합니다
분석 결과 보고 시작합니다

Same client code can work with different subclasses:
파일 경로의 데이터를 불러옵니다
파일 경로에 PDF가 있네요, pdf 데이터 추출 진행할게요
파일 경로에 PDF가 있네요, pdf 데이터 파싱 진행할게요
PDF전용 추가 기능을 추가했습니다
데이터 분석 시작합니다
분석 결과 보고 시작합니다
```

[파이썬-데코레이터](https://schoolofweb.net/blog/posts/%ED%8C%8C%EC%9D%B4%EC%8D%AC-%EB%8D%B0%EC%BD%94%EB%A0%88%EC%9D%B4%ED%84%B0-decorator)

<br>

## 5. 장단점
|장점|단점|
|:----------------------:|:------------------------:|
|클라이언트가 대규모 알고리즘의 특정 부분만 재정의하여<br> 알고리즘의 다른 부분에 발생하는 변경 사항의 영향을 덜 받도록 할 수 있다|일부 클라이언트는 알고리즘의 제공된 골격에 의해 제한될 수 있습니다|
중복 코드를 수퍼 클래스로 가져올 수 있다|템플릿 메서드는 더 많은 단계를 유지하기가 더 어려운 경향이 있다|
|-|하위 클래스의 기본 단계 구현을 억제해 [Liskov 대체 원칙](https://ko.wikipedia.org/wiki/%EB%A6%AC%EC%8A%A4%EC%BD%94%ED%94%84_%EC%B9%98%ED%99%98_%EC%9B%90%EC%B9%99)을 위반할 수 있다|