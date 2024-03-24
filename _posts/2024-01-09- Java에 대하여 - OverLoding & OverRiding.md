---
title: 자바에 대하여 - OverLoding & OverRiding
date: 2024-01-09 06:44:00 +09:00
categories:
  - java
tags:
  - [java, OOP, 다형성 ,OverLoding ,OverRiding ]
---
# 6.JAVA-OverLoding & OverRiding

## 들어가며

이번 포스트에서는 overLoding 과 OverRiding에 대한 이야기를 해보려고 합니다.

OOP 에서 다형성을 구현하는 두 가지의 중요한 개념입니다.

## OOP - 다형성 Polymorphism

다형성 (Polymorphism)은 이전 포스트에서 간단하게 정리하여서 개념만 집고 넘어가면

같은 이름의 메서드 or 연산자를 다양한 객체에 대해 서로 다른 방식으로 동작하도록 하는 원리를 말합니다. 당연히 재사용성과 유연성이 높아지는 결과를 가지고 옵니다.

## OverLoding

오버로딩은 같은 이름의 연산자나 메서드를 여러개 정의하는 것을 말합니다.

보통 이름은 같지만 매개변수의 타입, 갯수, 순서가 다른 경우를 파악해 구현합니다.

이것을 파악하는 것은 정적 바인딩 (정적 다형성)에 의해 결정됩니다.

특징으로는 컴파일 하는 시간, 즉 프로그램이 올라가기 전에 동작하게 됩니다.

예시를 하나 들어보면 

```java
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public double add(double a, double b) {
        return a + b;
    }
    public int add(int a, int b, int c) {
        return a + b + c;
    }
}
```

이렇게 구현해볼 수 있습니다.

1. 2개의 int타입
2. 2개의 실수형 double 타입
3. 3개의 int타입 

다형성은 생각보다 자주 사용합니다.

개인적인 경험으로 프로젝트 중 칵테일 머신의 종류를 구분하는 코드를 작성한 경험이 있는데

이때 각 칵테일은 2~4개 사이의 base가 되는 술의 정보를 가지고 있었습니다. (이 base들을 섞어 cocktail로 제조하는 방법을 만드는 로직을 구현하였습니다.)

이때 각 매개변수의 개수를 파악해 활용해봤던 경험이 있습니다. 

그런데 정적 다형성? 이라는 단어는 처음 들어보네요 찾아보면 다음과 같습니다.

## 정적 다형성 과 동적 다형성

**정적 다형성(Static Polymorphism) 또는 컴파일 시 다형성(Compile-time Polymorphism)**

**동적 다형성(Dynamic Polymorphism) 또는 실행 시 다형성(Runtime Polymorphism)**

이렇게 구분해볼 수 있습니다. 즉, 컴파일이 되는 시간동안 동작하는Overloading때 사용하면

**정적다형성**

프로그램이 실행되는 동안 동작하는 즉, Overriding 때 사용하면 

**동적다형성**

이렇게 구분할 수 있습니다.

특징에 대한 정리를 해보면

1. **정적 다형성(Static Polymorphism) 또는 컴파일 시 다형성(Compile-time Polymorphism):**
    - 정적 다형성은 컴파일 시간에 발생합니다.
    - 메서드 오버로딩(Overloading)을 통해 구현됩니다.
    - 컴파일러가 코드를 분석하여 어떤 메서드가 호출될지 결정합니다.
    - 오버로딩된 메서드 중에서 컴파일 시에 적절한 메서드가 선택되어 실행됩니다.
    
2. **동적 다형성(Dynamic Polymorphism) 또는 실행 시 다형성(Runtime Polymorphism):**
    - 동적 다형성은 실행 시간에 발생합니다.
    - 메서드 오버라이딩(Overriding)을 통해 구현됩니다.
    - 상속 관계에 있는 클래스들 간에 발생합니다.
    - 부모 클래스의 참조 변수로 자식 클래스의 객체를 참조할 때 발생합니다.
    - 메서드 호출은 실제 객체의 타입에 따라 동적으로 결정됩니다.
    - 가상 메서드 테이블(Virtual Method Table, VMT)을 사용하여 메서드 호출을 동적으로 연결합니다.

