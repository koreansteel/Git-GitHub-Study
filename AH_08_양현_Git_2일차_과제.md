# Git & GitHub 학습 정리

## 1. 버전 관리의 필요성

### 버전 관리(Version Control)
파일이나 코드의 변경 이력을 체계적으로 기록하고 관리하는 것이다. 언제, 누가, 무엇을 바꿨는지 확인할 수 있고 필요하면 과거 상태로 되돌릴 수 있다.

### 버전 관리가 필요한 이유
- 변경 이력을 추적할 수 있다.
- 실수를 복구하고 이전 정상 버전으로 돌아갈 수 있다.
- 여러 사람이 함께 작업할 때 변경 내용을 확인하고 충돌을 해결할 수 있다.
- `final_v2.py`, `진짜최종.py`처럼 파일 복사본이 늘어나는 문제를 줄일 수 있다.

### Git과 GitHub의 차이
- **Git**: 버전 관리를 위한 도구(프로그램). 내 컴퓨터에서 동작한다.
- **GitHub**: Git 저장소를 올려두는 웹 서비스로, 코드 공유와 협업을 지원한다. Pull Request, Issue, Review 기능 등을 제공한다.

> Git은 버전 관리 엔진, GitHub는 협업과 공유를 위한 서비스이다.

## 2. Git 기본 개념

### Repository(저장소)
파일과 변경 이력을 함께 관리하는 저장소로, 프로젝트 전체와 버전 기록을 포함한다.
- **Local Repository**: 내 컴퓨터에 있는 저장소. `git init`으로 초기화할 수 있다.
- **Remote Repository**: GitHub 등 서버에 있는 저장소. `git clone`으로 로컬에 복사할 수 있다.

### Commit(커밋)
변경 사항을 하나의 기록으로 저장하는 단위이며, 프로젝트의 특정 시점 상태를 기록하고 되돌아갈 기준점이 된다.
```bash
git commit -m "회원가입 기능 추가"
```

### Branch(브랜치)
기존 코드에 영향을 주지 않고 독립적으로 작업할 수 있는 작업 흐름이다. 기능 개발이나 실험 후 병합할 수 있다.
```bash
git branch feature/login
git switch feature/login
```

### HEAD
현재 작업 중인 위치를 가리키는 포인터이다. 보통 현재 브랜치의 최신 커밋을 가리킨다. HEAD가 움직이면 작업 기준이 바뀐다.

### Working Directory와 Staging Area
- **Working Directory**: 실제 파일을 수정하고 작업하는 공간. 아직 Git에 기록되지 않은 변경 사항이 있을 수 있다.
- **Staging Area**: 다음 커밋에 포함할 변경 사항을 미리 모아두는 공간.

## 3. Git 설치 및 초기 설정

### 설치
- macOS: `brew install git`
- Windows: Git for Windows 설치
- 설치 확인:
```bash
git --version
```

### GitHub 가입
GitHub 웹사이트에서 회원가입 후 계정을 생성한다.

### 사용자 정보 설정
전역 설정은 모든 저장소에 적용된다.
```bash
git config --global user.name "홍길동"
git config --global user.email "hong@example.com"
```

특정 저장소에만 적용하는 로컬 설정:
```bash
git config user.name "홍길동"
git config user.email "hong@example.com"
```

## 4. 로컬에서 Git 사용하기

### 저장소 초기화
현재 폴더를 Git 저장소로 초기화한다. `.git` 폴더가 생성된다.
```bash
git init
```

### 상태 확인
수정된 파일, 스테이징 여부, 커밋 가능한 변경 사항 등을 확인한다.
```bash
git status
```

### Staging
커밋에 포함할 변경 사항을 선택한다. 아직 커밋이 생성된 것은 아니다.
```bash
git add 파일명
git add .
```

### Commit
Staging Area에 있는 변경 사항을 버전으로 기록한다.
```bash
git commit -m "커밋 메시지"
```

### 커밋 메시지 작성 규칙
- 명확하고 간결하게 작성한다.
- 동사로 시작하는 메시지를 권장한다.
- 하나의 커밋에는 하나의 작업을 담는다.

예시:
- `Add login API`
- `Fix password validation bug`
- `Refactor user service`

### .gitignore
Git 버전 관리에서 제외할 파일을 지정하는 설정 파일이다. 환경 변수, 빌드 결과물, 개인 설정 파일 등을 제외할 때 사용한다.
```gitignore
.env
__pycache__/
*.log
```
이미 커밋된 파일은 Git이 계속 추적할 수 있으므로, 추적을 해제하려면 다음 명령을 사용할 수 있다.
```bash
git rm --cached 파일명
```

## 5. Branch와 병합(Merge)

### 브랜치 주요 명령어
```bash
git branch                 # 브랜치 목록 확인
git branch feature/ui      # 브랜치 생성
git switch feature/ui      # 브랜치 전환
```

### 브랜치 관리 전략 예시
- `main`: 배포 가능한 안정 버전
- `dev`: 개발 중인 기능을 테스트하는 버전
- `feature/*`: 기능 개발용 브랜치
- `hotfix/*`: 긴급 수정용 브랜치

