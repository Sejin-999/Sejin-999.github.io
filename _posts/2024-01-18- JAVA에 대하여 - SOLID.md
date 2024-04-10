---
title: JAVA - SOLID에 대하여
date: 2024-01-18 06:44:00 +09:00
categories:
  - java
tags:
  - [java, OOP, SOLID]
---
## 들어가며

이번 포스트에서는 SOLID 에 대하여 정리합니다.

## SOLID?

SOLID는 객체 지향 설계에서 중요한 5가지 원칙을 의미합니다

각 글자는 이름의 첫글자를 따와서 지은 이름입니다.

이것을 잘 지키면 결합도는 낮추고 응집도는 올릴 수 있습니다.

결국 모듈, 클래스간의 상호 의존성은 낮춰 재사용성 , 수정 , 유지보수의 용의해집니다.

또한, 구성요소들의 기능적 관련성이 높아짐으로 책임이 집중되고 , 독립성이 높아져

재사용성 , 수정 , 유지보수가 용의해집니다.

## **SRP - Single Responsibility Principle  - 단일 책임 원칙**

클래스는 하나의 변경 이유만 가져야 합니다.

클래스는 단일 기능에 집중해야 하며 여러 가지 책임을 갖지 않도록 해야 합니다.

다양한 경우가 있지만 

사람과 주식을 예로 들어보겠습니다.

```java
class 사람 {
	final static Boolean 한국인 = true;
	final static Boolean 미국인 = false;
	Boolean 인종;
}

public void 주식(){
	if(this.인종 == 한국인){
			System.out.print("쌀");
	}else{
			System.out.print("빵");
	}
}
```

이런 경우 주식() 이라는 메서드가 인종에 대한 모든 행위를 구현하려고 합니다.

이런 경우가 SRP - 단일 책임 원칙을 위배한 경우를 말합니다.

만약 옥수수를 주식으로 먹는 멕시코인을 추가하려면 어떻게 될까요?

```java
public class 사람 {
    final static 인종 한국인 = 인종.한국인;
    final static 인종 미국인 = 인종.미국인;
    final static 인종 멕시코인 = 인종.멕시코인;

    enum 인종 {
        한국인,
        미국인,
        멕시코인
    }

    private 인종 인종;

    public 사람(인종 인종) {
        this.인종 = 인종;
    }

    public void 주식() {
        switch (인종) {
            case 한국인:
                System.out.print("쌀");
                break;
            case 미국인:
                System.out.print("빵");
                break;
            case 멕시코인:
                System.out.print("옥수수");
                break;
        }
    }
}
```

예를 들어 이런식으로 구현할 수 있을 것입니다.

생각보다 정말 많은 부분을 수정해야 합니다. 이런 경우가 결합도가 높아 생기는 문제로

SRP는 이런 결합도를 줄이기 위해 사용됩니다.

단일 책임 원칙을 잘 지키는 방법 중 추상화를 활용하는 방법이 대표적인데요

추상화를 통해 결합도를 줄일 수 있기 때문입니다.

다음 코드처럼 만들었다면 어떨까요?

```java
public abstract class 사람 {
    // 추상 메서드 주식() 선언
    public abstract void 주식();
}

Class 한국인 extends 사람{
		@Override
		public void 주식(){
				System.out.print("쌀");
		}
}

Class 미국인 extends 사람{
		@Override
		public void 주식(){
				System.out.print("빵");
		}
}
```

만약 여기에 옥수수를 먹는 멕시코인을 추가한다면 

```java
public abstract class 사람 {
    // 추상 메서드 주식() 선언
    public abstract void 주식();
}

Class 한국인 extends 사람{
		@Override
		public void 주식(){
				System.out.print("쌀");
		}
}

Class 미국인 extends 사람{
		@Override
		public void 주식(){
				System.out.print("빵");
		}
}

Class 멕시코인 extends 사람{
		@Override
		public void 주식(){
				System.out.print("옥수수");
		}
}
```

