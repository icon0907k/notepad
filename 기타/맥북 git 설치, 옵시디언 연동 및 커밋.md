### 1. Homebrew로 Git 설치

Homebrew는 macOS에서 패키지 관리를 쉽게 해주는 툴입니다.

1. **Homebrew 설치** (이미 설치되어 있다면 건너뛰세요): 터미널을 열고 아래 명령어를 입력합니다.
    
    `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
    
2. **Git 설치**: Homebrew가 설치되었으면 터미널에 아래 명령어를 입력합니다.
    
    `brew install git`
    
3. **Git 버전 확인**: 설치가 완료되면 Git이 제대로 설치되었는지 확인하기 위해 다음 명령어를 실행합니다.
    
    `git --version`

### 2. 터미널 열기

- 먼저 **터미널**을 엽니다.

### 3. 원하는 폴더로 이동

- 프로젝트를 가져오고 싶은 폴더로 이동합니다. 예를 들어, `Documents` 폴더로 이동하고 싶다면:
    
    `cd ~/Documents`
    `git clone <GitHub 프로젝트의 URL>`


### 4. github에 변경 파일  올려보기 

- 자격 증명이 등록이 안되어 있는 경우 아래를 실행합니다. 
- GitHub에서는 비밀번호 대신 **Personal Access Token**(PAT)을 사용해야 합니다.

1. **PAT 만들기**
    
    - GitHub에 로그인합니다.
    - 오른쪽 상단의 프로필 아이콘 클릭 > **Settings** 선택.
    - 왼쪽 메뉴에서 **Developer settings** 클릭.
    - **Personal access tokens** 클릭 > **Generate new token** 클릭.
    - 필요한 권한을 선택하고, **Generate token** 클릭.
    - 생성된 토큰을 복사합니다.
2. **PAT을 사용하여 인증하기**
    
    - Git 명령어를 사용할 때, 사용자 이름에는 GitHub 사용자 이름을 입력하고, 비밀번호 자리에 복사한 **PAT**을 입력합니다.
    
    예를 들어, `git push`를 사용할 때:
    
    - 사용자 이름: `your-username`
    - 비밀번호: **PAT**

옵시디언 열기 -> Commit all changes 명령어 실행 -> Git: Push 명령어 실행 -> 완료