Java 프로그램이 네트워크 상의 서비스 및 리소스에 대한 정보를 검색하는데 
사용되는 API이다. JNDI는 Java 애플리케이션이 분산 환경에서 다양한 서비스를 찾고 사용할 수 있도록 하는 일종의 디렉터리 서비스의 일부로 작동한다.

예시 코드는 다음과 같다.

## LDAP server
LDAP는 계층적 디렉터리 서비스로 주로 사용자 관리 및 인증, 권한 관리, 네트워크 접근 제어, 보안 등을 저장하는데 활용된다. LDAP은 다양한 분야에서 중요한 역할을 담당하고 있으며, 앞으로도 그 중요성이 지속될 것으로 예상된다. 아래는 LDAP 서버에 연결하고 몇 가지 기본적인 작업을 수행하는 Java 예시 코드이다.

```java
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.naming.directory.DirContext;
import javax.naming.directory.SearchControls;
import javax.naming.directory.SearchResult;
import java.util.Hashtable;

public class LDAPExample {
    public static void main(String[] args) {
        // LDAP 연결 설정
        Hashtable<String, String> env = new Hashtable<>();
        env.put(Context.INITIAL_CONTEXT_FACTORY, "com.sun.jndi.ldap.LdapCtxFactory");
        env.put(Context.PROVIDER_URL, "ldap://ldap-server-url");
        env.put(Context.SECURITY_AUTHENTICATION, "simple");
        env.put(Context.SECURITY_PRINCIPAL, "ldap-username");
        env.put(Context.SECURITY_CREDENTIALS, "ldap-password");
        try {
            // LDAP 서버에 연결
            DirContext context = new InitialContext(env);
            
            // LDAP 검색 수행
            SearchControls controls = new SearchControls();
            controls.setSearchScope(SearchControls.SUBTREE_SCOPE);
            
            String searchFilter = "(objectClass=user)";
            String[] returningAttributes = {"cn", "mail"};
            
            NamingEnumeration<SearchResult> results = context.search("ou=users,dc=example,dc=com", searchFilter, returningAttributes, controls);
            
            // 결과 출력
            while (results.hasMore()) {
                SearchResult result = results.next();
                System.out.println("Common Name: " + result.getAttribute("cn"));
                System.out.println("Email: " + result.getAttribute("mail"));
            }
            
            // 연결 닫기
            context.close();
        } catch (NamingException e) {
            e.printStackTrace();
        }
    }
}
```

위 소스 코드 설명 
- `Hashtable`을 사용하여 LDAP 연결에 필요한 환경 속성을 설정한다
- `Context.INITIAL_CONTEXT_FACTORY`  JNDI를 초기화하는 데 사용되는 팩토리 클래스(다른 클래스의 객체를 생성하는 클래스)이다
- `Constext.PROVIDER_URL` LDAP 서버의 URL 입력
- `Constext.SECURITY_AUTHENTICATION` 인증 방법 (여기서는 "simple" 즉, 기본 인증 사용)
- `SECURITY_PRINCIPAL`LDAP 서버에 로그인하는 데 사용되는 사용자 이름 입력
- `SECURITY_CREDENTIALS`사용자 비밀번호 입력
- LDAP 서버에 연결
```java
DirContext context = new InitialContext(env);
```
- `InitialContext`를 사용하여 LDAP 서버에 연결. (  InitialContext 는 자바 Naming and Directory Interface (JNDI) 에서 제공하는 클래스로, 디렉토리 서비스에 연결하고 객체를 검색하는 데 사용)
- `DirContext`는 디렉터리 서비스에 대한 컨텍스트를 나타낸다
- LDAP 검색 수행
```java
SearchControls controls = new SearchControls();
controls.setSearchScope(SearchControls.SUBTREE_SCOPE);

String searchFilter = "(objectClass=user)";
String[] returningAttributes = {"cn", "mail"};

NamingEnumeration<SearchResult> results = context.search("ou=users,dc=example,dc=com", searchFilter, returningAttributes, controls);
```
- `SearchControls`를 사용하여 검색의 범위와 속성을 설정한다
- `searchFilter`검색 조건을 나타내는 LDAP 필터
- `returningAttributes` 검색 결과로 반환할 속성 배열
- `context.search()`지정된 범위와 검색 조건으로 LDAP 검색을 수행한다
- 결과 출력
```java
while (results.hasMore()) {
    SearchResult result = results.next();
    System.out.println("Common Name: " + result.getAttribute("cn"));
    System.out.println("Email: " + result.getAttribute("mail"));
}
```
- 검색 결과를 반복하면서 각 사용자의 "Common Name"과 "Email" 속성을 출력
- 연결 닫기
```java
context.close();
```
- 리소스 누수가 발생하므로 LDAP 서버와의 연결은 닫아야 한다.


