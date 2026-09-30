---
type: note
title: 홈
description: study 볼트 대시보드
status: active
tags: [home]
created: 2026-09-30
updated: 2026-09-30
cssclasses: [home-wide]
---

# 홈

> [!tip] 메모는 Cmd+N(또는 달력 아이콘)으로 `00_inbox/`에 아무렇게나 적고, 정리는 AI에게 "인박스 정리해줘"
> 자료는 `10_raw/`에 넣고 "ingest해줘" · 궁금한 건 그냥 질문 · [[20_wiki/index|위키 목차]] · [[20_wiki/log|작업 기록]]

## 인박스

```base
filters:
  and:
    - file.inFolder("00_inbox")
    - file.ext == "md"
views:
  - type: table
    name: 정리 대기
    order:
      - file.name
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
```

## 최근 원본

`10_raw/`에 새로 넣은 자료. 아직 ingest하지 않았다면 AI에게 "ingest해줘".

```base
filters:
  and:
    - file.inFolder("10_raw")
    - '!file.inFolder("10_raw/assets")'
views:
  - type: table
    name: 최근 원본
    order:
      - file.name
      - file.folder
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
    limit: 10
```

## 검토 대기 위키

AI가 쓴 위키 노트(`unverified`). 읽어 보고 맞으면 `status`를 `verified`로 바꾼다.

```base
filters:
  and:
    - file.inFolder("20_wiki")
    - 'status == "unverified"'
views:
  - type: table
    name: 검토 대기
    order:
      - title
      - type
      - category
      - updated
    sort:
      - property: updated
        direction: DESC
```

## 트러블슈팅 · 코테 복습

```base
filters:
  or:
    - and:
        - file.inFolder("30_troubleshooting")
        - 'status == "unsolved"'
    - and:
        - file.inFolder("40_coding-test")
        - 'status == "review"'
views:
  - type: table
    name: 미해결 · 복습
    groupBy:
      property: type
      direction: ASC
    order:
      - file.name
      - description
      - created
    sort:
      - property: created
        direction: DESC
```

## 최근 위키

```base
filters:
  and:
    - file.inFolder("20_wiki")
    - '!type.isEmpty()'
views:
  - type: table
    name: 최근 위키
    order:
      - title
      - type
      - status
      - updated
    sort:
      - property: updated
        direction: DESC
    limit: 10
```