## Overriding?

오버라이딩은 말 그대로 덮어쓰다 라는 의미로 받아들이면 됩니다.

부모 클래스의 메서드를 자식 클래스의 메서드에서 재정의하여 사용한다는 개념입니다.

오버라이딩은 런타임 시에 동적 바인딩(동적 다형성)에 의해 결정됩니다.

여기서 상속시킨 클래스가 확장의 개념을 가지고 오게 됩니다. 예를 하나 들어보며 어떤 식으로 사용하는지 보이겠습니다.

만약 Spring을 위주로 공부해본 사람이라면 Extends라는 용어를 자주 클래스 옆에 붙이게 됩니다.

예로 인증구조를 만들기 위해 

  

```java
public class SecurityConfig extends WebSecurityConfigurerAdapter
```

이런 식의 클래스를 정의 하여 시스템 요구 사항에 맞게 고치게 되는데

이때 개념이 바로 확장의 개념이 됩니다.

즉, WebSecurityConfigurerAdapter라는 부모클래스를 SecurityConfig에서 재정의 하게 되는 것입니다.

그리고 자세한 메서드를 구현하게 되면 

```java
@Configuration
@EnableWebSecurity
public class SecurityConfig extends WebSecurityConfigurerAdapter {

    @Override
    protected void configure(HttpSecurity http) throws Exception {
        http.authorizeRequests()
            .antMatchers("/public/**").permitAll() // 특정 URL은 인증 없이 접근 허용
            .anyRequest().authenticated() // 그 외의 요청은 인증이 필요함
            .and()
            .formLogin(); // 기본 로그인 폼 사용
    }
}
```

이런식으로 Override 해서 메서드를 재정의하는 것을 알 수 있습니다.

원래 인증을 만들기 위해서는 아주 어려운 코드를 작성해야 하는데 spring-boot-starter-security 패키지를 받아와 내가 원하는 부분만 빠르게 만들 수 있는 즉, 생산성이 높아지는 결과를 보여줍니다.

좀 더 간단한 코드로 직관적으로 이해해 보면

```java
class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow");
    }
}
```

Animal 부모 클래스를 Cat 자식클래스가 받아와 메서드를 재 정의하여 사용하고 있습니다.

즉, 클래스 간에 동일한 이름의 메서드를 사용하여 부모 클래스의 메서드를 자식 클래스에서 수정하거나 확장할 수 있습니다.

## @Override

자주 쓰는 어노테이션인데 따로 찾아본 경험은 없는 것 같아 정리했습니다.

상위 클래스나 인터페이스에서 이미 정의된 메서드를 하위 클래스에서 다시 정의할때

자바 컴파일러에게 명시적으로 여기  OverRiding 할꺼임 이라고 알려줍니다.

그래서 특징으로 

1. **컴파일 타임 체크:** **`@Override`** 어노테이션을 사용하면 컴파일러가 해당 메서드가 오버라이드되었는지를 검사합니다. 만약 오버라이드되지 않은 메서드를 **`@Override`**로 표시하면 컴파일러가 오류를 발생시킵니다.
2. **코드 가독성 향상:** **`@Override`** 어노테이션을 사용하면 메서드가 오버라이드되었음을 명시적으로 표시할 수 있습니다. 이는 코드를 읽는 사람이 해당 메서드가 어떤 용도로 사용되는지를 쉽게 이해할 수 있도록 도와줍니다

## 정리하며

이번 포스트 에서는 다형성의 중요개념인 OverLoding 과 OverRiding에 대해 알아봤습니다.

정말 간단하게 정리해보면

OverLoding : 컴파일 실행  ,  정적 바인딩 , 매개변수를 통해 구분

OverRiding :  프로그램 실행 , 동적 바인딩 , @Override 어노테이션을 통해 구분

또한, @ 어노테이션에 대한 언급을 했는데요

다음 포스트에서는 @ 어노테이션에 대해 이야기 해보겠습니다.

## 다음 포스트

@ 어노테이션에 대하여