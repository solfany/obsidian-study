# CLAUDE.md — study 볼트 운영 규칙

이 볼트는 공부용 지식 베이스이며 Karpathy의 LLM Wiki 패턴을 엄격하게 적용한다.
사람은 원본(raw)을 모으고 트러블슈팅·코딩테스트 기록을 쓰며, 에이전트(Claude)는 위키(wiki)를 작성·유지한다.

## 1. 폴더 구조

| 폴더 | 역할 | 쓰기 권한 |
|---|---|---|
| `00_inbox/` | 아직 분류되지 않은 메모·링크·파일 임시 보관소. ingest 전 대기열 | 사람 (에이전트는 분류 후 이동 제안만) |
| `10_raw/` | 원본 자료 보관소 | 사람만. **에이전트는 읽기 전용** |
| `10_raw/papers/` | 논문 | |
| `10_raw/articles/` | 웹 기사, 블로그 글, 공식 문서 클리핑 | |
| `10_raw/courses/` | 강의·코스 자료, 강의 메모 | |
| `10_raw/books/` | 책 발췌, 독서 메모 | |
| `10_raw/assets/` | 이미지·첨부파일 (Obsidian 첨부 기본 경로) | |
| `20_wiki/` | 에이전트가 작성하는 위키 | 에이전트 |
| `20_wiki/cs/` | 컴퓨터 과학 기초 (OS, 네트워크, DB, 자료구조 등) 개념 | |
| `20_wiki/backend/` | 백엔드 개발 (프레임워크, 서버, 인프라, 아키텍처) 개념 | |
| `20_wiki/ai/` | AI·ML·LLM 개념 | |
| `20_wiki/algorithm/` | 알고리즘·문제 풀이 기법 개념 | |
| `20_wiki/sources/` | 원본 1개당 소스 요약 노트 1개 | |
| `20_wiki/synthesis/` | 여러 출처를 종합한 분석·비교, 보존 가치가 있는 query 답변 | |
| `20_wiki/index.md` | 위키 전체 목차 | 에이전트 |
| `20_wiki/log.md` | 작업 기록 (append-only) | 에이전트 |
| `30_troubleshooting/` | 에러·장애 기록. 증상 / 원인 / 해결 | **사람**. 에이전트는 링크 추가만 (5절) |
| `40_coding-test/` | 코딩테스트 풀이. 문제 1개 = 파일 1개 | **사람**. 에이전트는 링크 추가만 (5절) |
| `80_templates/` | 노트 템플릿 | 사람 (에이전트는 요청 시 수정) |
| `90_archive/` | 더 이상 쓰지 않는 노트 보관 | 사람 |

- 개념 노트가 어느 카테고리에도 맞지 않으면 가장 가까운 곳에 두고 사람에게 새 카테고리 폴더를 제안한다. 폴더를 임의로 만들지 않는다.

## 2. 절대 규칙

1. **`10_raw/`는 절대 수정·이동·삭제·이름 변경하지 않는다. 읽기만 한다.**
2. `20_wiki/`의 모든 주장(사실·수치·인용·해석)에는 `[[10_raw/...]]` 형식의 출처 링크를 단다. **출처 없는 주장은 쓰지 않는다.**
   - 예: `B-Tree 인덱스는 범위 검색에 유리하다 ([[10_raw/books/real-mysql-ch8]])`
   - 에이전트의 추론·종합은 `> [!note] 추론` 콜아웃으로 구분하고 근거가 된 출처를 함께 적는다.
   - `30_troubleshooting/`, `40_coding-test/`의 사람 기록을 근거로 쓸 때도 해당 노트 링크를 출처로 단다. 단, 일반화된 개념 주장은 가능한 한 `10_raw/` 출처로 뒷받침한다.
3. 새 정보가 기존 주장과 충돌하면 **덮어쓰지 않는다.** 기존 주장에 superseded 표시를 하고 새 주장을 추가한다 (6절).
4. **민감정보 보호**: 비밀번호, API 키, 토큰, 접속 정보, 사내 코드 같은 민감정보는 옮기거나 요약하거나 위키에 복사하지 않는다. 발견하면 위치(파일·줄)만 사람에게 알린다. 트러블슈팅 기록에 섞여 있어도 마찬가지다.
5. **사람이 쓴 기존 노트는 수정 전에 반드시 물어본다.** (`00_inbox/`, `30_troubleshooting/`, `40_coding-test/`, 그 밖에 사람이 만든 노트.) 이동·이름 변경·삭제·frontmatter 수정도 포함한다. 에이전트가 새로 만드는 문서(`20_wiki/` 노트, index·log 갱신)는 바로 만들어도 된다.
6. 폴더명·파일명은 영어(kebab-case), 노트 제목과 본문은 한국어로 쓴다. 기술 용어는 필요하면 영어를 괄호로 병기한다 (예: `검색 증강 생성(RAG)`).
7. `20_wiki/` 안에서 링크는 **전체 경로**로 쓴다 (`[[20_wiki/cs/b-tree|B-Tree]]`). 같은 파일명이 여러 폴더에 생겨도 링크가 깨지지 않게 하기 위함이다.

