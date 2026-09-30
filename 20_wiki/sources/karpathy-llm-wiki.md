---
type: source
title: "LLM Wiki (Karpathy gist)"
description: LLM이 원본과 사람 사이에서 지속적으로 쌓이는 마크다운 위키를 직접 작성·유지하게 하는 개인 지식 베이스 패턴을 설명한 아이디어 문서.
status: unverified
tags: [llm, knowledge-base, obsidian, rag]
created: 2026-09-30
updated: 2026-09-30
source_type: article
sources:
  - "[[10_raw/articles/2026-09-30 llm-wiki]]"
---

# LLM Wiki (Karpathy gist)

## 기본 정보
- 원본: [[10_raw/articles/2026-09-30 llm-wiki]]
- 저자/출처: Andrej Karpathy의 GitHub Gist (https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) ([[10_raw/articles/2026-09-30 llm-wiki]])
- 날짜: 2026-09-30 클리핑 (원문 게시일 미기재) ([[10_raw/articles/2026-09-30 llm-wiki]])

## 한 줄 요약
질문할 때마다 원본을 다시 검색하는 RAG 대신, LLM이 원본을 읽어 서로 링크된 위키로 한 번 "컴파일"하고 계속 최신으로 유지하게 하자는 패턴이다 ([[10_raw/articles/2026-09-30 llm-wiki]]).

## 핵심 내용
- 이 문서는 LLM 에이전트(Claude Code, Codex 등)에 붙여 넣어 쓰도록 만든 아이디어 파일이며, 구체적 구현은 에이전트와 함께 정하라고 한다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 일반적인 RAG는 질문할 때마다 관련 조각을 다시 찾아 조합하므로 지식이 누적되지 않는다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- LLM Wiki에서는 새 소스가 들어올 때 LLM이 이를 읽고 기존 위키에 통합한다. 엔티티 페이지 갱신, 주제 요약 수정, 기존 주장과의 모순 표시까지 포함한다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 위키는 "지속적이고 복리로 쌓이는 산출물"이며, 사람은 소스 수집·탐색·질문을 맡고 LLM은 요약·교차 참조·정리 같은 잡무를 전부 맡는다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 비유: Obsidian은 IDE, LLM은 프로그래머, 위키는 코드베이스 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 3계층 구조: 불변의 원본(raw sources), LLM이 소유하는 위키, 구조·규칙·절차를 정의하는 스키마 문서(CLAUDE.md, AGENTS.md 등) ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 3가지 작업: ingest(소스 1개가 위키 페이지 10~15개를 건드릴 수 있음), query(좋은 답변은 위키에 다시 저장), lint(모순·낡은 주장·고아 페이지·누락된 교차 참조 점검) ([[10_raw/articles/2026-09-30 llm-wiki]]).
- `index.md`는 내용 중심 목차, `log.md`는 시간순 append-only 기록이다. `## [날짜] ingest | 제목` 같은 일정한 접두어를 쓰면 `grep`으로 파싱할 수 있다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 중간 규모(소스 ~100개, 페이지 수백 개)에서는 index 파일만으로도 임베딩 기반 RAG 인프라 없이 잘 동작한다고 한다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 팁: Obsidian Web Clipper로 원본 수집, 첨부 폴더를 고정해 이미지를 로컬에 다운로드, 그래프 뷰로 구조 확인, Marp·Dataview 활용, 위키를 git 저장소로 관리 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- 효과가 있는 이유: 지식 베이스 유지의 어려운 부분은 읽기·생각이 아니라 장부 정리이며, LLM은 지루해하지 않고 한 번에 여러 파일을 고칠 수 있어 유지 비용이 거의 0에 가깝다 ([[10_raw/articles/2026-09-30 llm-wiki]]).
- Vannevar Bush의 Memex(1945)와 정신적으로 닮았고, Memex가 풀지 못한 "누가 유지보수하는가"를 LLM이 해결한다고 본다 ([[10_raw/articles/2026-09-30 llm-wiki]]).

## 주요 개념
- [[20_wiki/ai/llm-wiki|LLM Wiki 패턴]]
- [[20_wiki/ai/rag|검색 증강 생성(RAG)]]

## 인상적인 인용
> "Obsidian is the IDE; the LLM is the programmer; the wiki is the codebase." ([[10_raw/articles/2026-09-30 llm-wiki]])

> "The wiki stays maintained because the cost of maintenance is near zero." ([[10_raw/articles/2026-09-30 llm-wiki]])

## 기존 위키와의 관계
- 보강: 없음 (첫 ingest)
- 충돌: 없음

## 열린 질문
- 위키 규모가 커지면 index 파일 대신 어떤 검색 도구(예: 원문에서 언급한 qmd)가 필요해지는 시점은 언제인가? ([[10_raw/articles/2026-09-30 llm-wiki]])
- 이미지가 포함된 원본을 LLM이 읽을 때 텍스트 먼저, 이미지는 따로 보는 우회 방식을 이 볼트에서 어떻게 운영할까? ([[10_raw/articles/2026-09-30 llm-wiki]])