재사용성도 높아지고 , 코드의 변경도 적은걸 볼 수 있습니다.

이런식으로 코드의 유지보수 , 수정 등의 이유를 들어 SRP 를 잘 지켜야 된다고 말할 수 있습니다.

## **OCP - Open/Closed Principle- 개방-폐쇄 원칙**

소프트웨어 엔터티(클래스, 모듈, 함수)는 확장에는 개방되어 있지만, 수정에는 닫혀 있어야 한다

라는 의미인데 사실 이해가 어렵습니다.

extends 나 implements 를 통해 확장은 가능하지만

주의의 변경이 일어난 경우에는 수정사항이 발생하면 안된다.

라는 의미로 받아들일 수 있는데 결국

기존 코드는 변화 No **BUT** 새로운 기능 추가 Yes 

이렇게 정리해 볼 수 있습니다.

코드를 통해 예시를 들어보며 이해해 보겠습니다.

```java
public class 사람 {
    private final String 국적;

    public 사람(String 국적) {
        this.국적 = 국적;
    }

    public void 주식() {
        if (국적.equals("한국")) {
            System.out.print("쌀");
        } else if (국적.equals("미국")) {
            System.out.print("빵");
        } else {
            throw new IllegalArgumentException("지원하지 않는 국적입니다: " + 국적);
        }
    }
}
```

예를 들어 이런 코드를 하나 작성했다고 가정하겠습니다.

그럼 옥수수를 먹는 멕시코인을 추가해보겠습니다.

```java
public class 사람 {
    private final String 국적;

    public 사람(String 국적) {
        this.국적 = 국적;
    }

    public void 주식() {
        if (국적.equals("한국")) {
            System.out.print("쌀");
        } else if (국적.equals("미국")) {
            System.out.print("빵");
        } else if (국적.equals("멕시코")) {
            System.out.print("옥수수");
        }else {
            throw new IllegalArgumentException("지원하지 않는 국적입니다: " + 국적);
        }
    }
}
```

else문이 중첩되며 코드의 가독성도 떨어지고 만약 이 메서드를 다른 곳에서 사용하기 어려워 집니다.

결국 확장성이 줄며 재 사용성이 낮아집니다. 이는 유지 관리 , 버그 발생의 원인이 됩니다.

그래서 OCP를 활용하는 좋은 방법 중 InterFace를 활용해 보겠습니다.

```java
public interface 주식 {
    void 출력();
}

public class 사람 {
    private final 주식 주식;

    public 사람(주식 주식) {
        this.주식 = 주식;
    }

    public void 먹는다() {
        주식.출력();
    }
}

public class 한국인 implements 주식 {
    @Override
    public void 출력() {
        System.out.print("쌀");
    }
}

public class 미국인 implements 주식 {
    @Override
    public void 출력() {
        System.out.print("빵");
    }
}

public class Main {
    public static void main(String[] args) {
        사람 한국인 = new 사람(new 한국인());
        한국인.먹는다(); // 쌀 출력

        사람 미국인 = new 사람(new 미국인());
        미국인.먹는다(); // 빵 출력
    }
}
```

이렇게 주식이라는 인터페이스를 미리 만들어두고, 필요에 따라 구현한다면 어떨까요

똑같이 추가한다면 이런 식으로 할 수 있습니다.

```java
public interface 주식 {
    void 출력();
}

public class 사람 {
    private final 주식 주식;

    public 사람(주식 주식) {
        this.주식 = 주식;
    }

    public void 먹는다() {
        주식.출력();
    } 
}

public class 한국인 implements 주식 {
    @Override
    public void 출력() {
        System.out.print("쌀");
    }
}

public class 미국인 implements 주식 {
    @Override
    public void 출력() {
        System.out.print("빵");
    }
}

public class 멕시코인 implements 주식 {
    @Override
    public void 출력() {
        System.out.print("옥수수");
    }
}

public class Main {
    public static void main(String[] args) {
        사람 한국인 = new 사람(new 한국인());
        한국인.먹는다(); // 쌀 출력

        사람 미국인 = new 사람(new 미국인());
        미국인.먹는다(); // 빵 출력
        
        사람 멕시코인 = new 사람(new 멕시코인());
        멕시코인.먹는다(); // 옥수수
    }
}
```

