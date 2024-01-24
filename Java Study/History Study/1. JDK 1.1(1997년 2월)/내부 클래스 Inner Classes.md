##### 멤버 내부 클래스 (Member Inner Class)
다른 클래스 내부에서 정의되고, 해당 클래스의 멤버로 간주된다. 외부 클래스의 멤버 변수 및 메서드에 쉽게 접근할 수 있다.

```java
public class OuterClass {
    private int outerVar;
    // 멤버 내부 클래스
    public class InnerClass {
        public void innerMethod() {
            System.out.println("OuterVar: " + outerVar);
        }
    }
}
```

##### 정적 내부 클래스 (Static Inner Class)
외부 클래스의 인스턴스에 종속되지 않고, 정적으로 정의된 클래스입니다. 주로 외부 클래스의 정적 멤버에 접근할 필요가 있을 때 사용한다.
```java
public class OuterClass {
    private static int staticVar;
    // 정적 내부 클래스
    public static class StaticInnerClass {
        public void staticInnerMethod() {
            System.out.println("StaticVar: " + staticVar);
        }
    }
}
```

##### 지역 내부 클래스 (Local Inner Class)
특정 메서드 내에서만 유효한 클래스로, 해당 메서드 내부에서만 인스턴스화 및 사용할 수 있다.
```java
public class OuterClass {
    public void outerMethod() {
        int localVar = 10;
        // 지역 내부 클래스
        class LocalInnerClass {
            public void localInnerMethod() {
                System.out.println("LocalVar: " + localVar);
            }
        }
        LocalInnerClass inner = new LocalInnerClass();
        inner.localInnerMethod();
    }
}
```

##### 익명 내부 클래스 (Anonymous Inner Class)
이름이 없는 클래스로, 주로 인터페이스나 추상 클래스의 인스턴스를 생성할 때 사용한다.
```java
public interface MyInterface {
    void myMethod();
}

public class OuterClass {
    public void createAnonymousInner() {
        // 익명 내부 클래스
        MyInterface anonymousInner = new MyInterface() {
            @Override
            public void myMethod() {
                System.out.println("Anonymous");
            }
        };
        anonymousInner.myMethod();
    }
}
```
