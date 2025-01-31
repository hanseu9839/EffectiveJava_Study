# item15:클래스와 멤법의 접근 권한을 최소화하라
잘 설계된 컴포넌트의 가장 큰 차이는 바로 클래스 내부 데이터와 **내부 구현 정보를 외부 컴포넌트**로부터 얼마나 잘 숨겼느냐다 <br/>

- 시스템 개발 속도를 높일 수 있음. 여러 컴포넌트를 병렬로 개발할 수 있기 때문
- 시스템 관리 비용 ⬇️ 각 컴포넌트 더 빨리 파악하여 디버깅할 수 있고, 다른 컴포넌트로 교체하는 부담도 적다.
- 정보 은닉 자체가 성능을 높여주지는 않지만, 성능 최적화에 도움이된다. -> 해당 컴포넌트에만 집중 할 수 있기 때문이다.
- 소프트웨어 재사용성을 높인다. 외부에 거의 의존하지 않고 독자적으로 동작할 수 있는 컴포넌트라면 그 컴포넌트와 함께 개발되지 않은 환경에도 적용될 가능성이 높다.
- 큰 시스템을 제작하는 난이도를 낮춰준다. 


큰 시스템 제작 난이도를 낮춰준다고 하는데 아무래도 결합이 코드간에 높지 않기 때문에 난이도를 낮춰주는 거 같음. <br/>
실제로 도메인 단위로 프로젝트를 관리했을 때, 코드를 쉽게 수정 할 수 있는 원리와 비슷한 원리 같음 <br/>

## **💡핵심 주제**
**모든 클래스와 멤버의 접근성을 가능한 한 좁혀야 한다.** -> 소프트웨어가 올바로 동작하는 한 가장 낮은 접근 수준 부여

- private : 멤버를 선언한 톱레벨 클래스에서만 접근할 수 있다.
- package-private: 멤버가 소속된 패키지 안의 모든 클래스에서 접근
- protected: package-private 접근 범위 포함, 이 멤버를 선언한 클래스의 하위 클래스에서 접근 가능
- public: 모든 곳에서 접근 가능

### 멤버 접근성을 좁히지 못하게 방해하는 제약 
- 상위 클래스 메서드를 재정의 할 때, 그 접근 수준을 상위 클래스에서보다 좁게 설정 불가능 => 리스코프 치환 원칙 (상위 클래스의 인스턴스는 하위 클래스의 인스턴스로 대체해 사용 가능)
- 인터페이스 정의한 모든 메서드를 public 선언

**public 클래스의 인스턴스 필드는 되도록 public이 아니어야 한다.**
- 외부 변경점을 최소화하기 위해 해당 방식으로 진행

**public 가변 필드를 갖는 클래스는 일반적으로 스레드 안전하지 않다**
- 공유 자원의 경우 private 되어 있지 않다면 언제어디서든 접근하여 바꾸기 때문이다. 

**public static final 필드는 외부로 공개 가능** : 관례상 이런 상수의 이름은 대문자 알파벳으로 사용, 각 단어 사이에 밑줄(_)을 넣는다. 

**클래스에서 public static final 배열 필드를 두거나 일 필드를 반환하는 접근자 메서드를 제공해서는 안된다** : 배열을 참조하여 메모리 주소에 있는 값을 실질적으로 저장 할 수 있기 때문임.


이를 해결하는 방법 두가지 

```java
import java.util.Collections;

private static final Thing[] PRIVATE_VALUES = { ...};
public static final List<Thing> VALUES = Collections.unmodifiableList(Arrays.asList(PRIVATE_VALUES));
```

```java
private static final Thing[] PRIVATE_VALUES = { ... };
public static final Thing[] values() {
    return PRIVATE_VALUES.clone();
}
```

