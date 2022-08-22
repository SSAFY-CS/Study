## Question
Q1. `TCP` 가 뭔가요?<br>
Q2. `IP` 는 뭐에요?<br>
Q3. `UDT` 가 뭐에요?<br>
Q4. `TCP` 와 `UDT` 의 차이가 뭔가요?<br>


<br>

## 용어 정리 
<details><summary> TCP, IP  </summary>

- 정의<br>
  - `T`ransmission `C`ontrol `P`rotocol(전송 제어 프로토콜)은 `IP suite(인터넷 프로토콜 스위트)`의 핵심 프로토콜 중 하나. `IP`와 함께 `TCP/IP`라는 명칭으로도 널리 불린다<br><br>
  
- 무엇을 전송?
  - TCP는 근거리 통신망이나 인트라넷, 인터넷에 연결된 컴퓨터에서 실행되는 프로그램 간에 일련의 `옥텟`을 안정적으로, 순서대로, 에러없이 교환할 수 있게 한다 <br><br>

- `옥텟`?
  1. 8비트로 구성된 컴퓨팅 및 통신의 디지털 정보 단위
  2. 그럼 1바이트 쓰면 되잖아?
   - 이 용어는 역사적으로 바이트가 다양한 크기의 저장 장치에 사용되었기 때문에 바이트라는 용어가 모호할 때 자주 사용된다<br>
  
- 정리
  - TCP는 디지털 정보 단위를 전송할 때 사용되는 통신 규약<br><br>
- `IP suite` 은 뭐지?
  - `I`nternet `P`rotocol `suite`(인터넷 프로토콜 스위트)<br>
    suite: 모음, 집단, 일행, 짝
  - 인터넷에서 컴퓨터들이 서로 정보를 주고받는 데 쓰이는 통신규약(프로토콜)의 모음 <br><br>

- `IP`는 뭐지?<br>
  - 인터넷 규약(`I`nternet `P`rotocol)은 네트워크 경계 간 `데이터그램`을 관계 짓는 인터넷 프로토콜 스위트에 적용되는 통신 규약 중 `네트워크 계층` 규약이다
</details>

<br>

## TCP/IP ?
- 인터넷 규약들 중 가장 많이 사용되는 `TCP`와 `IP`를 합쳐서 일컫는 프로토콜
- `TCP` : `T`ransmission `C`ontrol `P`rotocol, "***전송***" 제어 규약
- `IP` : `I`nternet `P`rotocol, 인터넷 규약 (***패킷*** 통신 방식)<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;⇢ 호스트 ~ 호스트 간 통신 즉, 보내는 컴퓨터 ~ 받는 컴퓨터 간 통신을 책임진다
- ex) 전화를 걸었다고 가정 (A ⇢ B)

<div align="center">
<img width="90%" src="https://user-images.githubusercontent.com/48194000/185865937-a57603bd-858e-4fc4-aee6-d7a5143442ef.jpg">
</div>

<br>

- 즉, `TCP/IP`를 말한다는 것은 `송신자`가 `수신가`에게 데이터를 전달하고,<br>
  그 데이터가 제대로 전송됐는지, 너무 빠르진 않았는지, 제대로 받았다고 연락은 오는지 등을 확인한다는 것

<br>

- 그렇다면 어떻게 `TCP`를 통해 전송을 제어하고 상태를 체크하게 될까?

<br>

## TCP 동작
- 앞서 언급하였듯 `TCP`는 통신 대상이 되는 ***단말기(endpoint)*** 들이 ***통신할 준비***가 되었는지
  전송된 데이터 양은 얼마이며, 도착한 데이터는 손상되거나 변형된 곳이 없는지 등을 확인

- 해당 과정들은 `TCP 헤더` 정보를 통해서 이루어짐
  <div align="center" id="tcpHeader">
  <img src="https://user-images.githubusercontent.com/48194000/185869540-d4e79289-36d8-4d7e-9bfa-ee1f5b33c881.jpeg">
  </div>

- 출발지 port번호, 도착지 port번호부터 다양한 정보들이 헤더에 들어있음
- 그 중 중간 `9bit`의 통신 제어 플래그인 `control bit`들이 데이터 전송 상태 체크에 사용
  - ex) ACK, SYN, FIN, ...

<br>

- ### `3-way handshaking`
  <div align="left">
  <img src="https://user-images.githubusercontent.com/48194000/185873391-caaed08f-ac37-48d7-9f4a-a5016b15b71c.jpg"  height="400px">
  
  <img src="https://user-images.githubusercontent.com/48194000/185874119-53c77a41-30d5-4afe-b03a-64311f45b333.png" height="340px">

  <img src="https://user-images.githubusercontent.com/48194000/185874886-3f6e5674-95f6-4bb2-a997-2ab1896e85e8.png" height="333px">

  </div>

<br>

## TCP 문제점과 한계
- 위처럼 데이터 전송 상태를 체크하는 것외에도 데이터 훼손 체크도 가능하며
  데이터 손실 시 통신을 기다리게 하는 기능도 있다.<br> 따라서 `신뢰할만한 프로토콜`로 불리지만 
  오래된 만큼 확장성이 부족하다.<br> 게다가 헤더에 기능을 추가하고 싶은 경우,
  [위 tct 헤더 사진](#tcp-동작)에서 확인할 수 있듯이, `option` 필드를 활용해야하는데, <br>
  크기 제약으로 인해 최대 `4구간`만 사용가능하다. <br>
  나아가 너무 잘 알려진 헤더 구조를 가지고 있어 외부에서 관찰하기 쉽고 손대기가 쉬워진 문제가 있다

<br>

## UDP 등장
- 언급된 TCP의 문제점을 해결하기 위해 나타난 것이 바로 `UDP` 이다
- `U`ser `D`atagram `P`rotocol 로, 사용자 데이터 그램 규약으로 TCP와 달리 
  말 그대로 사용자 데이터에 대한 규약이다. <br>
- 전송 제어에는 직접적으로 신경 쓰지 않는다
  
<br>

## UDP 헤더
<div align="left">
  <img src=https://user-images.githubusercontent.com/48194000/185878481-abaf33b1-a152-4c7e-b214-2dd1ea92445f.png height="310px">
</div>

- 이처럼 상당히 간단한 구조를 가지며, 연결 상태 체크를 진행하지 않기 때문에 <br>
- 데이터 송수신 속도가 빠르며 추가기능을 확장할 용량도 충분하다<br>
- 하지만 신뢰성이 부족하며 해당 부분은 헤더 추가 기능을 통해 보완할 수 있다<br>
- DNS, 스트리밍에 적용된다

<br>

## TCP 와 UDP 차이
<div align="left">
<img height="318px" alt="image" src="https://user-images.githubusercontent.com/48194000/185879750-c0c5bba2-2b08-4b37-b51d-b3a8638bd129.png">
</div>

<br>


## TCP와 UDP 설정은 어떻게 하는가?
- TCP와 UDP 둘 다 TCP/IP 계층의 `transport layer`에 속한다
- 어플리케이션 계층에서 패킷이 내려오면 해당 계층에서 `TCP`를 적용할지 `UDP`를 적용할지 정한다


<div align="left">
<img height="375px" src="https://user-images.githubusercontent.com/48194000/185880269-ed5a03c8-6c17-4a87-b34b-04ecb144e074.png">
</div>