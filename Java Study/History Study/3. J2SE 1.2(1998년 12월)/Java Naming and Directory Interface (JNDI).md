Java 프로그램이 네트워크 상의 서비스 및 리소스에 대한 정보를 검색하는데 
사용되는 API이다. JNDI는 Java 애플리케이션이 분산 환경에서 다양한 서비스를 찾고 사용할 수 있도록 하는 일종의 디렉터리 서비스의 일부로 작동한다.

예시 코드는 다음과 같다.

## LDAP 
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
- `Hashtable`을 사용하여 LDAP 연결에 필요한 환경 속성을 설정한다.
- `Context.INITIAL_CONTEXT_FACTORY`  JNDI를 초기화하는 데 사용되는 팩토리 클래스(다른 클래스의 객체를 생성하는 클래스)이다.
- `Constext.PROVIDER_URL` LDAP 서버의 URL
- `Constext.SECURITY_AUTHENTICATION` 인증 방법 (여기서는 "simple" 즉, 기본 인증 사용).
- `SECURITY_PRINCIPAL`LDAP 서버에 로그인하는 데 사용되는 사용자 이름.
- `SECURITY_CREDENTIALS`사용자 비밀번호.
- `DirContext context = new InitialContext(env);`LDAP 서버에 연결.
- `InitialContext`를 사용하여 LDAP 서버에 연결.