## 3. Frontmatter

모든 노트는 공통 필드 `type`, `title`, `description`, `status`, `tags`, `created`, `updated`를 이 순서로 가진다. 유형별 추가 필드는 그 뒤에 둔다. `20_wiki/` 노트는 `sources`가 추가로 필수다.

```yaml
---
type: source | concept | synthesis | troubleshooting | coding-test
title: 노트 제목 (한국어)
description: 한두 문장 요약
status: unverified | verified          # 20_wiki
        # unsolved | solved            # troubleshooting
        # tried | solved | review      # coding-test
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
sources:            # 20_wiki 필수
  - "[[10_raw/...]]"
---
```

- `20_wiki/`: 에이전트가 작성·수정한 노트는 기본 `unverified`. 사람이 검토한 뒤에만 `verified`로 바꾼다. 에이전트는 `verified`로 바꾸지 않는다. verified 노트를 에이전트가 수정하면 `unverified`로 되돌린다.
- 개념 노트는 `category: cs | backend | ai | algorithm` 필드를 추가로 가진다 (폴더와 일치).
- `created`는 노트를 만든 날. 이후 바꾸지 않는다.
- `updated`는 내용이 바뀔 때마다 오늘 날짜로 갱신한다.

## 4. 작업 절차

### ingest (원본 → 위키)
1. 대상 원본을 `10_raw/`에서 읽는다. `00_inbox/`에 있으면 적절한 `10_raw/` 하위 폴더로 옮기도록 사람에게 먼저 제안한다.
2. 원본을 한 번에 하나씩 처리한다. 핵심 내용을 사람에게 간단히 먼저 공유하고 강조점을 확인할 수 있다.
3. `20_wiki/sources/`에 소스 요약 노트를 만든다 (`80_templates/source.md`).
4. 원본에 등장하는 개념을 찾아 `cs/`, `backend/`, `ai/`, `algorithm/`의 기존 개념 노트를 갱신하거나 새로 만든다 (`80_templates/concept.md`).
5. 관련 노트끼리 `[[ ]]` 링크를 양방향으로 건다. 관련된 `30_troubleshooting/`, `40_coding-test/` 노트가 있으면 개념 노트에 링크를 추가한다 (5절).
6. 기존 주장과 충돌하는 내용이 있으면 6절 규칙을 따른다.
7. `index.md`와 `log.md`를 갱신한다.
8. 무엇을 만들고 바꿨는지 사람에게 요약 보고한다.

### query (질문 → 답변)
1. **에러·버그·장애 관련 질문이면 `30_troubleshooting/`부터 검색한다.** 같거나 비슷한 증상 기록이 있으면 그 기록을 먼저 제시하고, 이어서 관련 `20_wiki/` 개념을 확인한다.
2. 그 외 질문은 `index.md`에서 관련 노트를 찾아 위키 노트를 먼저 읽는다. 필요할 때만 `10_raw/` 원본을 확인한다. 알고리즘 문제 질문이면 `40_coding-test/`의 비슷한 풀이도 확인한다.
3. 출처 링크를 포함해 답한다. 위키에 근거가 없으면 없다고 말하고, 일반 지식으로 답할 때는 그 사실을 밝힌다.
4. 보존 가치가 있는 답변(비교, 분석, 새로 발견한 연결)은 사람에게 확인 후 `20_wiki/synthesis/`에 저장하고 index·log를 갱신한다.

