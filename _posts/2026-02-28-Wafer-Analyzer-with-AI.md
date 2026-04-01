---
title: "웨이퍼 맵 분석기를 만들었다 — AI와 함께한 공정 데이터 분석 툴"
layout: post
date: 2026-02-28 17:00
tag:
- Python
- Streamlit
- Semiconductor
- Wafer Analyzer
headerImage: false
image:
description: Streamlit 기반 웨이퍼 맵 분석 웹앱 개발기
category: blog
author: hsj00
externalLink: true
published: true
comments: true
share: true
---
업무에서 반도체 공정 데이터를 다룰 일이 많은데, 매번 Excel이나 사내 툴에 의존하는 게 불편했다.
CSV 파일 하나 불러서 웨이퍼 맵 한 번 보려면 손이 많이 갔다. 그래서 직접 만들어보기로 했다.

> GitHub: [hsj00/wafer-analyzer](https://github.com/hsj00/wafer-analyzer)
>
> 배포: [wafer-analyzer.streamlit.app](https://wafer-analyzer.streamlit.app/)

---

## 무엇을 만들었나

`Streamlit`을 기반으로 한 웹 앱이다. CSV 또는 Excel 파일을 업로드하면 브라우저에서 즉시 웨이퍼 맵을 시각화하고 분석할 수 있다. 설치 없이 Streamlit Community Cloud에 배포해두면 URL 하나로 누구나 접근할 수 있다.

![웨이퍼 맵 분석기 스크린샷](/assets/images/posts/2026-02-28/wafer-analyzer.png)

주요 기능은 총 6개 탭으로 구성했다:

- **웨이퍼 맵**: 2D Heatmap, Contour, Line Scan, 3D Surface 시각화 + 통계(Mean/Std/Uniformity 등)
- **다중 파라미터**: 같은 파일 내 여러 컬럼을 나란히 Heatmap으로 비교
- **결함 오버레이**: 별도 결함 CSV를 웨이퍼 맵 위에 겹쳐서 공간적 상관관계 분석
- **GPC 분석**: ALD 공정의 GPC(Growth Per Cycle) 계산 및 반경별 균일도 프로파일
- **보고서 생성**: 통계 + 차트 이미지 + 원본 데이터를 Excel(.xlsx)로 한 번에 내보내기
- **ML 이상 탐지**: PCA + IsolationForest로 여러 웨이퍼 중 이상 패턴 자동 분류

프로젝트 구조는 `app.py`(메인), `core.py`(공통 함수), 그리고 `modules/` 폴더 아래 기능별로
파일을 나눠서 관리하도록 설계했다.

---

## AI를 어떻게 활용했나

코드 작성에는 **Perplexity**와 **Claude** 두 가지 AI를 주로 활용했다.

Perplexity는 프로그램 컨셉을 잡는데 주로 사용했고, Claude는 실제 코드를 짜고 다듬는 데 중심적인 역할을 했다.
단순히 "이 코드 짜줘"가 아니라, 반도체 공정 특성과 맥락을 설명하면서 대화하는 방식으로 진행하려고 노력했다.

Python을 공부하는 입장에서 AI가 짜준 코드를 그냥 복붙하지 않고, 한 줄씩 읽으면서 왜 이렇게 동작하는지 이해하려고 노력했지만 완벽하게 이해하기는 쉽지 않았다.
혼자였다면 몇 달 걸렸을 작업을 훨씬 빠르게 완성할 수 있어서 세상이 정말 하루가 다르게 변한다는 생각이 들었다. 이 과정에서 Python 지식도 조금은 늘었기를 기대해본다.

---

## 마무리

아직 개선할 부분이 많지만, 실제 업무에서 간단한 테스트에 쓸 수 있는 수준의 툴은 나온 것 같아서 만족스럽다.
코드는 GitHub에 올려뒀고, Streamlit Community Cloud에도 배포해두었다.
나중에 기회가 되면 탭별 기능 상세나 ML 이상 탐지 알고리즘에 대한 포스팅도 따로 써야겠다.

---
