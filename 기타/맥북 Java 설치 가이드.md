### 1. **Java 17 다운로드**

- Oracle의 공식 사이트에서 **ARM64 DMG Installer** 파일을 다운로드합니다:  
    [jdk-17_macos-aarch64_bin.dmg](https://download.oracle.com/java/17/latest/jdk-17_macos-aarch64_bin.dmg)

### 2. **Java 17 설치**

1. 다운로드한 DMG 파일을 실행합니다.
2. **Java 설치 마법사**가 열리면, 안내에 따라 설치를 진행합니다. 일반적으로 몇 번의 클릭으로 쉽게 설치할 수 있습니다.

### 3. **터미널에서 Java 버전 확인**

설치가 완료된 후, 터미널을 열고 Java가 제대로 설치되었는지 확인합니다. 다음 명령어를 입력하세요:

`java -version`

**정상 출력 예시**:

`java version "17.x.x" Java(TM) SE Runtime Environment (build 17.x.x) Java HotSpot(TM) 64-Bit Server VM (build 17.x.x, mixed mode)`

### 4. **Java 경로 설정 및 확인**

Java가 설치된 경로는 보통 `/Library/Java/JavaVirtualMachines/` 아래에 저장됩니다. Java가 설치된 정확한 경로를 확인하려면 다음 명령어를 입력하세요:

`/usr/libexec/java_home -V`

이 명령어는 설치된 Java 버전의 경로를 표시해줍니다. 예시 출력은 다음과 같습니다:

`Matching Java Virtual Machines (1): 17.x.x, x86_64:	"Oracle JDK 17"	/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home`

여기서 `/Library/Java/JavaVirtualMachines/jdk-17.jdk/Contents/Home`가 Java 17의 실제 경로입니다.

### 5. **환경 변수 설정 (선택 사항)**

만약 터미널에서 Java 명령어를 사용할 때마다 경로를 설정하고 싶다면, `.zshrc` 파일에 Java 경로를 추가할 수 있습니다.

1. **.zshrc 파일 열기**:
    `nano ~/.zshrc`
    
2. **Java 경로 추가**: 아래 줄을 `.zshrc` 파일에 추가합니다:
    `export JAVA_HOME=$(/usr/libexec/java_home -v17) export PATH=$JAVA_HOME/bin:$PATH`
    
3. **변경 사항 적용**: 터미널에서 다음 명령어를 입력해 변경 사항을 적용합니다:
    `source ~/.zshrc`

### 6. **Java 위치 확인**

설정 후, Java가 설치된 경로를 확인하려면 아래 명령어를 사용하세요:
`echo $JAVA_HOME`

이 명령어는 현재 설정된 Java 17의 경로를 출력해줍니다.

---

### 요약:

1. **Oracle 웹사이트**에서 **ARM64 DMG Installer**를 다운로드 및 설치.
2. 터미널에서 `java -version`으로 설치 확인.
3. `/usr/libexec/java_home -V` 명령어로 Java 경로 확인.
4. **(선택 사항)** `.zshrc` 파일에 Java 경로를 설정하여 터미널에서 바로 Java를 사용할 수 있게 환경 변수 추가.