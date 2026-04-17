---
title: "Mo ALD 특허 분석 도구를 만들었다 — KIPRISPlus API + Streamlit Cloud 배포"
layout: post
date: 2026-03-17 22:00
tag:
- Python
- Streamlit
- KIPRISPlus
- KIPRIS
- Patent
- ALD
- Atomic Layer Deposition
- Mo
- Molybdenum

- SQLite

headerImage: false
image:
description: KIPRISPlus Open API를 활용한 몰리브덴 ALD 특허 수집·분류·분석 Streamlit 대시보드 개발기
category: blog
author: hsj00
externalLink: true
published: true
giscus_comments: true
share: true
---
반도체 공정에서 몰리브덴(Mo) ALD 관련 특허 동향을 파악할 일이 생겼다.
KIPRIS에서 하나씩 검색하면 되긴 하는데, 키워드별로 검색하고 결과를 엑셀에 정리하고 트렌드를 분석하는 과정이 너무 번거로웠다.
"KIPRISPlus가 Open API를 제공하니까, Claude로 자동화 도구를 만들면 되지 않을까?"
세상 진짜 좋아졌다. 아이디어가 프로그램으로 구현되는 세상이라니...

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/images/posts/2026-03-17/home-readme.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    홈 화면 — README가 메인 콘텐츠로 렌더링된다.
</div>

---

## 무엇을 만들었나

`Streamlit` 기반 Mo ALD 특허 분석 대시보드다. KIPRISPlus REST API로 특허를 검색·수집하고, SQLite에 저장한 뒤 통계 분석과 시각화까지 한 곳에서 처리한다.

주요 기능은 6개 페이지로 나뉜다:

* **검색/수집** : MoO₂Cl₂, MoCl₅ 등 Mo 전구체 관련 프리셋 키워드로 검색하거나, 발명명칭·초록·IPC 코드를 직접 조합해서 검색. API 1회 호출로 최대 500건 수집. 필요한 특허만 골라서 IPC/CPC, 출원인, 발명자, 청구항, 법적 상태, 패밀리 특허 등 상세 정보를 추가 수집
* **특허 목록** : 수집된 전체 특허를 연도·키워드·등록 상태로 필터링. CSV 내보내기 지원
* **통계 분석** : 연도별 출원 건수, 상위 출원인 순위, 등록 상태 분포, IPC 코드별 공정 분류, 연도×출원인 히트맵을 Plotly 차트로 시각화
* **인용 네트워크** : 특정 특허의 인용/피인용 관계 수집 및 핵심 특허 파악
* **법적 상태** : 등록/소멸/거절/공개 현황, 상세 법적 이력, 만료 예정(등록일+20년 기준, 3년 이내) 특허 알림
* **특허 상세** : 개별 특허의 전체 정보 조회 (출원인, 발명자, 청구항, 인용문헌, 패밀리 특허 등)

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/images/posts/2026-03-17/home-guide.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    사용 가이드 — 6단계 워크플로우
</div>

---

## 기술적으로 신경 쓴 부분

### 공통 사이드바

[블루링크 대시보드](/Hyundai-Bluelink-Dashboard-with-AI/)를 만들 때도 겪었던 Streamlit 멀티페이지 앱의 고질적인 문제가 있다. 각 페이지가 독립적으로 실행되기 때문에 사이드바가 페이지마다 따로 렌더링된다.

이번에는 `sidebar.py` 모듈을 만들어서 `render_sidebar()` 함수 하나로 모든 페이지에 동일한 사이드바를 적용하도록 신경썼다. README 링크, API Key 입력, DB 현황, API 사용량이 어느 페이지에서든 동일하게 표시된다.

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="/assets/images/posts/2026-03-17/search-collect.png" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    검색/수집 페이지 — 공통 사이드바가 모든 페이지에 적용된다
</div>

### README 홈 렌더링

처음 접속하는 사람이 바로 사용법을 확인할 수 있도록, 홈 페이지(`app.py`)가 `README.md` 파일을 읽어서 `st.markdown()`으로 렌더링한다. README를 수정하면 앱 홈 화면도 자동으로 업데이트되므로 문서와 앱이 항상 동기화된다.

```python
readme_path = Path(__file__).parent / "README.md"
if readme_path.exists():
    st.markdown(readme_path.read_text(encoding="utf-8"))
```

### API 호출 절약 설계

KIPRISPlus 무료 API는 월 1,000건이라 호출을 아껴야 한다. 2단계 수집 구조로 설계했다:

1. **1단계 검색**: API 1회로 최대 500건의 기본 정보(출원번호, 발명명칭, 일자, 등록상태) 수집
2. **2단계 상세**: 관심 있는 특허만 선택해서 상세 정보 수집 (1건당 최대 9회 호출)

사이드바에 월간 API 사용량과 잔여 호출 수를 실시간으로 표시해서, 한도를 초과하지 않도록 했다.

### Streamlit Cloud 배포

public 저장소로 전환하면서 보안을 신경 썼다:

* API Key는 코드에 하드코딩하지 않고, `st.secrets` 또는 사이드바 입력으로만 전달
* `.gitignore`에 `.streamlit/secrets.toml`, `.env`, `data/*.db` 등록
* Git 히스토리에 민감 정보가 없는지 전수 검사
* Streamlit Cloud의 Secrets 기능으로 서버 측 API Key 암호화 저장

Streamlit Cloud는 앱 재시작 시 SQLite 데이터가 초기화되는 제약이 있다. 영구 보관이 필요하면 로컬에서 실행해야 하는데, 데모 용도로는 충분하다.

---

## AI를 어떻게 활용했나

이번에도 **Claude**를 설계부터 구현까지 전반적으로 활용했다.

처음에는 "키워드로 특허 검색해서 DB에 넣는 도구" 정도를 생각했다. Claude와 대화하면서 "통계 차트도 넣으면 어떨까", "특허의 법적 상태 추적도 가능할까", "인용 네트워크를 보면 핵심 특허를 찾을 수 있다" — 이런 식으로 기능이 확장됐다.

공통 사이드바 설계도 블루링크 대시보드에서 얻은 교훈을 반영했다. 이전에는 CSS로 기본 네비게이션을 숨기는 방식이었는데, 이번에는 Streamlit의 기본 네비게이션을 그대로 두고 그 아래에 공통 사이드바를 추가하는 더 깔끔한 구조로 가닥을 잡았다.

보안 검토도 Claude에 맡겼다. public 전환 전에 전체 코드와 Git 히스토리를 스캔해서 하드코딩된 키, 민감 정보 노출, `.gitignore` 누락 항목을 점검했다.

---

## 앞으로

현재는 Mo ALD 특허에 특화되어 있지만, 검색 키워드와 IPC 코드만 바꾸면 다른 기술 분야의 특허 분석에도 쓸 수 있다. `config.py`의 `SEARCH_PRESETS`와 `IPC_PROCESS_MAP`만 수정하면 된다.

Streamlit Cloud에 배포했으니, 관심 있는 사람은 직접 KIPRISPlus API Key를 발급받아서 사용해볼 수 있다.

---