코드가 지금은 더 길어 보일 수는 있어도 메서드의 내용이 복잡하고 어려울 수록 기존 코드를 변경하지 않고 내가 필요한 부분만 고쳐서 사용할 수 있다는 장점이 부각되게 됩니다.

이전 포스트에서도 spring에서 예시를 들어 설명했던 부분이 있는데요

Overriding에 대한 포스트를 한번 읽어보시는걸 추천드리겠습니다.

- 코드의 확장성을 향상시킵니다.
- 코드의 유지 관리성을 향상시킵니다.
- 코드의 변경 가능성을 줄여 버그 발생 가능성을 감소시킵니다.
- 코드의 재사용성을 향상시킵니다.

정리하면 이런 장점을 OCP를 잘 사용하면 얻을 수 있습니다.

특히, 스프링 프레임워크에서는 정말 많은 부분에서 활용해 둔 것도 알아둘 수 있습니다. 

## **LSP - Liskov Substitution Principle  - 리스코프 치환 원칙**

“하위 유형은 기본유형으로 대체가 가능해야한다.”

상위클래스의 객체를 하위 클래스의 객체로 치환하여도 프로그램이 정상적으로 작동해야한다는 의미입니다.

따라서

하위형에서는 선행 조건을 강화 될 수 없고, 하위행에서 선행 조건을 약화 될 수 도없다.

하위형에서는 상위형의 불변조건을 반드시 유지해야한다. 라는 내용도 포함되어 있습니다.

코드를 통해 이해해 보겠습니다.

```java
public class 사각형 {
    protected int 가로;
    protected int 세로;

    public 사각형(int 가로, int 세로) {
        this.가로 = 가로;
        this.세로 = 세로;
    }

    public int 넓이() {
        return 가로 * 세로;
    }
}

public class 정사각형 extends 사각형 {
    public 정사각형(int 변) {
        super(변, 변);
    }

    @Override
    public void set가로(int 가로) {
        this.가로 = 세로 = 가로;
    }

    @Override
    public void set세로(int 세로) {
        this.가로 = 세로 = 세로;
    }
}

public class Main {
    public static void main(String[] args) {
        사각형 사각형 = new 사각형(4, 5);
        System.out.println("사각형 넓이: " + 사각형.넓이()); // 20 출력

        사각형 = new 정사각형(4);
        System.out.println("정사각형 넓이: " + 사각형.넓이()); // 16 출력

        ((정사각형) 사각형).set가로(5);
        System.out.println("변경 후 사각형 넓이: " + 사각형.넓이()); // 25 출력
    }
}
```

만약 해당 클래스에서 사각형을 정사각형으로 변경하면 어떤 문제가 발생할까요?

set가로를 부른 경우 세로의 값도 변경되게 되고, 그렇다면 원래 넓이를 구하는 방법과 달라져

의도한 결과와 다른 문제를 가지고 올 수 있습니다.

```java
public interface 도형 {
    int 넓이();
}

public class 사각형 implements 도형 {
    protected int 가로;
    protected int 세로;

    public 사각형(int 가로, int 세로) {
        this.가로 = 가로;
        this.세로 = 세로;
    }

    @Override
    public int 넓이() {
        return 가로 * 세로;
    }
}

public class 정사각형 implements 도형 {
    private int 변;

    public 정사각형(int 변) {
        this.변 = 변;
    }

    @Override
    public int 넓이() {
        return 변 * 변;
    }
}

public class Main {
    public static void main(String[] args) {
        도형 도형 = new 사각형(4, 5);
        System.out.println("사각형 넓이: " + 도형.넓이()); // 20 출력

        도형 = new 정사각형(4);
        System.out.println("정사각형 넓이: " + 도형.넓이()); // 16 출력
    }
}
```