### Merge
다른 브랜치의 작업 내용을 현재 브랜치로 합치는 명령이다.
```bash
git merge feature/ui
```

### Fast-forward와 3-way merge
- **Fast-forward merge**: 브랜치가 직선으로 이어질 때 커밋 포인터만 이동한다.
- **3-way merge**: 브랜치가 갈라졌다가 합쳐질 때 병합 커밋이 생성된다.

### Merge Conflict(병합 충돌)
같은 파일의 같은 부분을 서로 다르게 수정하면 충돌이 발생할 수 있다.
해결 과정:
1. 충돌이 발생한 파일을 확인한다.
2. 충돌 내용을 직접 수정한다.
3. 수정 파일을 스테이징한다.
4. 해결 내용을 커밋한다.

## 6. GitHub 기초

### 원격 저장소 연결
GitHub에서 Repository를 생성하고 로컬 저장소와 연결한다.
```bash
git remote add origin 원격주소
git remote -v
```

### Push와 Pull
- `git push`: 로컬 커밋을 원격 저장소에 올린다.
- `git pull`: 원격 변경 사항을 내려받아 로컬에 반영한다.
```bash
git push -u origin main
git pull
```

## 7. 협업 기능

### Clone과 Fork
- **Clone**: 원격 저장소를 로컬로 복사한다. 저장소에 push 권한이 있는 경우 팀 협업에 주로 사용한다.
- **Fork**: 다른 사람의 저장소를 내 GitHub 계정으로 복사한다. 원본 저장소에 직접 push 권한이 없을 때 오픈소스 기여 등에 활용한다.

### Pull Request(PR)
변경 내용을 원본 저장소에 반영해 달라고 요청하는 것이다. 변경 내용을 설명하고 리뷰를 요청하며 병합 전에 검증하는 과정이다.

### Code Review
코드를 병합하기 전에 함께 확인하는 과정이다.
- 버그 예방
- 코드 품질 유지
- 팀 규칙 공유
- 코드 중심으로 리뷰하고 이유를 남긴다.
- 승인 후 병합한다.

### Issue
할 일, 버그 리포트, 기능 요청 등을 관리한다. PR과 Issue를 연결해 작업을 관리할 수 있다.
예: `Resolve: #1234`

## 8. 자주 사용하는 Git 명령어

| 명령어 | 설명 |
|---|---|
| `git show` | 커밋의 변경 내용, 메시지, 작성자, 날짜 등을 확인 |
| `git show HEAD` | 현재 브랜치의 최신 커밋 확인 |
| `git show 커밋해시` | 특정 커밋 확인 |
| `git diff` | Working Directory와 Staging Area의 차이 확인 |
| `git diff --staged` | Staging Area와 HEAD의 차이 확인 |
| `git diff HEAD~1 HEAD` | 커밋 간 차이 확인 |
| `git blame main.py` | 파일 각 줄의 수정자, 시점, 커밋을 추적 |
| `git stash` | 작업 중 변경 사항을 임시 저장 |
| `git stash list` | stash 목록 확인 |
| `git stash pop` | 최근 stash를 적용하고 목록에서 제거 |
| `git stash apply` | stash를 적용하되 목록에서 제거하지 않음 |
| `git reset --soft HEAD~1` | HEAD를 한 커밋 이전으로 이동, 변경 사항 유지 |
| `git reset` | 기본 mixed 방식으로 커밋 및 스테이징을 취소하고 변경 파일은 유지 |
| `git reset --hard HEAD~1` | HEAD, 스테이징, 파일 변경까지 되돌림(주의 필요) |
| `git revert 커밋해시` | 기존 기록을 보존하면서 되돌리는 새 커밋 생성 |
| `git commit --amend` | 마지막 커밋을 수정/교체 |
| `git commit --amend --no-edit` | 마지막 커밋을 수정하되 메시지는 유지 |

## 9. 오늘 배운 내용 핵심 정리

- Git은 코드 변경 이력을 관리하는 버전 관리 도구이고 GitHub는 Git 저장소를 공유하고 협업하는 웹 서비스이다.
- `add`로 커밋할 변경 사항을 Staging Area에 올리고, `commit`으로 버전 기록을 남긴다.
- Branch를 이용하면 기존 작업과 분리해 기능을 개발할 수 있으며, Merge로 작업을 합칠 수 있다.
- GitHub의 `push`와 `pull`을 통해 로컬과 원격 저장소를 동기화한다.
- Clone, Fork, Pull Request, Code Review, Issue를 활용해 협업할 수 있다.
- `reset`은 히스토리를 바꿀 수 있어 주의가 필요하고, `revert`는 기존 기록을 보존하며 취소 커밋을 만든다.

---

## 과제 제출 체크리스트

- [ ] 학습 내용을 `.md` 파일로 정리
- [ ] GitHub에 **Public Repository** 생성
- [ ] Markdown 파일을 Repository에 Push
- [ ] Repository URL 복사
- [ ] 오즈코딩 LMS 과제 제출란에 Repository 링크 첨부
- [ ] 노션 링크도 함께 제출
