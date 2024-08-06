**1. Obsidian Git: Pull**

- 원격 저장소(예: 깃허브)에서 최신 변경 사항을 가져와 로컬 저장소에 반영합니다. 즉, 다른 사람이 변경한 내용을 내 옵시디언에 업데이트하는 명령입니다.

**2. Obsidian Git: Push**

- 로컬 저장소의 변경 사항을 원격 저장소로 보냅니다. 즉, 내가 수정한 내용을 깃허브에 업로드하여 다른 사람과 공유하는 명령입니다.

**3. Obsidian Git: fetch**

- 원격 저장소의 정보만 가져와 로컬 저장소에 반영합니다. Pull과 비슷하지만, 로컬 저장소에 병합하지는 않습니다. 즉, 변경 사항을 미리 확인하고 싶을 때 사용합니다.

**4. Obsidian Git: Edit remotes**

- 원격 저장소 설정을 편집합니다. 예를 들어, 원격 저장소의 주소를 변경하거나 새로운 원격 저장소를 추가할 수 있습니다.

**5. Obsidian Git: Create backup**

- 현재 상태의 옵시디언 볼트를 백업합니다. 혹시라도 문제가 발생할 경우 복구하기 위해 사용합니다.

**6. Obsidian Git: Commit staged**

- 스테이징된 변경 사항을 커밋합니다. 즉, 변경된 내용을 로컬 저장소에 저장하는 명령입니다.

**7. Obsidian Git: Remove remote**

- 연결된 원격 저장소를 삭제합니다.

**8. Obsidian Git: Switch branch**

- 다른 브랜치로 전환합니다. 브랜치는 작업 내용을 분리하여 관리하는 기능입니다.

**9. Obsidian Git: Delete branch**

- 특정 브랜치를 삭제합니다.

**10. Obsidian Git: Open diff view**

- 두 버전 간의 차이를 시각적으로 비교할 수 있는 화면을 엽니다.

**11. Obsidian Git: Edit .gitignore**

- 깃에 올리지 않을 파일이나 폴더를 지정하는 `.gitignore` 파일을 편집합니다.

**12. Obsidian Git: Open history view**

- 커밋 히스토리를 확인할 수 있는 화면을 엽니다.

**13. Obsidian Git: Create new branch**

- 새로운 브랜치를 생성합니다.

**14. Obsidian Git: Commit all changes**

- 모든 변경 사항을 스테이징하고 커밋합니다.

**15. Obsidian Git: Stage current file**

- 현재 열려 있는 파일을 스테이징합니다.

### 추가 설명

- **스테이징(Staging):** 커밋하기 전에 변경된 파일을 임시 저장하는 과정입니다.
- **커밋(Commit):** 스테이징된 변경 사항을 로컬 저장소에 저장하는 과정입니다.
- **푸시(Push):** 로컬 저장소의 변경 사항을 원격 저장소로 보내는 과정입니다.
- **풀(Pull):** 원격 저장소의 변경 사항을 로컬 저장소로 가져와 병합하는 과정입니다.
- **페치(Fetch):** 원격 저장소의 정보만 가져와 로컬 저장소에 반영하는 과정입니다.
- **브랜치(Branch):** 작업 내용을 분리하여 관리하기 위한 기능입니다.

### 깃허브 관련 명령어

- **Obsidian Git: Open file on GitHub:** 깃허브에서 해당 파일을 바로 열어 확인할 수 있습니다.
- **Obsidian Git: Open file history on GitHub:** 깃허브에서 해당 파일의 변경 이력을 확인할 수 있습니다.
- **Obsidian Git: Clone an existing remote repo:** 이미 존재하는 원격 저장소(예: 깃허브)를 복제하여 로컬에 가져옵니다.

### 파일 및 변경사항 관리 명령어

- **Obsidian Git: Unstage current file:** 현재 작업 중인 파일을 스테이징 해제합니다. (커밋 대상에서 제외)
- **Obsidian Git: Add file to gitignore:** 해당 파일을 `.gitignore` 파일에 추가하여 깃에서 추적하지 않도록 설정합니다.
- **Obsidian Git: Initialize a new repo:** 현재 폴더를 새로운 깃 저장소로 초기화합니다.
- **Obsidian Git: Switch to remote branch:** 원격 저장소의 다른 브랜치로 전환합니다.
- **Obsidian Git: Create backup:** 현재 상태를 백업합니다.
- **Obsidian Git: Create backup with specific message:** 특정 메시지와 함께 백업을 생성합니다.
- **Obsidian Git: Commit staged with specific message:** 스테이징된 변경 사항을 특정 메시지와 함께 커밋합니다.
- **Obsidian Git: Commit all changes with specific message:** 모든 변경 사항을 스테이징하고 특정 메시지와 함께 커밋합니다.
- **Obsidian Git: Toggle line author information:** 각 라인의 작성자 정보를 표시하거나 숨깁니다.
- **Obsidian Git: CAUTION: Discard all changes:** 모든 변경 사항을 버립니다. **주의:** 이 명령은 되돌릴 수 없으므로 신중하게 사용해야 합니다.

### 기타 명령어

- **Obsidian Git: Open source control view:** 깃 관련 정보를 확인할 수 있는 뷰를 엽니다.
- **Obsidian Git: CAUTION: Delete repository:** 현재 저장소를 삭제합니다. **주의:** 이 명령은 되돌릴 수 없으므로 신중하게 사용해야 합니다.