### lint (위키 점검)
다음을 점검하고 결과를 보고한다. 수정은 사람 확인 후 진행한다.
- 출처 링크가 없는 주장, 깨진 `[[ ]]` 링크
- frontmatter 필수 필드 누락·형식 오류, 폴더와 `category` 불일치
- 서로 모순되는 주장 (superseded 처리 누락)
- 어떤 노트에서도 링크되지 않는 고아 노트
- `index.md`에 누락된 노트, 삭제됐는데 남아 있는 항목
- 여러 소스에서 반복 언급되지만 개념 노트가 없는 주제 (새 노트 후보)
- 개념 노트와 연결되지 않은 `30_troubleshooting/`, `40_coding-test/` 노트
- `status: unsolved`로 오래 남은 트러블슈팅, `status: review`인 코테 풀이 (복습 후보)

## 5. 30_troubleshooting · 40_coding-test 규칙

- **사람이 쓴다.** 에이전트는 내용을 대신 쓰거나 고치지 않는다. 사람이 요청하면 템플릿으로 빈 노트를 만들어 줄 수는 있다.
- **30_troubleshooting/**: 파일명 `YYYY-MM-DD-<증상-요약>.md`, 본문은 증상 / 원인 / 해결 (`80_templates/troubleshooting.md`).
- **40_coding-test/**: 문제 1개 = 파일 1개. 파일명 `<platform>-<문제번호>-<짧은-이름>.md` (예: `boj-1260-dfs-bfs.md`) (`80_templates/coding-test.md`).
- **에이전트가 하는 일은 링크 연결이다.**
  - 관련 `20_wiki/` 개념 노트의 "관련 트러블슈팅" / "관련 코테 문제" 섹션에 해당 노트 링크를 추가한다 (위키 쪽이므로 바로 해도 된다).
  - 사람 노트의 "관련 개념" 섹션에 위키 링크를 추가하는 것은 **먼저 물어본 뒤** 한다.
  - 관련 개념 노트가 없으면 새 개념 노트 후보로 제안한다.

## 6. 충돌 처리 (superseded)

기존 주장을 지우지 않고 취소선과 표시를 남긴 뒤 새 주장을 아래에 추가한다. 개념 노트의 "변경 이력"에도 한 줄 남긴다.

```markdown
- ~~A는 B이다 ([[10_raw/articles/old-source]])~~ — superseded (YYYY-MM-DD): [[10_raw/articles/new-source]]에서 반박됨
- A는 C이다 ([[10_raw/articles/new-source]])
```

어느 쪽이 맞는지 판단이 어려우면 두 주장을 모두 남기고 `> [!warning] 충돌` 콜아웃으로 표시한 뒤 사람에게 알린다. 버전 차이(예: 라이브러리 v2와 v3)로 둘 다 맞는 경우에는 superseded 대신 버전 조건을 명시한다.

## 7. index.md / log.md 갱신 규칙

### index.md
- 위키 노트를 만들거나, 이름을 바꾸거나, 보관할 때마다 갱신한다.
- 섹션(CS, Backend, AI, Algorithm, Sources, Synthesis) 아래에 한 줄씩: `- [[20_wiki/ai/xxx|제목]] — description`
- 섹션 안은 가나다/알파벳 순으로 정렬한다.

### log.md
- append-only. 기존 항목은 수정하지 않고 맨 아래에 추가한다.
- 형식:

```markdown
## [YYYY-MM-DD] ingest | 제목
- 생성: [[...]], [[...]]
- 수정: [[...]]
- 메모: 충돌·보류 사항 등
```

- 작업 종류: `ingest`, `query`, `lint`, `update`, `link`, `archive`
- `grep "^## \[" 20_wiki/log.md | tail -5`로 최근 작업을 확인할 수 있도록 접두어를 반드시 지킨다.

## 8. 템플릿

- `80_templates/source.md` — 소스 요약 (에이전트)
- `80_templates/concept.md` — 개념 (에이전트)
- `80_templates/troubleshooting.md` — 트러블슈팅 (사람)
- `80_templates/coding-test.md` — 코테 풀이 (사람)

## 9. Git 커밋 규칙

- 이 볼트는 git 저장소(GitHub `solfany/obsidian-study`)로 관리한다.
- **ingest, lint, 구조 변경 작업이 끝나면 커밋한다.** 메시지 형식:
  - `ingest: 제목`
  - `lint: 요약`
  - `refactor: 요약` (폴더·템플릿·규칙 등 구조 변경)
- 커밋 전에 비밀번호, API 키, 토큰으로 보이는 내용이 스테이징되지 않았는지 확인한다. 발견하면 커밋하지 말고 사람에게 알린다.
- `.gitignore`에 있는 파일(`.obsidian/workspace*.json`, `.obsidian/cache/`, `.trash/`, `graphify-out/`, `.DS_Store`)은 커밋하지 않는다.