그래서 이런식으로 인터페이스를 선언하고 상속 시켜 구현해 추상화시키면 각 클래스가 각 도형에 맞는 넓이의 계산공식을 도출하게 됩니다.

그래면 사각형 클래스를 정사각형으로 변경하여도 정상적으로 작동할 수 있게 되겠죠.

이런 원칙을 준수하면 확장성, 유지보수의 도움이 됩니다.

## **ISP -  Interface Segregation Principle- 인터페이스 분리 원칙**

하나의 인터페이스는 너무 많은 기능을 포함하지 않고, 클라이언트가 실제로 사용하는 기능만을 포함해야 한다

클라이언트가 사용하지 않는 기능을 포함하는 큰 인터페이스 대신에 작고 특정한 기능을 제공하는 여러 인터페이스를 사용하는 것을 의도하라는 원칙입니다.

```java
public interface 데이터처리 {
    void 읽기();
    void 쓰기();
    void 업데이트();
    void 삭제();
}

public class 파일처리 implements 데이터처리 {

    @Override
    public void 읽기() {
        // 파일 읽기 기능 구현
    }

    @Override
    public void 쓰기() {
        // 파일 쓰기 기능 구현
    }

    @Override
    public void 업데이트() {
        // 파일 업데이트 기능 구현
    }

    @Override
    public void 삭제() {
        // 파일 삭제 기능 구현
    }
}

public class 데이터베이스처리 implements 데이터처리 {

    @Override
    public void 읽기() {
        // 데이터베이스 읽기 기능 구현
    }

    @Override
    public void 쓰기() {
        // 데이터베이스 쓰기 기능 구현
    }

    @Override
    public void 업데이트() {
        // 데이터베이스 업데이트 기능 구현
    }

    @Override
    public void 삭제() {
        // 데이터베이스 삭제 기능 구현
    }
}

public class 프로그램 {

    public void 데이터가져오기(데이터처리 데이터처리) {
        데이터처리.읽기();
    }

    public void 데이터저장(데이터처리 데이터처리) {
        데이터처리.쓰기();
    }
}
```

이런식으로 작성을 하면 사용하는 부분은 읽기,쓰기 만 사용함으로 처리 class부분의 코드가 사용할 이유없이 길어지게 되면서 굳이 부르지 않아도 되는 메서드까지 불러서 사용을 하게 될 것입니다.

만약 저 기능이 100줄짜리 코드라면 시스템 성능에 악영향을 미칠 가능성이 높게 되죠

그래서 

```java
public interface 읽기 {
    void 읽기();
}

public interface 쓰기 {
    void 쓰기();
}

public interface 업데이트 {
    void 업데이트();
}

public interface 삭제 {
    void 삭제();
}

public class 파일읽기 implements 읽기 {

    @Override
    public void 읽기() {
        // 파일 읽기 기능 구현
    }
}

public class 파일쓰기 implements 쓰기 {

    @Override
    public void 쓰기() {
        // 파일 쓰기 기능 구현
    }
}

public class 데이터베이스읽기 implements 읽기 {

    @Override
    public void 읽기() {
        // 데이터베이스 읽기 기능 구현
    }
}

public class 데이터베이스쓰기 implements 쓰기 {

    @Override
    public void 쓰기() {
        // 데이터베이스 쓰기 기능 구현
    }
}

// 업데이트, 삭제 인터페이스 및 구현 클래스도 추가 가능

public class 프로그램 {

    public void 데이터가져오기(읽기 읽기) {
        읽기.읽기();
    }

    public void 데이터저장(쓰기 쓰기) {
        쓰기.쓰기();
    }
}
```

