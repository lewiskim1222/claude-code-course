# Claude Code 완전 정복 코스
> 처음 만나는 AI 페어 프로그래밍 — 설치부터 실전까지

---

## 목차

1. [Claude Code란?](#1-claude-code란)
2. [설치 & 첫 실행](#2-설치--첫-실행)
3. [대화하듯 코딩하기 — 기본 사용법](#3-대화하듯-코딩하기--기본-사용법)
4. [파일 읽기 · 쓰기 · 수정](#4-파일-읽기--쓰기--수정)
5. [슬래시 명령어 치트시트](#5-슬래시-명령어-치트시트)
6. [Git 워크플로우 자동화](#6-git-워크플로우-자동화)
7. [실전 패턴 10가지](#7-실전-패턴-10가지)
8. [권한 & 보안 설정](#8-권한--보안-설정)
9. [고급 기능 — MCP · 훅 · 멀티에이전트](#9-고급-기능--mcp--훅--멀티에이전트)
10. [자주 하는 실수 & 해결법](#10-자주-하는-실수--해결법)

---

## 1. Claude Code란?

Claude Code는 **터미널 안에서 동작하는 AI 코딩 어시스턴트**입니다.  
단순한 코드 자동완성이 아니라, 프로젝트 전체를 이해하고 파일을 직접 읽고 수정하며 셸 명령을 실행하는 **자율 에이전트**입니다.

| 일반 AI 채팅 | Claude Code |
|---|---|
| 코드 붙여넣기 → 복붙 반복 | 프로젝트를 직접 탐색 |
| 파일 구조 모름 | `find`, `grep`으로 스스로 파악 |
| 실행 불가 | 터미널 명령 직접 실행 |
| 컨텍스트 날아감 | 대화 이어가며 누적 이해 |

**핵심 철학:** "시키는 것만 하지 않는다. 맥락을 이해하고 판단한다."

---

## 2. 설치 & 첫 실행

### 설치 (Node.js 18+ 필요)

```bash
npm install -g @anthropic-ai/claude-code
```

### API 키 설정

```bash
export ANTHROPIC_API_KEY="sk-ant-..."
# 영구 저장하려면 ~/.zshrc 또는 ~/.bashrc에 추가
```

### 첫 실행

```bash
cd 내-프로젝트-폴더
claude
```

`>` 프롬프트가 뜨면 성공. 이제 한국어로 말해도 됩니다.

### 비대화형 모드 (스크립트에서 사용)

```bash
claude -p "이 파일의 버그를 찾아줘" --output-format json
```

---

## 3. 대화하듯 코딩하기 — 기본 사용법

### 황금 규칙: 컨텍스트를 충분히 줘라

**나쁜 프롬프트:**
```
버그 고쳐줘
```

**좋은 프롬프트:**
```
src/auth/login.ts의 loginUser 함수에서 JWT 토큰 만료 처리가 
안 되고 있어. 401 에러 대신 빈 객체를 반환하는 문제야. 
TypeScript이고 express 서버야.
```

### 멀티라인 입력

`\` 뒤에 Enter를 치면 여러 줄 입력 가능:

```
> 다음 조건으로 API 엔드포인트 만들어줘 \
  - POST /api/users \
  - body: { name, email, password } \
  - bcrypt로 비밀번호 해싱 \
  - JWT 반환
```

### ! 접두사 — 셸 명령 직접 실행

Claude에게 묻지 않고 셸 명령을 직접 실행합니다:

```
! ls -la
! git log --oneline -10
! npm test
```

### 이미지 붙여넣기

Mac에서 `Cmd+V`로 스크린샷을 바로 붙여넣을 수 있습니다:

```
> [스크린샷 붙여넣기]
이 에러 화면을 보고 원인을 찾아줘
```

---

## 4. 파일 읽기 · 쓰기 · 수정

Claude Code는 **허락 없이 파일을 수정하지 않습니다** (기본값).  
수정 전에 항상 확인을 요청합니다.

### @ 멘션으로 파일 지정

```
@src/utils/parser.ts 이 파일에서 메모리 누수 찾아줘
```

### 여러 파일 한번에

```
@package.json @tsconfig.json @.env.example 
프로젝트 구조 파악하고 TypeScript 설정 최적화해줘
```

### 폴더 전체

```
@src/ 전체 코드를 훑어보고 중복된 유틸 함수 목록 뽑아줘
```

### 새 파일 생성

```
> Redis 캐시 클라이언트를 src/lib/cache.ts로 만들어줘.
  connection pool 설정 포함해서.
```

---

## 5. 슬래시 명령어 치트시트

| 명령어 | 설명 |
|---|---|
| `/help` | 도움말 |
| `/clear` | 대화 히스토리 초기화 (컨텍스트 리셋) |
| `/compact` | 대화를 요약해서 컨텍스트 절약 |
| `/cost` | 이번 세션 토큰 사용량 & 비용 확인 |
| `/status` | 현재 설정 상태 확인 |
| `/model` | 사용할 Claude 모델 변경 |
| `/fast` | Fast 모드 토글 (빠른 응답) |
| `/review` | 현재 브랜치 코드 리뷰 |
| `/memory` | 저장된 메모리 보기 |
| `/init` | CLAUDE.md 프로젝트 설정 파일 생성 |
| `/vim` | Vim 키바인딩 모드 |
| `/quit` | 종료 |

### 가장 자주 쓰는 것 TOP 3

```bash
/clear        # 새 주제 시작할 때
/cost         # 비용 체크할 때
/compact      # 대화가 길어졌을 때
```

---

## 6. Git 워크플로우 자동화

Claude Code의 진가가 가장 빛나는 영역입니다.

### 커밋 메시지 자동 생성

```
> 변경사항 커밋해줘
```

Claude가 `git diff`를 읽고 의미 있는 커밋 메시지를 작성합니다.

### PR 생성

```
> GitHub PR 만들어줘. 
  main 브랜치로, 이 기능의 목적과 테스트 방법 포함해서.
```

### 브랜치 전략

```
> feature/user-auth 브랜치 만들고 거기서 작업 시작해줘
```

### 충돌 해결

```
> git merge 충돌 났어. 해결해줘.
  우리 팀 컨벤션은 새 기능 우선이야.
```

### 코드 리뷰 (로컬)

```
/review
```

또는:

```
> main 대비 변경된 파일들 코드 리뷰해줘.
  보안, 성능, 가독성 관점에서.
```

---

## 7. 실전 패턴 10가지

### 패턴 1 — 버그 헌팅

```
> npm test 실행하고 실패하는 테스트 모두 고쳐줘
```

### 패턴 2 — 리팩터링

```
> src/api/ 폴더의 에러 처리가 일관성이 없어.
  express-async-errors 패턴으로 통일해줘.
  파일 하나씩 확인받고 진행해.
```

### 패턴 3 — 문서화

```
> @src/lib/database.ts 
  JSDoc 주석 달아줘. 퍼블릭 메서드만, 파라미터 타입과 예시 포함.
```

### 패턴 4 — 테스트 작성

```
> @src/utils/validator.ts 
  Jest 유닛 테스트 작성해줘. 엣지케이스 포함해서.
  파일은 src/utils/validator.test.ts
```

### 패턴 5 — 의존성 분석

```
> package.json 보고 안 쓰는 패키지 찾아줘.
  실제 import 여부까지 확인해서.
```

### 패턴 6 — 성능 최적화

```
> @src/db/queries.ts 
  N+1 쿼리 문제 있는 곳 찾아서 수정해줘.
  ORM은 Prisma야.
```

### 패턴 7 — 보안 감사

```
> 이 프로젝트에서 SQL Injection, XSS, 하드코딩된 시크릿 
  취약점 스캔해줘.
```

### 패턴 8 — 타입 강화

```
> any 타입을 모두 찾아서 적절한 타입으로 교체해줘.
  한 번에 하나씩, 내가 확인하면서 진행.
```

### 패턴 9 — 환경 설정

```
> Docker Compose로 개발환경 만들어줘.
  Node.js 앱 + PostgreSQL + Redis.
  hot reload 포함.
```

### 패턴 10 — 레거시 마이그레이션

```
> @src/routes/user.js 
  CommonJS를 ESM으로 마이그레이션하고 TypeScript 적용해줘.
  기존 동작은 그대로 유지.
```

---

## 8. 권한 & 보안 설정

Claude Code는 **3가지 권한 모드**로 실행됩니다:

### 기본 모드 (권장)
파일 수정, 셸 명령 실행 시 매번 확인을 요청합니다.

### Auto-approve 모드
```bash
claude --dangerously-skip-permissions
```
⚠️ 주의: 확인 없이 모든 작업 실행. 신뢰하는 작업에만 사용.

### CLAUDE.md — 프로젝트 규칙 설정

프로젝트 루트에 `CLAUDE.md` 파일을 만들면 Claude가 항상 참조합니다:

```markdown
# 프로젝트 규칙

## 기술 스택
- Runtime: Node.js 20, TypeScript 5.x
- Framework: Express 4
- ORM: Prisma + PostgreSQL
- Test: Jest + Supertest

## 코딩 컨벤션
- 함수명: camelCase
- 에러 처리: Result 패턴 사용 (throw 금지)
- 주석: 한국어

## 절대 하지 말것
- console.log를 코드에 남기지 말것
- any 타입 사용 금지
- 직접 DB 접근 (반드시 repository 레이어 경유)

## 자주 쓰는 명령
- 개발 서버: npm run dev
- 테스트: npm test
- 빌드: npm run build
```

`/init` 명령으로 자동 생성도 가능합니다.

---

## 9. 고급 기능 — MCP · 훅 · 멀티에이전트

### MCP (Model Context Protocol) — 외부 도구 연결

Claude Code에 외부 서비스를 연결할 수 있습니다:

```bash
# GitHub MCP 서버 추가
claude mcp add github -- npx @modelcontextprotocol/server-github

# Slack MCP 서버 추가  
claude mcp add slack -- npx @modelcontextprotocol/server-slack
```

연결 후:
```
> GitHub에서 열린 이슈 목록 보여줘
> Slack #dev 채널에 배포 완료 메시지 보내줘
```

### 훅 (Hooks) — 자동화 트리거

파일 수정 후 자동으로 lint 실행하는 훅 예시 (`~/.claude/settings.json`):

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npm run lint -- --fix"
          }
        ]
      }
    ]
  }
}
```

### 서브에이전트 활용

```
> 다음 4가지 작업을 병렬로 처리해줘:
  1. 유닛 테스트 작성
  2. API 문서 업데이트
  3. 타입 에러 수정
  4. README 업데이트
```

Claude Code가 내부적으로 서브에이전트를 생성해 동시에 처리합니다.

---

## 10. 자주 하는 실수 & 해결법

### ❌ 실수 1: 컨텍스트 없이 질문

```
❌ "고쳐줘"
✅ "src/auth/middleware.ts:45 에서 req.user가 undefined일 때 
   401 대신 500 에러가 나. 타입가드 추가해서 고쳐줘."
```

### ❌ 실수 2: 너무 큰 작업 한 번에 요청

```
❌ "전체 프로젝트 리팩터링해줘"
✅ "일단 src/utils/ 폴더만 리팩터링해줘. 
   끝나면 다음 폴더 얘기하자."
```

### ❌ 실수 3: 컨텍스트 한계 무시

대화가 길어지면 초기 내용을 잊습니다. 해결:
```bash
/compact    # 요약하고 계속
/clear      # 새로 시작
```

### ❌ 실수 4: 결과 검증 안 함

Claude가 작성한 코드는 **반드시 테스트**하세요:
```
> 작성한 코드 테스트 실행해줘
> 엣지케이스도 확인해줘
```

### ❌ 실수 5: CLAUDE.md 없이 사용

매번 컨텍스트를 설명하는 대신 `/init`으로 프로젝트 규칙을 파일에 저장하세요.

---

## 빠른 시작 체크리스트

```
□ npm install -g @anthropic-ai/claude-code
□ ANTHROPIC_API_KEY 환경변수 설정
□ 프로젝트 폴더에서 claude 실행
□ /init 으로 CLAUDE.md 생성
□ 첫 작업: "이 프로젝트 구조 파악하고 요약해줘"
□ /cost 로 비용 확인하는 습관
□ /clear 로 새 작업마다 컨텍스트 리셋
```

---

## 비용 최적화 팁

| 상황 | 추천 모델 |
|---|---|
| 간단한 수정, 빠른 답변 | `/model haiku` |
| 일반 코딩 작업 | 기본 (Sonnet) |
| 복잡한 아키텍처, 어려운 버그 | `/model opus` |

프롬프트 캐싱이 자동 적용되어 같은 파일을 반복 참조할 때 비용이 대폭 절감됩니다.

---

*Claude Code 공식 문서: https://docs.anthropic.com/claude-code*  
*문의 & 피드백: https://github.com/anthropics/claude-code/issues*
