## 1. 정규화

정규화란? 관계형 데이터베이스에서 “중복을 최소화”하기 위해 데이터를 구조화 하는 작업. 마치 저번주에 배웠던 디자인패턴의 존재와 비슷하다고 보면 됩니다.

## 정규화를 통한 성능 향상 전략
데이터처리의 성능이 무엇인지 정확히 구분하여 인식할 필요가 있다. 데이터베이스에서 데이터를 처리할 때 성능이라고 하면 조회 성능과 입력/수정/삭제 성능의 두 부류로 구분된다. 이 두 가지 성능이 모두 우수하면 좋겠지만 데이터 모델을 구성하는 방식에 따라 두 성능이 Trade-Off 되어 나타나는 경우가 많이 있다.
정규화를 수행한다는 것은 데이터를 결정하는 결정자에 의해 함수적 종속을 가지고 있는 일반속성을 의존자로 하여 입력/수정/삭제 이상을 제거하는 것이다. 데이터의 중복속성을 제거하고 결정자에 의해 동일한 의미의 일반속성이 하나의 테이블로 집약되므로 한 테이블의 데이터 용량이 최소화되는 효과가 있다. 따라서 정규화된 테이블은 데이터를 처리할 때 속도가 빨라질 수도 있고 느려질 수도 있는 특성이 있다.

결론 : 정규화를 진행하면
1.조회성능이 저하될 수 있음(처리조건에 따라 향상될 수도 있다).
2.입력수정삭제성능은 향상됨.

조회성능 vs 입력수정삭제성능, 정규화는 어떻게 보면 양날의 검과 같다.


### 정규화의 장점

1. 중복되는 데이터를 줄이고, 데이터의 일관성을 유지함
2. 삽입/갱신/삭제 시 이상 현상의 발생 가능성을 줄임

#이상현상

→ 갱신 이상, 삽입 이상, 삭제 이상

![image](https://user-images.githubusercontent.com/65318329/185805381-a2f43864-f395-4428-9e72-1b2e95cf75f7.png)


1. 확장성이 있어 새로운 데이터 형식 추가시에도 그 구조를 변경하지 않아도 되거나 일부만 변경

### 단점

1. 연산 시간이 증가(정규화 과정을 거치면 릴레이션을 분해한다. 이로 인해 테이블을 조회할 때, 여러 테이블 간 조인하여 연산하는 경우가 많아진다)

### 결론

<aside>
💡 따라서, 조회 시 조인이 많이 발생하여 성능저하가 나타나는 경우에는 비정규화를 진행한다!

</aside>

## 2. 정규화의 종류

기본 정규형 : 제1정규형, 제2정규형, 제3정규형, BCNF

고급 정규형 : 제4정규형, 제5정규형

### 제 1 정규형

-모든 속성을 반드시 하나의 원자값만 가져야 한다.

![image](https://user-images.githubusercontent.com/65318329/185805395-6d673cec-1974-4ea1-9ac2-2acf54bfbf69.png)


위의 그림의 경우, ‘강의’라는 한 가지 속성에 여러 데이터를 가지고 있다. 따라서 한 가지 속성에는 한 데이터만 존재하도록 릴레이션을 분해해주어야 한다.

### 제 2 정규형

제 1정규형에 속하고, 모든 속성이 **기본키에 완전 함수 종속**되면 제 2정규형에 속한다.

즉, 기본키중에 특정 컬럼에만 종속된 컬럼(부분적 종속이 없어야 한다.)

**완전 함수 종속**

어떤 속성이 기본키에 대해 완전히 종속일 때

**부분 함수 종속**

어떤 속성이 기본키가 아닌 다른 속성에 종속되거나, 기본키가 여러 속성으로 구성되어 있을 경우 기본키를 구성하는 속성 중 일부만 종속될 때
![image](https://user-images.githubusercontent.com/65318329/185805417-0b598929-090a-43fb-8f0d-2b98784b3605.png)


위의 그림의 경우 제 2정규형을 만족하려면, 모든 속성이 기본키에 **완전 함수 종속**되도록 릴레이션을 분해하는 정규화 과정을 거쳐야 한다.

ex) 학생의 강의 점수를 알려면 {학번, 이름}이 아닌, {학번} 또는 {이름}만으로도 알 수 있다. 이는 기본키를 구성하는 속성 중 일부만 종속인 부분 함수 종속 상태이다. 따라서 해당 릴레이션을 분해해주어야 한다.

### 제 3 정규형

제 2정규형에 속하고, 기본키가 아닌 모든 속성이 기본키에 **이행적 함수 종속**이 되지 않으면 제 3정규형에 속한다. 즉, 기본키가 아닌 속성들 간에 종속 관계가 있으면 안된다. 

*이행적 함수 종속 : A→B,B→C인 경우 A→C

![image](https://user-images.githubusercontent.com/65318329/185805425-a5773884-72c4-47ac-b7b5-6e02ea195424.png)


위의 그림의 경우, 기본키가 아닌 ‘강의’가 ‘강의실’을 결정한다. 이는 이행적 함수 종속이므로 해당 릴레이션을 분해해주어야한다.

### BCNF

1. 제 3 정규화에 속하고 **모든 결정자가 후보키**이면 BCNF에 속한다.
2. **기본키가 아닌 속성이 키본키의 속성을 결정지을 수 없다.**

![image](https://user-images.githubusercontent.com/65318329/185805433-a70f7a6a-0a23-4d1d-aa0e-9b7a2663c075.png)


위의 그림의 경우, 강사가 강의를 결정하고 있다. (한 강사가 각자 강의를 담당하고 있다고 가정) 기본키에 속하지 않은 ‘강사’가 기본키에 속한 ‘강의’를 결정짓고 있으므로 해당 릴레이션을 분해해주어야 한다.

<aside>
💡 3NF와 BCNF 차이

1. 3NF 와 BCNF는 일반 칼럼이 주체(결정자)일 경우 이 릴레이션을 분해해주어야 한다.
2. 일반 칼럼이 다른 일반 칼럼에 영향을 준다 :  3NF와 BCNF 모두 위반
3. 일반 칼럼이 기본키에 영향을 준다 : BCNF만 위반

</aside>

[정규화.pptx](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b50bae17-27f5-4432-8d76-63eff02c3994/%EC%A0%95%EA%B7%9C%ED%99%94.pptx)