각각의 인터페이스를 분리해서 코드를 구현하게 되면

클라이언트에서 사용하는 부분만 추려서 구현이 가능하게됩니다.

만약 여기에 삭제 기능을 넣는다고 해도 다른 수정없이 바로 추가할 수 도 있어

코드변경에 유동적이게 되고, 재사용성도 높아진다고 이야기 할 수 있습니다.

## **DIP - Dependency Inversion Principle - 의존 역전 원칙**

고수준 모듈은 저수준 모듈에 의존해서는 안 되고, 둘 다 추상화에 의존해야 한다. 

추상화는 구체적인 구현에 의존해서는 안 되고, 구체적인 구현은 추상화에 의존해야 한다

즉, 코드는 구체적인 구현보다는 추상화에 의존해야지 결합도가 낮아지고, 유연성과 유지 관리성이 높아진다는 의미입니다.,

예를 들어 이런식으로 코드를 짜보겠습니다.

```java
public class 파일처리 {
    public void 읽기(String filename) {
        // 파일 읽기 로직 (파일 시스템 API 사용)
    }

    public void 쓰기(String filename, String data) {
        // 파일 쓰기 로직 (파일 시스템 API 사용)
    }
}

public class 프로그램 {

    private 파일처리 파일처리 = new 파일처리();

    public void 데이터가져오기(String filename) {
        파일처리.읽기(filename);
    }

    public void 데이터저장(String filename, String data) {
        파일처리.쓰기(filename, data);
    }
}
```

직접 코드를 구현하기 때문에 테스트환경을 구축한다면 여러 문제가 발생할 수 있습니다.

예를 들어 외부에서 받아온 경우에는 내부코드를 수정할 수 없거나 어렵기 때문에 테스트의 불 확실한 결과를 가져 올 것입니다.

그러면 제품에 대한 신뢰도가 떨어지게 되겠죠

```java
public interface 데이터소스 {
    String 읽기(String filename);
    void 쓰기(String filename, String data);
}

public class 파일시스템데이터소스 implements 데이터소스 {

    @Override
    public String 읽기(String filename) {
        // 파일 읽기 로직 (파일 시스템 API 사용)
        return "파일 내용";
    }

    @Override
    public void 쓰기(String filename, String data) {
        // 파일 쓰기 로직 (파일 시스템 API 사용)
    }
}

// 네트워크 데이터 소스, 데이터베이스 데이터 소스 등 추가 가능

public class 프로그램 {

    private 데이터소스 데이터소스;

    public 프로그램(데이터소스 데이터소스) {
        this.데이터소스 = 데이터소스;
    }

    public void 데이터가져오기(String filename) {
        String data = 데이터소스.읽기(filename);
        // 데이터 처리 로직
    }

    public void 데이터저장(String filename, String data) {
        데이터소스.쓰기(filename, data);
    }
}
```

이런식으로 인터페이스를 구축하여 추상화하고 구체적인 구현은 하위 클래스에서 처리하면 문제가 발생할 가능성이 없어지고, 외부라이브러리를 가져다 쓴다고 해도 테스트를 하는데 문제가 없어질 것입니다.

spring에서 service코드를 구현할때 serviceImpl를 만들어 구체적인 구현을 해당 클래스에서 처리하는 것이 대표적인 예로 볼 수있습니다.

## 정리하며

이번 포스트에서는 SOLID 라고하는 자바 객체지향의 원칙을 알아보았습니다.

결국 결합도는 낮추고 응집도는 높이기 위한 방법인데요

각 원칙이 반드시 적용시킨다고해서 결합도와 응집도에 영향을 주는 것보다는

각 원칙이 의도하는 방법대로 구현을 하기 위해 노력하다보면 자연스럽게

연관되어 영향을 준다고 이해하면 좋을 것 같습니다.