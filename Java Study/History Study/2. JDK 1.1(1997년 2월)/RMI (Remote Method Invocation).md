RMI(원격 메서드 호출)는 Java에서 제공하는 기술 중 하나로, 분산 애플리케이션을 개발하기 위해 사용된다. RMI를 사용하면 다른 Java 가상 머신(VM) 또는 원격 기계에 위치한 Java 객체의 메서드를 호출할 수 있다. 


### 원격 인터페이스 정의
먼저, 원격으로 호출될 메서드를 선언하는 원격 인터페이스를 만든다.
```java
// RemoteMath.java
import java.rmi.Remote;
import java.rmi.RemoteException;

public interface RemoteMath extends Remote {
    int add(int a, int b) throws RemoteException;
}
```
- `RemoteMath` 인터페이스는 `Remote` 인터페이스를 확장합니다. `Remote` 인터페이스는 RMI(원격 메서드 호출)에서 원격으로 호출될 수 있는 메서드를 포함한다.
- `Remote` 인터페이스는 마커 인터페이스(Marker Interface)로, 메서드를 갖지 않고 말그대로 "원격" 가능한 인터페이스임을 나타냅니다. 이를 통해 해당 인터페이스가 원격으로 호출될 수 있음을 알 수 있다.
- `add` 메서드는 클라이언트에서 서버로 전송되어 실행될 메서드로, 두 정수를 더한 결과를 반환한다. `RemoteException`은 원격 호출 중 발생할 수 있는 예외를 처리하기 위해 선언되어 있다.
### 원격 객체 구현
위에서 정의한 원격 인터페이스를 구현하는 클래스를 만든다.
```java
// RemoteMathImpl.java
import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class RemoteMathImpl extends UnicastRemoteObject implements RemoteMath {
    public RemoteMathImpl() throws RemoteException {
        // 생성자에서 RemoteException을 처리해야 합니다.
    }

    @Override
    public int add(int a, int b) throws RemoteException {
        return a + b;
    }
}
```
- `RemoteMathImpl` 클래스는 `UnicastRemoteObject`를 확장하고 `RemoteMath` 인터페이스를 구현한다.
- `UnicastRemoteObject`는 원격 객체를 생성하고, 클라이언트로부터 호출을 받을 수 있도록 해줍다.
- `RemoteException`은 원격 메서드 호출 시 발생할 수 있는 예외를 처리하기 위해 사용된다.
- `add` 메서드는 원격에서 호출될 메서드로, 두 정수를 더한 결과를 반환한다.
### 서버 구성
RMI 서버를 생성하고 원격 객체를 등록한다.
```java
// MathServer.java
import java.rmi.Naming;

public class MathServer {
    public static void main(String[] args) {
        try {
            RemoteMath remoteMath = new RemoteMathImpl();
            Naming.rebind("MathService", remoteMath);
            System.out.println("MathServer is ready.");
        } catch (Exception e) {
            System.err.println("MathServer exception: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```
- `MathServer` 클래스는 RMI 서버를 구현한다.
- `RemoteMathImpl` 객체를 생성하고, `Naming.rebind`를 사용하여 이를 "MathService"라는 이름으로 등록한다.
- 서버가 성공적으로 시작되면 "MathServer is ready." 메시지를 출력한다.
- 예외가 발생하면 해당 예외를 출력한다.
### 클라이언트 구성
RMI 클라이언트에서 원격 객체에 대한 참조를 얻고 메서드를 호출한다.
```java
// MathClient.java
import java.rmi.Naming;

public class MathClient {
    public static void main(String[] args) {
        try {
            RemoteMath remoteMath = (RemoteMath) Naming.lookup("rmi://localhost/MathService");
            int result = remoteMath.add(5, 10);
            System.out.println("Result from server: " + result);
        } catch (Exception e) {
            System.err.println("MathClient exception: " + e.getMessage());
            e.printStackTrace();
        }
    }
}
```
- `MathClient` 클래스는 RMI 클라이언트를 구현한다.
- `Naming.lookup`을 사용하여 서버에 등록된 "MathService"라는 이름의 원격 객체에 대한 참조를 얻다.
- `add` 메서드를 호출하여 서버에서 더한 결과를 받아와 출력한다.
- 예외가 발생하면 해당 예외를 출력한다.