---
title: "ALD 공정 계산기를 만들었다 — Gordon 모델 기반 Precursor 노출량 예측 툴"
layout: post
date: 2026-03-09 22:00
tag:
- Python
- Streamlit
- Semiconductor
- ALD
- Atomic Layer Deposition
- CVD
- Chemical Vapor Deposition
headerImage: false
image:
description: Streamlit 기반 ALD Gordon Model 계산기 개발기 — 고종횡비 구조에서의 conformal 코팅 조건 예측
category: blog
author: hsj00
externalLink: true
published: true
comments: true
share: true
---
업무에서 ALD 공정을 다루다 보면 항상 하는 질문이 있다.
"이 구조에 ALD를 할 때, 바닥까지 100% step coverage를 달성하려면 precursor dosing time을 얼마나 줘야 하지?"
엑셀에 조악하게 수식 때려 넣거나, 감으로 조건을 잡는 게 불편했다. 그래서 직접 계산기를 만들어보기로 했다.

> GitHub: [ALD Gordon calculator](https://github.com/hsj00/ALD-Gordon-Calc)
>
> Streamlit 배포: [ALD Gordon calculator web app](https://ald-gordon.streamlit.app/)

---

## 무엇을 만들었나

이번에도 `Streamlit` 기반 웹 앱이다. 사이드바에서 구조 파라미터와 precursor 정보를 입력하면, Gordon 모델이 필요한 최소 노출량과 도징 시간을 즉시 계산해준다. 설치 없이 브라우저에서 바로 쓸 수 있다.

핵심 물리 모델은 Gordon et al. (2003)의 Eq. 14와 Eq. 24를 기반으로 한다:

- **필요 노출량**: `E = K_max × √(2π·m·k_B·T) × (1/19 + 4a/3 + 2a²)`
- **침투 깊이**: `l(t) = (4w/3) × [√(1 + 3E(t)/8·scale) − 1]`

주요 기능은 다음과 같다:

- **결과 카드**: AR, EAR, 필요 노출량(Langmuir), 필요 펄스 시간, Knudsen 수를 한눈에 표시
- **신호등 해석**: 🟢🟡🔴 색상으로 각 결과의 공정 난이도를 즉시 판단 — "EAR 72, 매우 도전적. 노출량 500L 이상 목표." 같은 1줄 안내 포함
- **Fill Tank 모드**: Fill tank ↔ Chamber ↔ Pump ODE를 수치 적분하여 실제 챔버 압력 프로파일과 누적 노출량을 계산. Model A(상한)와 Model B(ODE 실측) 비교
- **침투 깊이 시간 추적**: 도징 사이클 동안 precursor가 구조 내부로 얼마나 깊이 들어갔는지 시간별로 시각화. t_full(완전 코팅 달성 시각)과 l_max(이론적 최대 깊이)도 계산
- **Coverage Profile θ(z)**: 도징 시간별 스냅샷으로 코팅 프로파일 변화 시각화
- **그래프 탭 4개**: Exposure vs EAR, Pulse Time vs Pressure/Temperature, 구조별 EAR 비교
- **프리셋 5종**: DRAM Capacitor, 3D NAND Slit, 3D NAND Channel Hole, FinFET Gate, Flash WL Mo Fill — 대표 반도체 구조를 클릭 한 번으로 불러오기
- **개념 설명 탭**: Knudsen 수, sticking coefficient, Gordon 수식 등 핵심 물리 개념을 앱 안에서 바로 확인

---

## 왜 만들었나 — 물리적 배경

ALD에서 고종횡비(High Aspect Ratio) 구조를 코팅하는 것은 생각보다 훨씬 어렵다.
논문에서 말하길 DRAM 커패시터(AR≈50:1)에 HfO₂를 코팅하려면 평탄 기판 대비 수백 배의 precursor 노출량이 필요하고, 3D NAND 채널 홀(AR≈100:1)이라면 수천 Langmuir가 요구된다.

공정 난이도는 구조 형상에 따라서도 크게 달라진다. 같은 AR=50:1이라도 Cylindrical Hole은 EAR=50, Infinite Trench는 EAR=25다. 트렌치는 양면이 열려있어 코팅하기가 절반 정도 쉽다. 이런 차이를 직관적으로 확인할 수 있는 도구가 필요했다.

Gordon 모델이 유효하려면 분자 유동(Molecular Flow, Kn > 10) 조건이어야 한다.
압력이 높거나 구조가 넓으면 Kn이 1 이하로 떨어져 점성 유동으로 바뀌고, 이 경우 모델이 부정확해진다. 앱이 자동으로 Kn을 계산해서 모델 유효성을 신호등으로 알려주는 이유다.

---

## AI를 어떻게 활용했나

코드 작성과 물리 모델 검토에 **Perplexity**와 **Claude**를 함께 활용했다.

Perplexity는 Gordon 모델의 물리적 의미, Fill Tank ODE 구조, Knudsen 수 해석 같은 개념들을 빠르게 정리하는 데 썼다. 단순 검색이 아니라 "왜 이 조건에서 이 수식이 나오는지"를 대화하듯 물어보면서 이해를 다졌다.

Claude는 실제 코드를 짜고 다듬는 데 중심적인 역할을 했다. 처음엔 단순한 계산기 수준이었는데, "각 결과에 물리적 해석을 추가해줘", "파라미터를 어떻게 선정해야 하는지 툴팁으로 안내해줘" 같은 요청을 반복하면서 v5까지 발전했다. 코드를 그냥 받아서 붙여넣기 하는 것보다, ALD 공정의 맥락을 설명하면서 "이런 상황에서 이게 왜 중요한지"를 함께 생각하는 방식으로 진행한 게 결과물의 품질을 높여줬다.

버그 수정도 AI 덕분에 빨랐다. 에러 메시지를 그대로 붙여넣으면 원인과 수정 방법을 바로 짚어줬다. 세상이 정말 많이 변하긴 변했다.

---

## 마무리

이것도 한동안 써보면서 얼마나 유용할지는 모르겠지만 그럭저럭 써볼 만한 수준은 된 것 같아서 만족스럽다. DRAM이나 3D NAND 구조에서 fill tank 조건을 바꿔가며 포화도가 어떻게 변하는지 확인하는 데 실제로 사용해보면 더 좋을 것 같다.

코드는 GitHub에 올려뒀고, Streamlit Community Cloud에도 배포해두었다. README에 파라미터 선정 가이드와 실전 예시(DRAM, 3D NAND, Flash Mo Word Line)도 정리해두었으니 참고가 되길 바란다.

> 참고문헌:
> R. G. Gordon et al., *Chem. Vap. Deposition* **9**, 73 (2003) [DOI LINK](https://doi.org/10.1002/cvde.200390005)
> V. Cremers et al., *Appl. Phys. Rev.* **6**, 021302 (2019) [DOI LINK](https://doi.org/10.1063/1.5060967)

---