## DB server
DB 서버에 접속할 때 JNDI를 사용하는 이유는 주로 DB 접속 정보를 코드에 직접 저장하지 않고, WAS(웹 어플리케이션 서버) 서버에 DB 접속 정보를 중앙화하여 저장하고 필요할 때 가져다 사용하기 위함이다. 특히, 대규모 프로젝트에서는 여러 서버 환경에서 운영되는 경우가 많아서, 각각의 서버에 따라 DB 서버가 다르거나 개발 서버, 운영 서버 등이 분리되는 경우가 있다.

JNDI를 사용하면 각 서버에서 DB에 접속할 때 필요한 정보를 WAS 서버에 저장해두고, 애플리케이션은 JNDI를 통해 해당 정보를 참조하여 DB에 접속한다. 이렇게 하면 DB 접속 정보가 애플리케이션 코드에 직접 포함되지 않아서 유지보수가 용이해지며, 서버 간의 이동이나 변경 시에도 코드를 수정하지 않고 설정만 변경하여 간편하게 적용할 수 있다.

소규모 프로젝트에서는 하나의 DB 서버만을 사용하는 경우가 일반적이지만, 대규모 프로젝트에서는 여러 서버 간의 환경이 다양하게 구성되기 때문에 JNDI를 통한 중앙화된 DB 접속 정보 관리가 더욱 필요해진다. 

아래는 JNDI 통해 JDBC 데이터베이스 연결을 사용하여 MySQL 데이터베이스에 연걸하는 예시이다.

**JNDI 설정 파일 (context.xml)**
```XML
<?xml version="1.0" encoding="UTF-8"?>
<Context>
    <Resource name="jdbc/myDB" auth="Container" type="javax.sql.DataSource"
        maxTotal="100" maxIdle="30" maxWaitMillis="10000"
        username="your_username" password="your_password"
        driverClassName="com.mysql.cj.jdbc.Driver"
        url="jdbc:mysql://localhost:3306/your_database"/>
</Context>

```

**Java 코드**
```java 
import javax.naming.Context;
import javax.naming.InitialContext;
import javax.naming.NamingException;
import javax.sql.DataSource;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;

public class JNDIDemo {
    public static void main(String[] args) {
        try {
            // JNDI 초기화
            Context initialContext = new InitialContext();
            Context environmentContext = (Context) initialContext.lookup("java:/comp/env");
            
            // DataSource 검색
            DataSource dataSource = (DataSource) environmentContext.lookup("jdbc/myDB");
            
            // Connection 얻기
            Connection connection = dataSource.getConnection();
            
            // SQL 쿼리 실행
            Statement statement = connection.createStatement();
            ResultSet resultSet = statement.executeQuery("SELECT * FROM your_table");
            
            // 결과 출력
            while (resultSet.next()) {
                System.out.println("Column 1: " + resultSet.getString(1));
                System.out.println("Column 2: " + resultSet.getString(2));
                // 추가적인 필요에 따라 결과 처리
            }
            
            // 리소스 해제
            resultSet.close();
            statement.close();
            connection.close();
        } catch (NamingException | SQLException e) {
            e.printStackTrace();
        }
    }
}
```

위 소스 코드 설명
- `InitialContext`를 사용하여 JNDI 컨텍스트를 초기화
- `java:/comp/env`를 통해 환경 컨텍스트를 얻어온다
- `jdbc/myDB`를 사용하여 등록된 DataSource를 찾아온다.
- 얻어온 DataSource를 사용하여 Connection 객체를 획득한다.
- Connection으로부터 Statement를 얻어온다.
- `executeQuery` 메서드를 사용하여 SQL 쿼리를 실행하고 ResultSet을 얻어온다.
- ResultSet을 반복하여 각 행의 데이터를 출력한다.
- 사용이 끝난 ResultSet, Statement, Connection 등의 리소스를 안전하게 해제한다.
- `NamingException`과 `SQLException`에 대한 예외 처리를 수행한다.