# Adapter Pattern

<br>

### 어댑터 패턴이란?
> 클래스의 인터페이스를 사용자가 기대하는 다른 인터페이스로 변환하는 패턴
- 콘센트 전원에서 나오는 전기는 교류 200V, 노트북 충전기는 직류 120V
- 교류 200V를 직류 120V로 바꾸어 주는 어댑터

<img src="https://user-images.githubusercontent.com/48194000/185931019-e1649ab5-103f-4159-be33-e32821970eac.jpg" height=55% width=55%></img>
<img src="https://user-images.githubusercontent.com/48194000/185931013-18ae2d23-1801-4585-8a6c-c5dbd67e286c.png" height=55% width=55%></img>

#### 1. Target Interface
- Adapter가 implements하는 인터페이스
- 클라이언트는 Target Interface를 통해 Adaptee인 써드파티 라이브러리를 사용

```
public interface Duck{
    public void quack();
    public void fly();
}
```

```
public class MallardDuck implements Duck {

    public void quack() {
         System.out.println("꽥! 꽥!");
    }

    public void fly() {
        System.out.println("날라갈 수 있습니다!");
    }
 }
```

#### 2. Adaptee
- 써드파티 라이브러리나 외부시스템

```
public interface Turkey { 
    public void gobble(); 
    public void fly(); 
}
```

```
public class WildTurkey implements Turkey{ 

    public void gobble() { 
        System.out.println("고르륵! 고르륵!"); 
    } 

    public void fly() { 
        System.out.println("짧은거리만 날라갈 수 있습니다."); 
    } 
}
```

#### 3. Adapter
- Client와 Adaptee 중간에서 호환성이 없는 둘을 연결시켜주는 역할
- Target Interface를 통해 어댑터에 요청을 보낸다.
- Adapter는 클라이언트의 요청을 Adaptee가 이해할 수 있는 방법으로 전달하고 처리는 Adaptee에서 이루어짐
```
public class TurkeyAdapter implements Duck { 

    Turkey turkey; 

    public TurkeyAdapter(Turkey turkey) { 
        this.turkey = turkey; 
    } 

    public void quack(){
        turkey.gobble(); 
    } 

    public void fly() { 
        turkey.fly(); 
    } 
}
```

#### 4. Client
- 써드파티 라이브러리나 외부시스템을 사용하려는 곳
```
public class App {

    public static void main(String[] args) {
        System.out.println("칠면조가 웁니다.");
        WildTurkey turkey = new WildTurkey();
        turkey.gobble();
        turkey.fly();

        System.out.println("칠면조 어댑터가 웁니다.");
        Duck turkeyAdapter = new TurkeyAdapter(turkey);
        turkeyAdapter.quack();
        turkeyAdapter.fly();

        System.out.println("오리가 웁니다.");
        MallardDuck duck = new MallardDuck();
        duck.quack();
        duck.fly();
    }
}
```

#### 실행결과
```
칠면조가 웁니다.
고르륵! 고르륵!
짧은거리만 날라갈 수 있습니다.
칠면조 어댑터가 웁니다.
고르륵! 고르륵!
짧은거리만 날라갈 수 있습니다.
오리가 웁니다.
꽥! 꽥!
날라갈 수 있습니다!
```