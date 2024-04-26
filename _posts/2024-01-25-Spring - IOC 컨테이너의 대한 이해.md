---
title: Spring - IOC 컨테이너의 대한 이해
date: 2024-01-25 06:44:00 +09:00
categories:
  - spring
tags:
  - [java, spring, ioc , bean ]
---
## 들어가며

IOC란 Iversion of Control : 의존 관계 주입 - (DI) 와 밀집한 관계를 가지고 있습니다.

객체가 사용하는 의존 객체를 직접 만드는 것이 아닌 주입 받아 사용하는 것을 말합니다.

```java
private final PostReposioty postRepository = new PostRepository();
```

이런 식으로 만드는 것이 아니라

```java
private final PostReposioty postRepository
```

이렇게 주입 받아 만드는 것을 DI라고 하는데 이때 우리가 자주 보는 생성자를 통해 의존성을 주입하여 사용하게 됩니다.

```java
public PostService(PostRepository postRepository){
	this.postRepository = postRepository;
}
```

당연히 위에 방법에서 정의하는 방식으로도 설정이 가능해 postRepository를 불러와서 메서드를 만들고 새롭게 만들어낸 서비스 객체에 레포지토리를 매개변수로 주입시키는 방법도 가능합니다.

다만, 이런 방법은 아주 오래전부터 개발자들 사이에 논의를 통해 정의 된 방법인 아래 방법을 통해 구축하는 것이 안전하고 효과적입니다.

굳이? 하지 않아도 되는 것을 할 이유는 없습니다. (특별한 경우를 제외하면)

## 저는 이렇게 씁니다.

저는 주로 @RequiredArgsConstructor 어노테이션을 통해 사용하는데요

@RequiredArgsConstructor 란,

 Lombok에서 제공하는 애노테이션 중 하나로, 클래스의 필수 인자에 대한 생성자를 자동으로 생성해줍니다. 이 애노테이션을 사용하면 필드에 **`final`** 키워드가 붙은 경우에 대해서만 생성자가 자동으로 생성됩니다.

일반적으로 레포지토리를 불러다 쓰는 서비스 , 서비스를 부르는 컨트롤러에서는 더 이상 수정을 하지 않는 다는의미의 **`final`** 키워드를 붙이기 때문에 코드 가독성이 높아지는 것이 사실입니다.

final 이라는 것이 변수의 붙이며 ‘초기화 - 최초값을 넣고 나면 변경 X 상수’ , 메서드의 붙이면 하위 클래스에서 해당 메서드를 재정의 - 오버라이딩 할 수 없다는 의미입니다.

오버로딩과 오버라이딩은 다른 포스트에서 정리하였으니 참고해주시길 바랍니다.

즉, 내 의도 이상으로 변경이 되는것을 방지해주는 역할을 합니다.

## Bean

빈은 자바에서 관리하는 객체 - 스프링에서 관리하는 객체 라는 의미인데 정확한 구분을 해야합니다.

자바에서 관리하는 객체는 클래스를 통해 구축되었다면 모두 자바 빈이라고 부를 수 있습니다.

그리고 스프링 빈 - 이라면 스프링 컨테이너에서 관리하는 빈 - 객체 라는 것을 이해해야 합니다.

우리가 흔히 사용하는 **@Service , @Repository , @Configuration** 이렇게 객체를 컨테이너에 알려두고 등록시켜 둔 애들만 스프링 빈이라고 해야합니다.

### 빈 등록하는 방법

스프링 빈을 주입시키는 방법이 3가지있는데요. xml , 자바코드 ,어노테이션이 있습니다.

xml을 예전에는 많이 썼다고 하는데 개발자들이 어노테이션 방향으로 주입시키는 방법을 의도하였다고 합니다.

```java

```

이렇게 했는데 사실 불편한 건 사실입니다, 그래서 어노테이션은 미리 지정된 **@Service , @Repository** 이런 애들을 통해서 정의를 합니다.

```java
@Service
public class UserServiceImpl implements UserService {
    // Implementation...
}

@Repository
public class UserRepositoryImpl implements UserRepository {
    // Implementation...
}
```

그런데 외부에서 온 라이브러리 같이 주입을 시킬 때 사용자가 커스터 마이징해서 정의를 한 다음 스프링에 빈으로 등록 해야하는 경우가 있습니다. 이럴 때는 자바 코드를 통해 정의하고 등록을 하는데요

대표적으로 **@Configuration** 을 통해 만듭니다. 저는 S3나 SpringSecurity를 쓸 때 사용했던 경험이 있습니다.

 

```java
@Configuration
public class AppConfig {

    @Bean
    public UserService userService() {
        return new UserServiceImpl();
    }

    @Bean
    public UserRepository userRepository() {
        return new UserRepositoryImpl();
    }
}
```

## 그럼 왜 등록을 하는 가?

DI - 의존성을 주입 받기 위해서는 스프링 IOC컨테이너에 등록이 되어야 하는 이유가 가장 큽니다.

앞서 선언하는 방법에 대해 이야기했던 부분이죠.

또한 , 빈 관리가 유용합니다. 의존성을 관리하는 부분과 스코프 - 싱글톤 패턴으로 만드는 거라면

다른 설정 없이 곧바로 사용할 수 있게 됩니다. 예를 들어 postService가 다른 곳에서 불려서 재 정의를 통해 계속 변경되지 않는 스코프 - 즉, 범위를 가지고 있다면 , 하나만 있어도 충분합니다.

굳이 여러개의 객체를 생성하는 것이 아니라 생성을 미리 해놓고 사용하면 자원관리가 효율적이게 관리하는 것이 더 좋은 선택일 것입니다. 

## 그럼 의존성 주입은 왜 하는 가?

DI를 따로 포스트 할려고 정리 중 이긴한데 말이 나온김에 간단하게 생각해보면

테스트를 하는데 용의하게 작성을 하려고 입니다.

만약 new - 를 통해 새로운 객체를 생성하는 방법으로 짠다면 Mock프레임워크를 통해

가짜 객체를 만드는 것이 안되겠죠.

그러면 의도대로 코드는 작성을 했는데 반대되는 결과값이 있어 테스트가 어려울 때 상당히 난감할 것입니다. 그래서 의존성을 주입 받으면 실제 생성되는 것은 돌아갈 때  만들어 지니까 Mock을 통해 

구현하여도 문제가 없어집니다.