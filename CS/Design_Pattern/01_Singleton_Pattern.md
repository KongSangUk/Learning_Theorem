# Singleton pattern



## 정의

싱글턴(singleton)은 오직 하나의 객체만을 생성할 수 있는 클래스를 말합니다. 따라서 싱글턴 패턴을 사용하면 쉽게 객체의 유일성을 보장할 수 있습니다. 또한 일반적으로 싱글턴 객체에 대한 참조를 public static 필드나 public static 메서드로 노출하므로 어디에서나 싱글턴 객체에 접근할 수 있다.

![img](https://blog.kakaocdn.net/dn/ctQPjZ/btrBx1auqlI/O2INmcM9wkYtz8BjI1gRG0/img.png)



## 구현



### public static final 필드

객체가 오직 하나만을 보장하려면 어떻게 해야 할까? 

바로 정적(static) 필드를 사용하는 것다. 정적 필드를 사용하면 모든 객체가 공유하는 필드를 만들 수 있으며, 한 번만 생성되고 별도의 메모리 공간에 저장된다는 특징이 있다.

```
public class Singleton {
	public static Singleton INSTANCE = new Singleton();
	...
}
```



하지만 이것으론 부족하다. 외부에서 자유롭게 접근할 수 있기 때문에 이 필드에 다른 객체가 할당되거나, 이미 할당했는데 싱글턴 내부에서 다시 객체를 할당하는 실수가 없도록 final로 선언해야 한다. 그리고 외부에서 생성자를 통해 객체를 생성할 수 없도록 생성자의 접근 범위를 private로 제한해야 한다.



```
public class Singleton {
	public static final Singleton INSTANCE = new Singleton();
 
	private Singleton() { }
 
	...
}
 
// 혹은 ...
public class Singleton {
	private static final Singleton INSTANCE = new Singleton();
 
	private Singleton() { }
 
	public static Singleton getInstance() {
		return INSTANCE;
	}
 
	...
}
```



이러면 외부에서 아래와 같이 정적 필드로 접근할 수 있다.



```
// 생성자의 접근 범위가 private이기 때문에 생성자로는 객체를 생성할 수 없다.
// Singleton obj = new Singleton(); (X)
 
// INSTANCE는 final로 선언되었기 때문에 외부에서 다시 지정하는 것은 불가능하다.
// Singleton.INSTANCE = null; (X)
 
// 외부에서 정적 필드로 다음과 같이 접근할 수 있다.
Singleton.INSTANCE.service();
 
// 혹은...
Singleton obj = Singleton.getInstance();
obj.service();
```



### 지연 초기화(lazy initialization)

혹은 아래와 같은 방법으로도 초기화할 수 있다. 아래 예시에서는 Singleton.getInstance() 메서드 호출 시점에 정적 필드가 아직 초기화되지 않았으면 객체를 생성하고, 그 후에는 전에 생성한 객체의 참조(reference)를 그대로 반환한다.

```
public class Singleton {  
    private static Singleton instance;  
  
    private Singleton() { }  
  
    public static Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}
```

덧붙이자면, 사실은 여기에서 지연 초기화로 분류되지 않은 다른 방법들도 따지고 보면 지연 초기화처럼 동작한다.



#### 스레드 안전(THREAD SAFE)

여기서 주의할 점은 멀티 스레드 환경에서 위와 같은 초기화 방법을 사용할 경우 스레드 안전하지 않다는 문제가 있다. 두 개 이상의 스레드가 Singleton.getInstance()를 동시에 호출한다면 어떻게 될까?



![img](https://blog.kakaocdn.net/dn/ctaRyz/btrBu6DAbjp/b0oRHAkXCxeHkusTz4HQe1/img.png)

싱글턴 패턴으로 객체가 유일함을 보장하려고 했으나 위와 같이 스레드가 서로 다른 객체의 참조를 가지는 상황이 벌어질 수 있다. 따라서 한 번에 하나의 스레드만 접근할 수 있도록 다음과 같이 동기화를 해주어야 한다.



```
public class Singleton {  
    private static Singleton instance;  
  
    private Singleton() { }  
  
    public static synchronized Singleton getInstance() {  
        if (instance == null) {  
            instance = new Singleton();  
        }  
        return instance;  
    }  
}
```

하지만 이 방법은 정적 필드가 초기화된 후 싱글턴 객체를 얻으려고 할 때 불필요하게 동기화가 일어나므로 성능이 걱정된다면 다음의 방법을 사용할 수 있다.



#### 더블 체크 락킹(DOUBLE-CHECKED LOCKING)

여기서 객체가 올바르게 생성된 이후에는 별다른 수정 작업 없이 참조를 반환하는 작업만 있으므로 동기화 범위를 다음과 같이 줄일 수 있다.



```
public class Singleton {  
    private static Singleton instance; // (1)
  
    private Singleton() { }  
 
	// 코드가 다소 장황하지만 동기화 오버헤드를 피할 수 있으므로 성능 이점이 있다.
	public static Singleton getInstance() {
		if (instance == null) {
			// 동기화 블록은 한번에 하나의 스레드만 접근할 수 있다.
			synchronized (Singleton.class) {
				if (instance == null) {
					instance = new Singleton();
				}
			}
		}
		return instance;
	}
 
	...
}
```



코드만 보면 정상적으로 동작할 것 같지만 멀티 스레드 환경에서는 직관을 벗어나는 동작을 보일 수 있다. 

컴파일러가 최적화라는 명목으로 연산의 순서를 변경(reordering)할 수 있기 때문이다. 프로그래머는 이를 단일 스레드 환경에서는 알아차릴 수 없지만 멀티 스레드 환경으로 오면 이야기가 달라집니다. 문제는 다음과 같다.



```
if (instance == null) { // (1)
	synchronized (Singleton.class) { // (2)
		if (instance == null) { // (3)
			instance = new Singleton(); // (4)
		} // (5)
	} // (6)
} // (7)
return instance; // (8)
```



- 한 스레드가 4번에서 싱글턴 객체를 위한 메모리 공간을 할당하고 참조를 instance에 저장한 후에 싱글턴 객체의 생성자에서 내부 상태 초기화가 이루어지고 있다고 해보자.

- 이렇게 초기화하고 있는 도중에 1번으로 다른 스레드가 들어와서 null이 아님을 확인하고 초기화 중인 객체 참조를 그대로 반환할 수도 있다. 
- 따라서 외부에서는 올바르게 초기화되지 않은 객체의 상태를 관찰할 수 있다. 
- 이를 보고 문득 '객체의 초기화가 완전히 끝난 다음에야 그 객체에 대한 참조가 instance에 저장되는 게 아닌가?'라고 생각할 수 있지만 실제론 동기화 블록 내부에서 컴파일러로 인해 재배열이 일어나므로 다르게 동작할 수도 있다. 
- 그렇게 연산 순서가 뒤바뀌더라도 (단일 스레드에서는) 프로그래머가 관찰할 수 있는 결과는 바뀌지 않기 때문이다. 
- 하지만 멀티 스레드 환경에서는 위의 코드가 동시에 실행될 수 있으므로 이런 최적화는 적절하지 않는다.



**이를 방지하려면 어떻게 해야 할까?**



```
public class Singleton {  
    private static volatile Singleton instance; // (1)
 
	...
}
```

바로 위와 같이 instance 필드를 volatile로 선언하는 것다. 이 키워드를 사용하여 instance 필드에 값을 쓸 때 이러한 재배열이 일어나지 않도록 컴파일러에게 지시할 수 있습니다(물론 volatile은 그 이상의 일을 이다) 따라서 최종적으로는 아래와 같이 쓸 수 있다.



```
public class Singleton {  
    private static volatile Singleton instance;
  
    private Singleton() { }  
 
	public static Singleton getInstance() {
		if (instance == null) {
			synchronized (Singleton.class) {
				if (instance == null) {
					instance = new Singleton();
				}
			}
		}
		return instance;
	}
 
	...
}
```



#### 요청 시 초기화 홀더 패턴(INITIALIZATION-ON-DEMAND HOLDER PATTERN)



이 방법은 홀더 클래스를 사용해서 지연 초기화를 구현합니다. 더블 체크 락킹보다 더 단순하며 안전하기까지 한다.



```
public class Singleton {  
 
    private Singleton() { }
    
    private static final class Holder {  
        private static final Singleton INSTANCE = new Singleton();  
    }  
    
    public static Singleton getInstance() {  
        return Holder.INSTANCE;  
    }
 
	...
}
```

어떻게 이런 방법을 사용할 수 있을까? Holder.INSTANCE는 결국엔 즉시 초기화를 장황하게 사용하는 방법이 아닐까? 물론 아니다. 이를 이해하려면 초기화가 언제 일어나는지 알고 있어야 하는데, 아래 내용은 [JLS 12.4.1](https://docs.oracle.com/javase/specs/jls/se9/html/jls-12.html#jls-12.4.1)에서 확인할 수 있는 내용이다.



> 클래스나 인터페이스 타입 T는 다음 중 하나가 처음 일어나기 직전에 초기화.
>
> \- T는 클래스이며 T의 인스턴스가 생성된다.
> \- T에 선언된 정적 메서드가 호출된다.
> \- T에 선언된 정적 필드가 할당된다.
> (추가: 예를 들어서 외부에서 공개된 정적 필드에 값을 할당하는 등이 있다.)
> \- T에 선언된 정적 필드가 사용되며 이때 이 필드는 상수 변수가 아니다.
> (JLS 4.12.4: 상수 변수는 상수 표현식으로 초기화된 기본 타입이나 String 타입의 final 변수를 말한다.)



따라서 Holder 클래스에 선언된 정적 필드인 INSTANCE가 사용될 때 Holder 클래스의 초기화가 일어난다. 즉, 위의 예시에서는 런타임에 Singleton.getInstance()를 호출하여 Holder.INSTANCE을 사용하기 전에 클래스로더를 통해 Holder 클래스의 초기화가 일어나게 된다. 그와 동시에 Holder 클래스의 초기화 단계에서 정적 필드 INSTANCE의 초기화가 단 한 번만 일어난다.

**굳이 중첩 클래스를 사용할 필요는 없지 않을까?***

맞다. JLS에서 위와 같은 내용을 보장하므로 Singleton 클래스의 초기화는 Singleton 클래스의 '정적 메서드' 혹은 '상수 변수가 아닌 정적 필드'를 사용하는 게 아닌 이상은 이루어지지 않을 것이다. 따라서 아래와 같이 작성해도 지연 초기화처럼 동작한다. 대부분의 경우 싱글턴에는 공개된 정적 메서드가 getInstance() 하나뿐이므로 그렇게 생각하는 것이 자연스럽습다.



```
public class Singleton {
	private static final Singleton INSTANCE = new Singleton();
 
    private Singleton() { }
    
    public static Singleton getInstance() {  
        return INSTANCE;  
    }
 
	...
}
```

만약에 getInstance() 이외의 공개된 정적 메서드가 있다면 Holder 중첩 클래스를 만드는 게 적절하지만 그런 사례를 좀처럼 떠올려 볼 수가 없다



### 열거형을 사용하는 방법

---

이번 방법은 바로 조슈아 블로치(Joshua Bloch)가 이펙티브 자바에서 소개한 열거형을 사용하는 방법이다. 지금까지 소개한 방법 중에 가장 간결하고 완벽한 방법이며 이 방법을 통해 리플렉션 API를 통해 인스턴스를 만드려는 시도를 손쉽게 무력화할 수 있습니다. 거기에다가 추가적인 노력 없이 직렬화할 수도 있다. Enum을 [공식 문서](https://docs.oracle.com/javase/specs/jls/se9/html/jls-8.html#jls-8.9)에서 살펴보면 다음과 같은 내용을 확인할 수 있다.



> 열거형에는 열거형 상수를 통해 정의된 인스턴스 이외의 인스턴스는 없습니다. 열거형을 명시적으로 인스턴스화하려고 시도하면 컴파일 타임 에러가 발생합니다. 컴파일 타임 에러 외에도 아래 세 가지 메커니즘이 열거형 상수를 통해 정의된 인스턴스 이외의 열거형 인스턴스가 존재하지 않음을 보장합니다.
>
> \- Enum의 final clone() 메서드를 통해 열거 상수를 복제할 수 없음을 보장한다.
> (추가: 복제하려고 하면 CloneNotSupportedException 예외를 던진다.)
> \- 직렬화 메커니즘을 통한 특수 처리로 역직렬화의 결과로 중복 인스턴스가 생성되지 않음을 보장한다.
> \- 리플렉션을 통해 열거형을 인스턴스화하는 것은 금지된다.



조금은 코드가 어색해보일 수 있지만 여태껏 소개한 방법 중에 가장 완벽한 방법입니다. 열거형에서도 이미 봤겠지만 열거형은 다른 클래스를 상속받을 수 없으므로, 클래스 상속이 필요하다면 이 방법은 사용할 수 없으니 참고하자.

```
public enum Singleton {
	INSTANCE;
 
	...
}
```



위를 좀 더 익숙한 코드로 바꾸면 실은 아래와 같다.



```
// 이 코드는 동작하지 않으니 참고만 하자.
final class Singleton extends Enum<Singleton> {
	public static final INSTANCE = new Singleton();
	...
}
```



## 문제점

싱글턴 패턴은 보통 객체의 유일성이 아니라 전역 접근, 즉 어디에서나 접근할 수 있다는 점을 과도하게 오용할 때 문제가 된다. 주로 이 패턴은 아래와 같은 문제점을 지니고 있다.



#### 테스트하기가 어렵다

정적 필드는 한번 할당되면 보통은 프로그램이 종료되기 전까지 계속 살아있게 된다. 각 테스트는 독립적, 즉 다른 테스트에 영향을 미치지 않아야 하는데 한 테스트에서 싱글턴 객체가 만들어지면 그 이후의 다른 테스트에서도 이를 확인할 수 있다. 또한 일반적으로 인터페이스가 아닌 클래스를 통해 구현되는 싱글턴은 목(mock)으로 대체할 수 없으므로 단위 테스트를 매우 까다롭게 만든다. 이를 해결하기 위해서 주로 의존관계 주입(dependency injection, 이하 DI)을 사용할 수 있으며 이때는 보통 DI 프레임워크가 싱글턴 객체의 생성을 제어한다.



#### 데이터 경쟁이 일어나기 쉽다

이는 굳이 싱글턴 패턴이 아니더라도 멀티 스레드 환경에서 공유할 수 있는 상태(쉽게 말해서 필드)를 가지고 있으면 항상 조심해야 하는 내용이다. 싱글턴이 상태를 가지고 있다면 상황이 더 복잡해지며, 멀티 스레드 환경에서는 공유 변수 접근 시 적절하게 동기화가 이루어지지 않았다면 경쟁 상태(race condition)가 문제가 될 수 있으므로 주의해야 한다.



#### 변경에 취약해진다

어디에서나 접근할 수 있으므로 싱글턴의 구조나 동작에 변경이 일어나면 싱글턴에 의존하고 있는 클래스에서도 역시 문제가 발생한다. 이렇게 긴밀하게 연결된 관계는 테스트가 어렵다는 문제점으로도 이어진다.



## 참고

- Bloch, J., 2018. _Effective Java_. 3rd ed. Boston [etc.]: Addison-Wesley, pp.17-18. (Item 3: Enforce the singleton property with a private constructor or an enum type)
- [The "double-checked locking is broken" declaration.](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html) (n.d.). Retrieved May 7, 2022