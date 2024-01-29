J2SE 1.4의 NIO(Non-blocking Input/Output)는 Java의 입출력(IO) 처리를 개선하기 위한 패키지이다. 기존의 입출력은 스트림(Streams) 기반이었지만, NIO는 채널(Channel)과 버퍼(Buffer)를 중심으로 하는 비동기 입출력 모델을 제공한다. 이를 통해 입출력 작업을 보다 효율적으로 처리할 수 있게 된다.

### NIO의 핵심 구성 요소

1. **채널(Channel)**: NIO에서 데이터가 흐르는 통로입니다. 파일, 네트워크 소켓 등 다양한 종류의 채널이 있습니다.
    
2. **버퍼(Buffer)**: 데이터를 읽거나 쓰는데 사용되는 메모리 영역입니다. 입출력 작업은 버퍼를 통해 이루어집니다.
    
3. **셀렉터(Selector)**: 비동기 이벤트 처리를 담당하는 객체로, 여러 채널의 상태를 모니터링하고 이벤트가 발생한 채널들을 선택합니다.
    

### NIO 예시 코드

다음은 간단한 NIO 예시 코드입니다. 이 코드는 파일을 읽어 들이고 콘솔에 출력하는 간단한 예시입니다.

```java
import java.io.RandomAccessFile;
import java.nio.ByteBuffer;
import java.nio.channels.FileChannel;

public class NIOExample {
    public static void main(String[] args) {
        try {
            RandomAccessFile file = new RandomAccessFile("test.txt", "r");
            FileChannel channel = file.getChannel();
            ByteBuffer buffer = ByteBuffer.allocate(1024);

            int bytesRead = channel.read(buffer);
            while (bytesRead != -1) {
                buffer.flip(); // 버퍼를 읽기 모드로 전환
                while (buffer.hasRemaining()) {
                    System.out.print((char) buffer.get()); // 버퍼에서 데이터 읽기
                }
                buffer.clear(); // 버퍼를 쓰기 모드로 전환
                bytesRead = channel.read(buffer);
            }
            file.close();
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```
이 예시 코드에서는 `FileChannel`을 사용하여 파일을 읽고, `ByteBuffer`를 사용하여 데이터를 버퍼에 저장합니다. `flip()` 메서드는 읽기 모드로 전환하고, `clear()` 메서드는 쓰기 모드로 버퍼를 초기화합니다. `channel.read(buffer)`는 파일에서 데이터를 읽어들이고 버퍼에 저장합니다.

NIO를 사용하면 입출력 작업이 블로킹되지 않으므로, 하나의 스레드가 여러 채널을 관리하고 데이터를 읽고 쓸 수 있습니다. 이것은 많은 연결을 다루는 서버 애플리케이션 등에서 특히 유용합니다.

그러나 NIO는 사용하기 복잡하고 실수하기 쉽습니다. 따라서 Java 7에서는 NIO.2 패키지가 추가되었으며, 보다 간편한 파일 및 디렉터리 작업을 지원합니다. 요즘에는 대부분의 경우 NIO.2를 사용하는 것이 권장됩니다.