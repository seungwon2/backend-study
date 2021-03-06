<!-- @format -->

# 싱글톤 컨테이터

## 웹 애플리케이션과 싱글톤

> 💡 웹 어플리케이션에서 싱글톤이 필요한 이유?

- 스프링은 기업용 온라인 서비스 기술을 지원하기 위해 탄생
- 보통 기업용 온라인 서비스는 여러 고객이 동시에 요청을 함
- 고객 요청 시마다 새로운 객체를 생성하면 메모리 낭비가 굉장히 심함
- 객체가 딱 1개만 생성되고, 고객끼리 공유하도록 설계해야 함 == 싱글톤 패턴

## 싱글톤 패턴

### 정의

> 💡 클래스의 인스턴스가 딱 1개만 생성되는 것을 보장하는 디자인 패턴

- `private` 생성자를 사용해 외부에서 임의로 new 키워드를 사용하지 못하도록 막는 방법 == 객체를 미리 만들어놓고 공유하는 방식

### 문제점

1. 싱글톤 패턴을 구현하는 코드 자체가 많이 들어간다.
2. 의존관계상 클라이언트가 구체 클래스에 의존한다. DIP를 위반한다.
3. 클라이언트가 구체 클래스에 의존해서 OCP 원칙을 위반할 가능성이 높다.
4. 테스트하기 어렵다.
5. 내부 속성을 변경하거나 초기화 하기 어렵다.
6. private 생성자로 자식 클래스를 만들기 어렵다.
7. 결론적으로 유연성이 떨어진다.
8. 안티패턴으로 불리기도 한다.

### 문제점 해결

- 스프링 컨테이너는 싱글톤 패턴의 문제점을 해결하면서 객체 인스턴스를 싱글톤으로 관리한다.
- 스프링 빈 그냥 쓰면 싱글톤 관리됨

## 스프링 컨테이너 == 싱글톤 컨테이너

> 💡 스프링의 @Configuration이 싱글톤을 만들기 위해 바이트코드를 세로 쓴다.

- 스프링 컨테이너는 싱글턴 패턴을 적용하지 않아도, 객체 인스턴스를 싱글톤으로 관리한다.
- 이전에 설명한 컨테이너 생성 과정을 자세히 보자. 컨테이너는 객체를 하나만 생성해서 관리한다.
- 스프링 컨테이너는 싱글톤 컨테이너 역할을 한다. 이렇게 싱글톤 객체를 생성하고 관리하는 기능을 `싱글톤 레지스트리`라 한다.
- 스프링 컨테이너의 이런 기능 덕분에 싱글턴 패턴의 모든 단점을 해결하면서 객체를 싱글톤으로 유지할 수있다.
- 싱글톤 패턴을 위한 지저분한 코드가 들어가지 않아도 된다.
- `DIP`, `OCP`, `테스트`, `private` 생성자로 부터 자유롭게 싱글톤을 사용할 수 있다.

## 싱글톤 방식의 주의점

> 💡 싱글톤 객체는 상태를 유지해서는 안되고, 무상태로 설계해야 한다.

- 객체 인스턴스를 하나만 생성해서 공유하는 방식이므로! 상태가 있으면 이전 고객이 쓰던거 새고객이 넘겨받음..
- 되도록이면 읽기만 가능하게 설계해야 함

1. 특정 클라이언트에 의존적인 필드가 있으면 안된다.
2. 특정 클라이언트가 값을 변경할 수 있는 필드가 있으면 안된다!
3. 가급적 읽기만 가능해야 한다.
4. 필드 대신에 자바에서 공유되지 않는, 지역변수, 파라미터, ThreadLocal 등을 사용해야 한다.
