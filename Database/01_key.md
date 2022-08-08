
## 1. Key 의 개념

### - 정의
  - 데이터베이스에서 **키(Key)** 는 조건에 만족하는 튜플을 찾거나, 순서대로 정렬할 때 다른 튜플들과 구별할 수 있는 기준이 되는 속성이다.<br>
### - 필요성
  - 중복 현상 없이, 여러 이상(삽입, 수정, 삭제) 현상을 방지하기 위함.
  - 특정 데이터를 검색 혹은 정렬을 하고 싶을 때, 해당 **키**를 통해 접근할 수 있다.
### - 특성
  - <u>**유일성**</u>: 키-튜플 이 1:1 매칭이 되는가?
  - <u>**최소성**</u>: 키를 구성하는 속성들 중 가장 최소로 필요한 속성들로만 구성되었는가?
  - Quiz 1) Students relation(=table)에서 유일성이 있는 키는?
        <details><summary>Answer</summary>: 학번, 인스타 계정<br></details>
  - Quiz 2) keyA = (학번, 인스타계정) 일때, keyA는 최소성이 있는가? 
        <details><summary>Answer</summary>: 없다. 학번 or 인스타 계정 하나만으로 relation의 튜플들을 구별할 수 있다.<br></details>
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⎪ Students ⎪
    | 학번 | 나이 | 이름 | 수강 과목 |인스타 계정|
    |:---:|:---:|:---:|:---:|:---:|
    |20221234| 20 | 제니 | 알고리즘 | @blackpink |
    |20221235| 20 |뷔| 웹 | @bts |
    |20221236| 24 |한소희| 임베디드 | @beuty |
    |20221237| 26 |손석구| 임베디드 | @gentle |
    
  
<br>

## 2. Key 의 종류
    1. Super Key     (수퍼키) ⎤
    2. Candidate Key (후보키) ⎥--> 집합 관계
    3. Prime Key     (기본키) ⎥
    4. Alternate Key (대체키) ⎦ 
    5. Composite Key (복합키) 
    6. Foreign Key   (외래키) 
    7. Surrogate Key (대리키)

### 1. Super Key
- ***<u>유일성</u>*** 을 만족하는 키
- ***<u>최소성은 만족하지 않아도 된다</u>***
- Quiz 3) 위의 relation에서 (학번, 나이, 이름)은 Super key 인가?
    <details><summary>Answer</summary>: 그렇다. 학번이 포함되어 유일성을 지닌다.</details>

### 2. Candidate Key
- ***<u>유일성</u>*** 과 ***<u>최소성</u>*** 을 만족하는 키
- <u>기본키가 될 수 있는 후보</u>이기 때문에 후보키라고 불린다
- Quiz 4) Students Relation에서 후보키는?
  <details><summary>Answer</summary>: 학번, 인스타 계정</details>

### 3. Prime Key
- ***<u>후보키에서 선택된 키</u>***
- 기본키로 NULL 값 사용 불가
- 기본키에는 동일한 값이 들어갈 수가 없다
- <details><summary>MySQL에서 Students relation에서 인스타 계정을 기본키로 설정하기</summary><br>
    
    1. Database 생성
    ```sql
    CREATE DATABASE study_db
    ```
    2. Table 생성
    ```sql
    CREATE TABLE student( 
        student_id CHAR(9), 
        age INT DEFAULT 20, 
        name VARCHAR(48) NOT NULL, 
        subject VARCHAR(80), 
        insta VARCHAR(120) 
    );
    ```
    3. PRIMARY KEY 설정
    ```sql
    ALTER TABLE student 
    ADD PRIMARY KEY(insta);
    ```
    4. 결과(전 -> 후)<br>
    <img width="60%" src="https://user-images.githubusercontent.com/48194000/183256362-53c4b0c7-f7a3-4795-99d5-8e47dea2eb4c.jpeg"/>

  </details>

### 4. Alternate Key
- ***<u>후보키에서 선택되지 못한 키</u>***
- Quiz 5) 위의 기본키가 세팅된 상태에서 대체키는?
  <details><summary>Answer</summary>: 인스타 계정</details>

### 5. Composite Key
- 기본키의 속성이 하나만 가지고는 유일성을 만족하지 않는 경우 적용
- 두 개 이상의 coulumn을 기본키로 사용<br>
  ex) 위의 수강신청 relation이 1학기에 이어서 2학기 정보가 쌓이는 경우<br>
      &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;동일한 학생이 생기게 된다 -> (학기 + 학번)으로 기본키 구성

### 6. Foreign Key
- 다른 relation 의 기본키를 그대로 참조하는 속성 또는 속성들의 집합이 외래키이다 
- 외래키는 relation 간의 관계를 올바르게 표현하기 위해 필요하다
- 외래키는 참조되는 relation 의 기본키와 동일한 키 속성을 가진다
- 
    <details><summary>부모 relation</summary>

    | 학번 | 나이 | 이름 | 수강 과목 |인스타 계정|
    |:---:|:---:|:---:|:---:|:---:|
    |20221234| 20 | 제니 | 알고리즘 | @blackpink |
    |20221235| 20 |뷔| 웹 | @bts |
    |20221236| 24 |한소희| 임베디드 | @beuty |
    |20221237| 26 |손석구| 임베디드 | @gentle |
    </details>
- 
    <details><summary>자식 relation</summary>

    | 수강과목 | 점수 | 학번 |
    |:---:|:---:|:---:|
    | 알고리즘 | A+ |20221234|
    | 웹 | A0 |20221235|
    | 임베디드 | A+ |20221236|
    | 임베디드 | A+ |20221237|
    </details>

- Quiz 6) 부모 relation 의 학번은 __키, 자식 relation의 학번은 __키가 된다
  <details><summary>Answer</summary>:기본, 외래</details>

### 7. Surrogate Key
- 키가 너무 길거나 여러개의 속성으로 구성된 경우, 인위적으로 추가하는 키이며, 인공키라고도 불린다
- 한 relation과 다른 relation이 서로 관계를 갖고 있고, 외래키로 접근하고 데이터를 수정하는 경우<br>
  키가 길거나 여러개 속성인 복합키인 경우 수행 속도가 떨어질 수 있다
- 위와 같은 상황에서 인공키를 통해 데이터는 주고 받는 속도가 향상될 수 있다
