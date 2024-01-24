JDBC(Java Database Connectivity)는 자바 언어를 사용하여 데이터베이스에 접속하고 관리하기 위한 자바 API(응용 프로그램 프로그래밍 인터페이스)이다. JDBC를 사용하면 자바 애플리케이션에서 다양한 데이터베이스와 통신할 수 있다. JDBC는 데이터베이스 관련 작업을 수행하기 위한 일종의 인터페이스로서, 여러 데이터베이스 제품과 상호 작용할 수 있도록 설계되었다.

JDBC 사용 예시
### JDBC 드라이버 로드
먼저, 데이터베이스에 연결하기 위해 해당 데이터베이스에 대한 JDBC 드라이버를 로드해야 한다. 예를 들어, MySQL 데이터베이스를 사용한다고 가정해 보자.
```java
import java.sql.*;

public class JDBCTest {
    public static void main(String[] args) {
        // JDBC 드라이버 로드
        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
            return;
        }
        
        // 데이터베이스 연결 정보 설정
        String url = "jdbc:mysql://localhost:3306/database";
        String username = "username";
        String password = "password";
        
        // 데이터베이스 연결
        try (Connection connection = DriverManager.getConnection(url, username, password)) {
            System.out.println("Connected to the database!!!!!");
            
            // 여기서부터 데이터베이스 작업 수행
            // 예를 들어, 쿼리 실행, 데이터 읽기/쓰기 등
            
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
}
```

### 데이터베이스 작업 수행
`Connection`을 통해 데이터베이스에 연결한 후, 해당 연결을 사용하여 쿼리를 실행하거나 데이터를 읽거나 쓸 수 있습니다. 아래는 간단한 SELECT 쿼리를 실행하는 예제입니다.
```java
// 위의 코드에 이어서...
try (Connection connection = DriverManager.getConnection(url, username, password)) {
    System.out.println("Connected to the database!!!!");

    // 쿼리 실행을 위한 Statement 생성
    try (Statement statement = connection.createStatement()) {
        // SELECT 쿼리 실행
        String query = "SELECT * FROM your_table";
        try (ResultSet resultSet = statement.executeQuery(query)) {
            // 결과 처리
            while (resultSet.next()) {
                int id = resultSet.getInt("id");
                String name = resultSet.getString("name");
                // 필요한 작업 수행
                System.out.println("ID: " + id + ", Name: " + name);
            }
        }
    }
} catch (SQLException e) {
    e.printStackTrace();
}
```
