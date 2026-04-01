---
title: "논문 읽기 04: 'Deposition of ALD-Molybdenum for Flash Memory Wordline Metallization'"
layout: post
date: 2026-02-17 20:17
tag:
    - ALD
    - Atomic Layer Deposition
    - Molybdenum
    - ALD-Mo
    - NAND Flash
    - Wordline Metallization
    - Semiconductor
    - Memory Device
    - Chemical Kinetics
    - Transport Modeling
    - Lam Research
    - Paper Review
    - Study
    - Note
headerImage: false
image:
description: 260217 `Deposition of ALD-Molybdenum for Flash Memory Wordline Metallization` paper review
category: blog
author: hsj00
externalLink: true
published: true
comments: true
share: true
use_math: true
---

# Paper review

NAND Flash memory의 wordline 금속화 공정과 관련하여 Lam Research에서 발표한 공정 관련 논문을 한 편 읽어봤다. 이 논문은 유전체(Al₂O₃) 위에 직접 핵생성이 가능한 독자적인 ALD-Mo 공정을 제시하며, 화학 반응 및 전달 모델링을 활용하여 300-pair에서 1000-pair로 확장되는 NAND Flash 구조에 대응하는 공정 최적화 방법론을 제공한다. 경험적으로 알고있던 내용들을 이론적으로 정리한 내용들도 있어서 흥미롭게 읽었다. 논문에서는 Barrierless라고 주장하는데, 내가 아는 한 MoN seed layer 없이 Mo만 단독으로 성장시키려면 nucleation이 너무 길었다. 아마도 MoN을 증착한 후에 H₂ spillover 효과를 이용해서 MoN을 Mo로 환원시키는 공정이 껴있을거다. 최근에 읽은 다른 논문에서도 MoN-H₂ treatment에 대한 내용이 있었기 때문에 거의 확실할 것이다. Mo로 환원되니까 barrierless라고 주장하는걸까...? 암튼... 이유는 잘 모르겠지만 그렇다고 하니 일단 넘어가자. 논문을 읽은 후에 AI를 사용해서 좀 더 쉽게 내용을 정리해보았다. 급변하는 시대에 AI를 잘못쓰면 바보가 되기 딱 좋을 것 같지만 일단 이 논문도 직접 읽은건 맞으니까 완전 바보까진 아니길 빌어본다.

ToC는 다음과 같다.
- [Paper review](#paper-review)
  - [논문 서지 정보](#논문-서지-정보)
  - [Abstract](#abstract)
  - [I. Low Resistance Metallization Choices and Ranking](#i-low-resistance-metallization-choices-and-ranking)
    - [금속화 재료 선정 기준](#금속화-재료-선정-기준)
    - [재료 선정 프로세스](#재료-선정-프로세스)
    - [Molybdenum의 우수성](#molybdenum의-우수성)
    - [Figure 설명: Table I - 가능한 금속화 선택지 순위](#figure-설명-table-i---가능한-금속화-선택지-순위)
  - [II. ALD-Mo Precursor Selection](#ii-ald-mo-precursor-selection)
    - [전구체 선정 기준](#전구체-선정-기준)
    - [Self-etching 거동](#self-etching-거동)
    - [MoO₂Cl₂의 선정](#moocl의-선정)
    - [Figure 설명: Table II - ALD-Mo 전구체 순위](#figure-설명-table-ii---ald-mo-전구체-순위)
  - [III. Overcoming Nucleation Delay and Metal Agglomeration](#iii-overcoming-nucleation-delay-and-metal-agglomeration)
    - [A. Creating a Strong Dielectric-Metal Interface](#a-creating-a-strong-dielectric-metal-interface)
    - [배리어 층의 문제점](#배리어-층의-문제점)
    - [B. Depositing Pure ALD-Mo on Dielectrics](#b-depositing-pure-ald-mo-on-dielectrics)
    - [Figure 설명](#figure-설명)
  - [IV. Optimizing ALD-Mo for Wordline Step Coverage](#iv-optimizing-ald-mo-for-wordline-step-coverage)
    - [A. Empirical Chemical Kinetics Modeling](#a-empirical-chemical-kinetics-modeling)
    - [화학 속도론 수식](#화학-속도론-수식)
    - [B. Transport Modeling](#b-transport-modeling)
    - [모델링 결과 및 공정 최적화](#모델링-결과-및-공정-최적화)
    - [Figure 설명](#figure-설명-1)
  - [V. Optimizing Metal Deposition Equipment for ALD-Mo](#v-optimizing-metal-deposition-equipment-for-ald-mo)
    - [동적 공정 적응성의 필요성](#동적-공정-적응성의-필요성)
    - [Lam ALTUS Halo 시스템 아키텍처](#lam-altus-halo-시스템-아키텍처)
    - [다단계 공정 최적화 전략](#다단계-공정-최적화-전략)
  - [VI. Conclusions](#vi-conclusions)
    - [ALD-Mo로의 전환 동인](#ald-mo로의-전환-동인)
    - [모델링 기반 공정 개발](#모델링-기반-공정-개발)
    - [미래 전망](#미래-전망)
  - [반도체 산업 관점에서의 의미와 중요도](#반도체-산업-관점에서의-의미와-중요도)
    - [1. NAND Flash 메모리 스케일링의 핵심 기술](#1-nand-flash-메모리-스케일링의-핵심-기술)
    - [2. 반도체 제조 패러다임의 전환](#2-반도체-제조-패러다임의-전환)
    - [3. 모델링 기반 공정 개발의 산업화](#3-모델링-기반-공정-개발의-산업화)
    - [4. 메모리 산업 경쟁력 강화](#4-메모리-산업-경쟁력-강화)
  - [ALD/CVD 관점에서의 공정적 특징](#aldcvd-관점에서의-공정적-특징)
    - [1. 전구체 화학의 정밀 제어](#1-전구체-화학의-정밀-제어)
    - [2. 표면 반응 메커니즘의 이해](#2-표면-반응-메커니즘의-이해)
    - [3. 핵생성 및 성장 모드 제어](#3-핵생성-및-성장-모드-제어)
    - [4. 고종횡비 구조에서의 Conformal Deposition](#4-고종횡비-구조에서의-conformal-deposition)
    - [5. 온도 윈도우 및 열 관리](#5-온도-윈도우-및-열-관리)
    - [6. 반응 부산물 관리](#6-반응-부산물-관리)
    - [7. 공정 통합 및 스케일링](#7-공정-통합-및-스케일링)
  - [Quotes](#quotes)
- [Note](#note)


## 논문 서지 정보

| 항목      | 내용                                                                 |
| --------- | -------------------------------------------------------------------- |
| 제목      | Deposition of ALD-Molybdenum for Flash Memory Wordline Metallization |
| 학회/저널 | 2025 IEEE International Memory Workshop (IMW)                        |
| 발행일    | 2025년                                                               |
| DOI       | 10.1109/IMW61990.2025.11026947                                       |
| 페이지    | 4 pages                                                              |

---

## Abstract

**핵심 요약:** 메모리와 로직 응용 분야에서 불소 프리(fluorine-free), 배리어리스 ALD Mo로의 전환이 진행 중이며, NAND Flash W/L 응용을 위해 유전체(Al₂O₃) 위에서 우수한 핵생성을 보이는 ALD-Mo 공정을 개발했다. 공식 발표도 있어서 다들 알고있는 내용이겠지만, 삼성전자에서 V9 NAND 양산에서 Lam ALD Mo를 사용했다고 알려져있다.

본 논문은 화학 반응 및 전달 모델링을 통해 NAND Flash W/L 공정 최적화를 수행하고, 300L에서 1000L W/L W/L으로 확장되는 3D NAND device의 요구사항을 예측하는 방법론을 제시한다. 기존 TiN-W 스택 대비 낮은 W/L 저항을 달성하며, 기하학적으로 제한된 NAND Flash 메모리 어레이 내에서 conformal한 증착이 가능하다.

---

## I. Low Resistance Metallization Choices and Ranking

**핵심 요약:** 다양한 금속 재료의 W/L 적용 가능성을 평가한 결과, 몰리브덴(Mo)이 유전체 위 직접 증착 능력, 고융점, 저비용, 우수한 식각 특성으로 인해 최적의 선택임을 입증하였다.

### 금속화 재료 선정 기준

NAND Flash W/L 금속화를 위한 산업 표준은 텅스텐(W)이다. TiN 배리어와 함께 통합된 ALD-W는 우수한 충진 능력, 거의 제로에 가까운 일렉트로마이그레이션(고융점), 잘 개발된 습식/건식 식각 공정, 상대적으로 낮은 비용을 가지고 있다. 그러나 TiN-W 금속화 스택의 저항률이 급격히 병목 현상을 일으키고 있다.

전자 평균 자유 경로(λ)와 금속 비저항(ρ)의 곱은 라인 저항과 잘 스케일링되며, 이 지표로 볼 때 논문의 Table 1에 나열된 모든 원소들이 TiN-W보다 낮은 저항을 생성할 잠재력을 가지고 있다. 그러나 W/L 금속 교체 시 저항 이상의 요소를 고려해야 한다.

### 재료 선정 프로세스

- 노드 분리를 위한 적절한 습식 또는 건식 식각 공정이 없는 원소들은 고려 대상에서 제외되었다 (Os, Ir)
- 낮은 융점 또는 지나치게 높은 비용을 가진 금속들도 제외되었다 (Cu, Al, Rh)
- Ni, Co, Ru, Mo는 모두 W 대비 잠재적으로 매력적이다

### Molybdenum의 우수성

Mo를 다른 후보들보다 돋보이게 만드는 것은 **ALD를 사용하여 유전체 위에 직접 증착할 수 있는 능력**이다. 이는 TiN과 같은 고저항 라이너 또는 배리어 층의 필요성을 제거하여, 저항이 낮은 금속 전도체를 위한 귀중한 부피를 확보한다. 고융점, 저비용, 잘 개발된 습식 및 건식 식각 공정과 결합하여, 몰리브덴은 W/L 저항 스케일링을 위한 명확한 선택이다.

### Figure 설명: Table I - 가능한 금속화 선택지 순위

Table 1은 다양한 금속 재료의 주요 특성을 비교한다.

- **λ × ρ 값**: Mo (5.99), W (8.20) - 값이 낮을수록 우수
- **융점**: Mo (2623°C), W (3422°C) - 높을수록 일렉트로마이그레이션 저항성 우수
- **상대 비용**: Mo (0.8), W (1.0 기준) - Mo가 W보다 20% 저렴
- **식각 가능성**: Mo와 W 모두 습식/건식 식각 가능 표시(●)
- **배리어 필요성**: Mo는 배리어 불필요(●), W는 배리어 필요(●)

Cu와 Al은 λ × ρ 값이 우수하지만 낮은 융점으로 제외되었고, Rh, Os, Ir은 매우 높은 비용(W 대비 260~520배)으로 실용성이 없다. Ru는 비용이 26배로 높고, Ni와 Co는 배리어가 필요하다는 점에서 Mo가 종합적으로 가장 우수한 선택임을 보여준다.

---

## II. ALD-Mo Precursor Selection

**핵심 요약:** 기판 손상 방지와 self-etching 특성을 고려하여, 몰리브덴의 염화물 및 산화염화물 중 MoO₂Cl₂가 NAND Flash W/L 응용에 최적의 ALD 전구체로 선정되었다.

### 전구체 선정 기준

기판 손상(Al₂O₃, SiN)을 피하기 위해 몰리브덴의 염화물(chlorides)과 산화염화물(oxi-chlorides)에 집중하였다. MoCl₂, MoCl₃, MoCl₄는 무시할 만한 증기압으로 인해 고려 대상에서 제외되었다.

### Self-etching 거동

더 휘발성이 높은 종들의 경우, 전구체 농도에 따라 Mo 증착에서 Mo 식각으로 전환되는 능력이 중요한 인자이다. MoOₓClᵧ 전구체들의 self-etching 거동은 Cl/Mo 비율과 대략적으로 상관관계가 있는 것으로 관찰되었다.

NAND Flash W/L에서 self-etching은 다음과 같은 문제를 야기한다.
- 상부 W/L: 높은 농도에서 완전히 식각됨
- 중간 W/L: 이상적인 농도에서 정상 증착
- 하부 W/L: 낮은 농도에서 전구체 고갈로 인한 증착 또는 식각 없음

### MoO₂Cl₂의 선정

이러한 분석은 MoO₂Cl₂를 NAND Flash W/L 응용을 위한 ALD-Mo 전구체 선택으로 만든다. MoO₂Cl₂는 Cl/Mo = 2로, 높은 농도에서도 식각이 발생하지 않아 균일한 증착이 가능하다.

### Figure 설명: Table II - ALD-Mo 전구체 순위

Table 2는 몰리브덴 전구체들의 특성을 비교한다.

- **MoO₂Cl₂**: Mo 산화 상태 +6, 높은 증기압, Cl/Mo = 2 → 높은 농도에서 식각 없음 (**선정**)
- **MoOCl₄**: Mo 산화 상태 +6, 낮은 증기압, Cl/Mo = 4 → 높은 농도에서 약한 식각
- **MoCl₅**: Mo 산화 상태 +5, 낮은 증기압, Cl/Mo = 5 → 낮은 농도에서 강한 식각
- **MoCl₄, MoCl₃, MoCl₂**: 무시할 만한 증기압으로 ALD 전구체로 부적합하나, ALD-Mo 반응의 중간체로 가능

Cl/Mo 비율이 낮을수록 self-etching 위험이 감소하므로, MoO₂Cl₂가 가장 안전하고 균일한 증착을 보장하는 전구체로 선정되었다.

---

## III. Overcoming Nucleation Delay and Metal Agglomeration

**핵심 요약:** Lam ALTUS Halo 시스템은 유전체-금속 계면에 고저항 층 없이 직접 ALD-Mo를 증착하여, 핵생성 지연 제로와 우수한 밀착력 및 응집 저항성을 달성하였다. ~(자기들 장비가 최고라는 소리다)~

### A. Creating a Strong Dielectric-Metal Interface

유전체 위에 금속 막을 직접 성장시키는 것은 어렵다. 왜냐하면 금속-금속 인력이 일반적으로 금속-유전체 인력보다 훨씬 강하기 때문이다. 이는 긴 핵생성 지연, 낮은 밀착력, 금속 응집을 초래할 수 있다.

전통적으로 이러한 문제들은 유전체와 금속 사이에 금속 질화물을 도입하여 극복되었다 (예: TiN-W). 금속 질화물은 다음과 같은 역할을 한다.
- 유전체 위에서 잘 핵생성되는 가교층 생성 (유전체-질화물 인력 ≈ 질화물-금속 인력)
- 강한 질화물-금속 계면 생성 (질화물-금속 인력 ≈ 금속-금속 인력)

### 배리어 층의 문제점

이 접근법의 문제는 얇은 금속-질화물 층이 높은 비저항(10² ~ 10³ µΩ-cm)을 가지며, 저항률이 낮은 금속(≈10¹ µΩ-cm)으로 채워져야 할 부피를 소비한다는 것이다.

현재 300-pair Flash W/L의 pillar-pillar 개구부 폭은 10-20 nm 수준이며 빠르게 축소되고 있다.

일반적인 300-pair Flash 메모리 어레이에서
- 3 nm 계면 라이너: 가용 W/L 부피의 30%만 저항률 낮은 금속으로 채워지고, W/L 저항이 5배 이상 증가
- 1 nm 계면 라이너: 가용 W/L 부피의 75%가 금속으로 채워지고, all-metal 대비 저항이 50% 증가

따라서 Flash W/L 스케일링을 가능하게 하려면 계면 라이너 부피를 제거하는 것이 필수적이다.

### B. Depositing Pure ALD-Mo on Dielectrics

Lam ALTUS Halo ALD-Mo 증착 시스템은 금속-유전체 계면에서 고저항 층 없이 저항률이 낮은 ALD-Mo 막을 생성하는 증착 공정을 활용한다.

이는 다음과 같은 결과를 가져온다.
- 유전체 위에서 핵생성 지연 제로인 ALD-Mo 막
- 낮은 비저항
- 우수한 밀착력 및 응집 저항성

### Figure 설명

**Figure 1 - 유전체-금속 계면**: 금속 원자들 간의 강한 인력(Metal-Metal attraction)이 유전체와 금속 간의 약한 인력(Metal-Dielectric attraction)을 나타내며, 이것이 직접 증착을 어렵게 만드는 원리를 도식화한다.

**Figure 2 - Flash 메모리 어레이 기하학**: NAND Flash W/L 구조의 기하학적 파라미터를 정의한다. Pillar 직경(d), pillar 간 거리(a), slit 간 거리(2L) 등이 표시되어 있으며, 전구체가 수직 슬릿을 통해 수평으로 확산되어 W/L을 채우는 구조를 보여준다.

**Figure 3 - 금속으로 채워진 W/L 부피 비율**: Normalized pillar-pillar 개구부 폭 (a-d)/L에 따른 금속이 차지하는 부피 비율을 보여준다. 라이너 두께가 증가할수록(0nm → 3nm) 금속 부피가 급격히 감소하며, 300-pair 위치에서 3nm 라이너는 금속 부피를 약 30%까지 감소시킨다.

**Figure 4 - 정규화된 W/L 저항**: Normalized pillar-pillar 개구부 폭에 따른 W/L 저항 증가를 로그 스케일로 표시한다. 3nm 라이너 사용 시 300-pair에서 저항이 5배 이상 증가하며, 개구부가 좁아질수록 라이너의 영향이 기하급수적으로 증가함을 보여준다.

**Figure 5 - Al₂O₃ 위의 ALD-Mo**: 계면 층 없이 Al₂O₃ 유전체 위에 직접 증착된 ALD-Mo의 단면 이미지를 보여주며, 명확한 계면과 균일한 Mo 층을 확인할 수 있다.

**Figure 6 - 핵생성 지연 비교**: ALTUS Halo ALD-Mo와 표준 ALD-Mo의 Al₂O₃ 위 증착 거동을 비교한다. 
- ALTUS Halo ALD-Mo: 초기 사이클부터 선형적으로 두께 증가 (핵생성 지연 0)
- 표준 ALD-Mo: 약 100 사이클까지 증착이 거의 일어나지 않음 (긴 핵생성 지연)

이는 ALTUS Halo 공정이 유전체 위에 직접 핵생성이 가능함을 명확히 입증한다.

**Figure 7 - 비저항 비교**: 총 두께에 따른 비저항을 비교한다.
- TiN + W: 5nm에서 ~100 µΩ-cm, 20nm에서 ~40 µΩ-cm
- ALD-Mo: 5nm에서 ~20 µΩ-cm, 20nm에서 ~15 µΩ-cm

ALTUS Halo ALD-Mo의 비저항이 일반적인 TiN-W 막 스택의 비저항보다 현저히 낮으며, 특히 얇은 두께에서 그 차이가 더욱 두드러진다.

---

## IV. Optimizing ALD-Mo for Wordline Step Coverage

**핵심 요약:** 경험적 화학 반응 속도론 모델링과 전달 모델링을 결합하여 ALD-Mo의 W/L 내부 스텝 커버리지를 예측하고 최적화하였으며, 증착 진행에 따른 공정 조건 조정 전략을 제시하였다.

### A. Empirical Chemical Kinetics Modeling

전체 ALD-Mo 반응식은 다음과 같다:

\[ \text{MoO}_2\text{Cl}_2 + 3\text{H}_2 \rightarrow \text{Mo} + 2\text{H}_2\text{O} + 2\text{HCl} \] (1)

이는 다음의 주요 단계들로 분해될 수 있다.

1. 가용한 Mo 사이트에서 MoO₂Cl₂의 해리 흡착 → 흡착된 Cl과 표면 MoOₓ 생성 (속도 제한 아님)
2. 가용한 금속 Mo 사이트에서 H₂의 해리 흡착 (속도 제한 아님)
3. 흡착된 H*와 표면 MoOₓ의 반응 (속도 제한 가능성 낮음)
4. 반응 부산물(H₂O, HCl)의 탈착 → H₂, MoO₂Cl₂ 흡착을 위한 새로운 금속 사이트 생성 (속도 제한 가능성 높음)

### 화학 속도론 수식

운동 이론으로부터, 기판으로의 전체 전구체 질량 플럭스 속도(\(j_i''\))는 다음과 같이 표현된다.

\[ j_i'' = \frac{1}{4} \cdot \omega_i \cdot \rho \cdot \bar{c}_i \cdot \varepsilon_i \] (2)

여기서 \(\varepsilon_i\)는 전구체 sticking coefficient이며, 박막 증착의 스텝 커버리지와도 밀접하게 관련된다 (\(\omega_i\) = 질량 분율, \(\rho\) = 밀도, \(\bar{c}_i\) = 평균 열속도).

전구체 질량 플럭스(\(j_i''\))는 화학 반응 속도 식으로도 표현될 수 있다.

\[ j_i'' \approx k_1 \cdot (\rho \cdot \omega_{Mo} \cdot \Delta t_{Mo \text{ dose}})^a \cdot (\rho \cdot \omega_{H_2} \cdot \Delta t_{H_2 \text{ dose}})^b \cdot \exp\left(\frac{-E_a}{RT}\right) \] (3)

여기서 \(k_1\)은 속도 상수, \(\Delta t_i\)는 투여 시간, \(E_a\)는 전체 반응 활성화 에너지이다.

식 (2)와 (3)을 결합함으로써, ALD-Mo 스텝 커버리지 모델의 필수 입력 값인 전구체 증착을 지배하는 sticking coefficient \(\varepsilon_i\)를 구할 수 있다.

### B. Transport Modeling

NAND Flash W/L의 기하학은 극도로 복잡하지만(Figure 2, 8), 적절한 단순화를 통해 일시적인 W/L 증기 전달을 모델링하고 공정 트렌드를 추출할 수 있다.

확산 방정식(4)을 사용하여 ALD 중 W/L 내부의 일시적인 전구체 질량 분율(\(\omega_{Mo}\))을 모델링한다.

\[ \frac{\partial \omega_{Mo}}{\partial t} = D_{eff} \cdot \frac{\partial^2 \omega_{Mo}}{\partial x^2} \] (4)

이 식의 유효 확산 계수 \(D_{eff}\)는 원통형 배열의 다공성 매체 내에서 횡류 중인 전구체 분자의 자유 분자 확산도이며, 2L은 어레이의 slit 간 간격이다.

적절한 스케일링 및 경계 조건을 통해, 식 (4)의 해는 Figure 8에 나타난 바와 같이 NAND Flash 메모리 어레이 내부의 일시적인 전구체 농도 프로파일에 대한 유용한 추정치를 제공한다.

### 모델링 결과 및 공정 최적화

W/L 포화 시간은 ALD-Mo 증착이 W/L seam 폐쇄를 향해 진행됨에 따라 증가한다 (Figure 9). 그러나 W/L이 완전히 열려 있는 공정 초기부터 끝의 seam 폐쇄까지 증착 조건을 조정함으로써, 합리적인 투여 시간으로 완전한 포화를 유지할 수 있다.

더 많은 W/L pair를 가진 구조로의 미래 NAND Flash 스케일링도 공정 스케일링을 필요로 한다 (Figure 10). 미래의 1000-pair NAND Flash가 정확히 어떤 모습일지는 알 수 없지만, 대략적인 스케일링만으로도 미래 소자 요구사항을 예측하고 준비할 수 있다.

### Figure 설명

**Figure 8 - 셀 어레이 내 NAND Flash wordline**: 전구체 분자가 전구체 투여 시작 시 어레이 양쪽의 수직 슬릿으로부터 W/L 어레이로 측면으로 확산되는 것을 보여준다. 복잡한 3D 구조를 2D 확산 문제로 단순화하여 모델링할 수 있음을 나타낸다.

**Figure 9 - Wordline 내 증착 금속 두께 대비 ALD-Mo 전구체 포화 시간 스케일링**: 
- X축: Mo 두께 (임의 단위), 수직 점선은 W/L이 닫히는 두께 표시
- Y축: Mo 포화 시간 (임의 단위)
- 두 가지 조건 비교:
  - 1배 전구체 농도: 60T에서 7.2%, 90T에서 14.4% 포화도
  - 3배 전구체 농도: 더 빠른 포화 달성

증착이 진행되어 W/L이 좁아질수록 포화 시간이 급격히 증가하지만, 전구체 농도를 3배로 증가시키면 포화 시간을 크게 단축할 수 있음을 보여준다. 이는 공정 중 동적 조건 조정의 필요성을 입증한다.

**Figure 10 - 증착 시작 시 wordline 수 대비 ALD-Mo 전구체 포화 시간**: 고정된 기하학에서 W/L 수가 증가할수록(300 → 1000) 포화 시간이 증가하는 트렌드를 보여준다. 이는 미래 고층 NAND Flash 구조에 대응하기 위한 공정 조건 최적화의 필요성을 제시한다.

---

## V. Optimizing Metal Deposition Equipment for ALD-Mo

**핵심 요약:** ALD-Mo 충진 공정 중 증기 전달이 극적으로 변화하므로, Lam ALTUS Halo 시스템은 4-스테이션 챔버 아키텍처를 활용하여 공정 초기, 중기, 말기의 레시피 최적화를 가능하게 한다.

### 동적 공정 적응성의 필요성

In-feature 모델링에서 볼 수 있듯이, ALD-Mo 충진 공정 중 증기 전달이 극적으로 변화한다. 따라서 ALD 시스템은 공정의 초기, 중기, 말기에서 레시피 최적화를 가능하게 해야 한다.

### Lam ALTUS Halo 시스템 아키텍처

Lam ALTUS Halo ALD-Mo 시스템은 4-스테이션 챔버 아키텍처를 활용하며, 이를 통해 사용자는 스테이션마다 다음을 최적화할 수 있다.
- 웨이퍼 온도
- 가스 타이밍
- 가스 유량

이러한 공정 적응성은 Flash 메모리가 300 W/L을 넘어서 스케일링됨에 따라 더욱 중요해질 것으로 예상된다.

### 다단계 공정 최적화 전략

4-스테이션 구조의 장점:
1. **초기 단계**: W/L이 완전히 열려 있을 때 - 높은 유량, 짧은 투여 시간
2. **중간 단계**: W/L이 부분적으로 채워질 때 - 중간 조건
3. **말기 단계**: Seam 폐쇄 직전 - 낮은 유량, 긴 투여 시간, 높은 전구체 농도
4. **최종 단계**: Seam 폐쇄 및 오버버든 - 조건 재조정

각 단계에서 독립적인 공정 조건 설정이 가능하여, 전체 증착 과정에서 최적의 스텝 커버리지와 균일도를 유지할 수 있다.

논문 읽으면서 Lam 장비도 그렇고 공정도 그렇고 참 대단하다는 생각을 했는데, Batch type V/F에서 W/L을 채우는 TEL은 그럼 얼마나 더 대단한건가 싶어서 어질어질했다.

---

## VI. Conclusions

**핵심 요약:** TiN 부피 소비가 NAND Flash 스케일링의 장애물이 되고 있으며, 배리어 없이 유전체 위에 직접 증착 가능한 conformal하고 저저항인 ALD-Mo가 해결책으로 제시되었다. 화학 반응 및 전달 모델링을 통한 공정 개발로 시장 출시를 가속화하고 있다.

### ALD-Mo로의 전환 동인

ALD-TiN + ALD-W W/L 금속화는 NAND Flash 메모리를 가능하게 한 핵심 공정 모듈 중 하나였다. 그러나 Flash 메모리 어레이가 축소됨에 따라, TiN이 소비하는 W/L 부피가 급격히 추가 스케일링의 장애물이 되고 있다.

현재 유전체 기판 위에 직접 conformal하고 저저항인 ALD-Mo를 생성할 수 있다 (TiN 없음). 이러한 변화가 현재 TiN-W에서 ALD-Mo로의 전환을 이끄는 주요 동인 중 하나이다.

### 모델링 기반 공정 개발

NAND Flash W/L 내부에 우수한 금속 충진을 달성하는 것은 계속되는 요구인데, Lam에서는 화학 반응 및 전달 모델링으로 공정 개발을 안내하여 시장 솔루션을 내놓는다고 하니 큰 회사는 달라도 뭔가 다르구나 싶다.

- **경험적 화학 반응 속도론 모델링**: 관찰된 공정을 설명하는 프레임워크 제공
- **Wordline 전달 모델링**: ALD 투여 및 퍼지 요구사항 추정, 유망한 공정 윈도우 제시

### 미래 전망

300-pair를 넘어 1000-pair로 확장되는 미래 NAND Flash 구조에 대응하기 위해 공정 조건의 동적 최적화와 장비 아키텍처의 유연성이 더욱 중요해질 것이라고 하는데, 결국 Lam의 모델링 접근법이 핵심적인 역할을 할 것으로 기대한다는 자랑아닌 자랑이 결론이다. 홍보 영상이나 아티클 보면 그만큼 자신이 있으니 소구점으로 삼는게 아닐까 싶다.

---

## 반도체 산업 관점에서의 의미와 중요도

### 1. NAND Flash 메모리 스케일링의 핵심 기술

**저항 병목 현상 해결**: 3D NAND Flash가 고층화되면서(300-pair → 1000-pair) W/L 저항이 급격히 증가하는 문제는 메모리 성능과 신뢰성을 저하시키는 주요 병목 현상이다. TiN-W 스택에서 ALD-Mo로의 전환은 단순한 재료 교체가 아니라, W/L 부피의 30-75%를 저항률 낮은 금속으로 확보할 수 있는 근본적인 구조 혁신이다. 그러니 다들 기를 쓰고 Mo로 넘어가려는게 아닐까 싶다.

**비용 경쟁력**: Mo는 W 대비 20% 저렴하면서도(상대 비용 0.8), Ru 대비 97% 저렴하다(Ru는 26배). 배리어 층(TiN) 제거로 공정 단계가 단순화되어 fab 생산성이 향상되며, 웨이퍼 처리량 증가와 장비 가동률 개선으로 이어진다고 예상할 수 있다. 그런데 MoN+Mo 공정이 유지되고 있는걸로 알고있는 이상 공정 단계 단순화는 말처럼 단순하지 않을 수 있을 것 같다. In-situ로 한 번에 MoN과 Mo를 연속 공정으로 덮는다고 하면 좀 나아질려나...?

### 2. 반도체 제조 패러다임의 전환

**배리어리스 금속화**: 기존 패러다임은 "유전체 → 배리어(TiN) → 금속(W)"의 3층 구조였으나, ALD-Mo는 "유전체 → 금속(Mo)" 직접 증착을 가능하게 한다. 이는 10nm 이하 협소 공간(confined space)에서 유효 전도 면적을 2배 이상 증가시킬 수 있는 혁신이다.

**핵생성 제어 기술**: 유전체 위 금속의 직접 핵생성은 표면 화학 및 계면 공학의 난제였다. Lam의 ALTUS Halo 공정이 핵생성 지연 제로를 달성한 것은, 향후 다른 금속 재료(Ru, Co 등)의 배리어리스 증착에도 적용 가능한 플랫폼 기술로 확장될 수 있다. 100% 될거라고 생각하지는 않지만 뭐... 미래는 모르는 일이니까.

### 3. 모델링 기반 공정 개발의 산업화

**Digital Twin 접근법**: 화학 반응 속도론 모델링과 전달 모델링을 결합한 접근법은 실험 횟수를 대폭 줄이고, 1000-pair와 같은 미래 구조에 대한 선제적 대응을 가능하게 한다. 이는 반도체 fab의 R&D 비용 절감과 time-to-market 단축에 직접적으로 기여한다.

**공정 윈도우 최적화**: 4-스테이션 아키텍처를 통한 단계별 조건 최적화는, 공정 마진을 확보하고 양산 수율을 향상시키는 핵심 요소이다. 특히 고종횡비(high aspect ratio) 구조에서 상부-하부 균일도 확보는 메모리 셀의 전기적 특성 균일성과 직결된다.

### 4. 메모리 산업 경쟁력 강화

**차세대 메모리 로드맵 지원**: 현재 양산 중인 200-300층 V-NAND를 넘어 500-1000층으로 확장하려는 삼성, SK하이닉스, Micron 등의 로드맵에서 ALD-Mo 기술은 필수 불가결한 요소이다. W/L 저항 문제가 해결되지 않으면 고층화는 물리적 한계에 직면한다.

**Read/Program/Erase 성능 개선**: W/L 저항 감소는 메모리 동작 속도를 직접적으로 향상시킨다. 특히 긴 W/L을 가진 고층 구조에서 RC 지연 감소는 tPROG(program time)와 tR(read time) 단축으로 이어져, QLC/PLC NAND의 성능 저하를 완화할 수 있다.

---

## ALD/CVD 관점에서의 공정적 특징

### 1. 전구체 화학의 정밀 제어

**MoO₂Cl₂의 Self-limiting 특성**: Cl/Mo 비율 = 2인 MoO₂Cl₂는 높은 전구체 농도에서도 self-etching이 발생하지 않아, self-limiting ALD 모드를 구현한다. 이는 고종횡비 구조의 상부-하부 두께 균일도 확보에 결정적이다.

**산화 상태 제어**: Mo⁺⁶ 산화 상태의 MoO₂Cl₂가 H₂ 환원제와 반응하여 Mo⁰으로 환원되는 메커니즘은, 불소계 전구체(MoF₆ 등) 대비 기판 손상이 없고, 유전체(Al₂O₃, SiN)와의 계면 품질이 우수하다. 불소는 유전체를 식각하거나 금속 막에 불순물로 잔류할 수 있으나, 염소는 HCl로 쉽게 탈착된다.

### 2. 표면 반응 메커니즘의 이해

**속도 제한 단계(RLS) 분석**: 부산물(H₂O, HCl) 탈착이 속도 제한 단계로 식별되었다. 이는 다음과 같은 공정 최적화 방향을 제시한다:
- **Purge 시간 최적화**: HCl과 H₂O의 완전한 제거를 위해 충분한 purge 시간 확보
- **온도 조절**: 탈착 속도를 높이기 위한 적절한 기판 온도 설정
- **캐리어 가스 선택**: Ar 또는 N₂ 유량 최적화로 부산물 제거 효율 향상

**Sticking Coefficient 모델링**: 식 (2)와 (3)을 결합한 경험적 모델링은, 전구체 투여 시간, H₂ 투여 시간, 온도의 함수로 sticking coefficient를 예측 가능하게 만든다. 이는 공정 레시피 개발 시 실험 공간을 대폭 축소시킨다.

### 3. 핵생성 및 성장 모드 제어

**Zero Nucleation Delay**: 표준 ALD-Mo는 약 100 사이클의 핵생성 지연을 보이지만, ALTUS Halo 공정은 첫 사이클부터 선형 성장을 달성한다. 이는 다음과 같은 의미를 가진다:
- **초기 막 품질**: 핵생성 단계의 island 성장이 제거되어, 초기 몇 nm부터 연속적인 금속 막 형성
- **계면 저항 최소화**: 불연속적인 island 구조 대신 연속 막으로 인해 전자 산란 감소
- **밀착력 향상**: 유전체와 금속 간 화학적 결합 최적화로 접착 에너지 증가

**Layer-by-layer 성장**: 이상적인 ALD 모드인 Frank-van der Merwe(층상) 성장을 유도하여, Volmer-Weber(island) 성장을 억제한다. 이는 표면 처리 또는 초기 사이클의 화학 조성 변형을 통해 달성되었을 것으로 추정된다.

### 4. 고종횡비 구조에서의 Conformal Deposition

**확산 제한 전달(Diffusion-limited Transport)**: Knudsen 수 > 1인 자유 분자 영역에서, 전구체 확산이 반응 속도보다 느려질 때 발생한다. 식 (4)의 확산 방정식은:
- **유효 확산 계수 \(D_{eff}\)**: 다공성 매체(pillar 배열)의 기하학적 제약을 Bruggeman 상관식으로 보정
- **농도 구배**: Slit에서 W/L 중심부로 갈수록 전구체 농도 감소
- **포화 시간**: W/L이 좁아질수록 포화 시간이 지수함수적으로 증가 (Figure 9)

**동적 공정 조건 조정**: 
- **초기(0% fill)**: 짧은 투여 시간, 낮은 전구체 농도 → 빠른 확산
- **중기(50% fill)**: 투여 시간 증가, 전구체 농도 증가 → 포화 유지
- **말기(100% fill)**: 긴 투여 시간, 3배 전구체 농도 → seam 폐쇄 달성 (Figure 9)

### 5. 온도 윈도우 및 열 관리

**ALD 온도 범위**: 논문에서 명시적으로 언급되지 않았으나, 내가 아는 MoO₂Cl₂ ALD 온도는 400-600°C 영역으로 추정된다. 그래서 다음과 같은 부분을 주의해서 적절한 온도를 선택해야한다.

- **전구체 열분해 방지**: 너무 높은 온도에서는 MoO₂Cl₂가 자발적으로 분해되어 CVD 모드로 전환
- **부산물 탈착 촉진**: 충분히 높은 온도에서 HCl과 H₂O의 빠른 탈착
- **유전체 손상 방지**: Al₂O₃와 SiN의 열적 안정성 범위 내에서 공정 수행

**4-스테이션 온도 제어**: 각 스테이션마다 독립적인 온도 설정이 가능하여, 증착 단계별 최적 온도 프로파일 구현이 가능하다는게 Lam 설비의 장점으로 알고있다. 그래서 상황에 따라 증착 온도를 다르게 해서 Mo를 성장시킬 수 있다는 점에서 공정적으로 유리한 것 같다.

### 6. 반응 부산물 관리

**HCl 및 H₂O 제거**: 
- **HCl**: 강한 산성 가스로 챔버 부식 및 잔류 시 다음 사이클의 전구체 흡착 방해
- **H₂O**: 금속 막 산화 또는 표면 재산화 유발 가능성

**Purge 전략**: 
- Ar 또는 N₂ 캐리어 가스를 이용한 효과적인 부산물 제거
- 고종횡비 구조에서는 purge 시간이 투여 시간보다 길어질 수 있음
- 잔류 부산물은 다음 사이클의 sticking coefficient를 감소시켜 스텝 커버리지 저하 유발

### 7. 공정 통합 및 스케일링

**300-pair에서 1000-pair로의 스케일링**: 
- W/L 수가 증가하면 slit 간 거리(2L)가 증가하여, 전구체가 중심부까지 도달하는 시간이 길어진다 (Figure 10)
- 포화 시간이 W/L 수의 제곱에 비례하여 증가할 수 있으므로, 전구체 농도 증가 또는 다단계 투여 전략 필요
- Lam의 모델링 접근법은 실제 1000-pair 구조가 개발되기 전에 공정 조건을 예측 가능하게 함

**장비 아키텍처의 중요성**: 
- 4-스테이션 설계는 각 스테이션에서 서로 다른 증착 단계(초기/중기/말기/오버버든)를 동시에 처리 가능
- 이는 웨이퍼 처리량(throughput)을 최대 4배 증가시킬 수 있으며, fab의 CoO(Cost of Ownership) 절감에 기여

---

## Quotes

> "Multiple metallization applications in both memory and logic are transitioning to fluorine-free, barrierless ALD molybdenum."

> "For the NAND Flash wordline application we have developed a proprietary ALD-Molybdenum (ALD-Mo) process that nucleates well on dielectrics (Al₂O₃), enabling low wordline resistance in geometrically constrained NAND Flash memory arrays."

> "But what really sets Mo above the others is our ability to deposit it directly on dielectrics using ALD. This eliminates the need for a high-resistivity Liner or Barrier layer, such as TiN, freeing up valuable volume for more low resistance metallic conductor."

> "In a typical 300-pair Flash memory array, a 3 nm interfacial liner would leave just 30% of the available wordline volume for low resistivity metal, and increase wordline resistance by over 5-times."

> "The self-etching behavior of MoOₓClᵧ precursors has been observed to roughly correlated with their Cl/Mo ratios."

> "This makes MoO₂Cl₂ the ALD-Mo precursor choice for the NAND Flash wordline application."

> "Lam ALTUS Halo ALD-Mo deposition systems utilize deposition processes that create low-resistivity ALD-Mo films without a high-resistance layer at the metal-dielectric interface."

> "This results in ALD-Mo films with zero nucleation delay on dielectrics and low resistivity."

> "Desorption of reaction byproducts (H₂O, HCl) creates new metal sites for H₂, MoO₂Cl₂ adsorption – Likely rate limiting."

> "By combining equations (2) and (3) we can solve for the sticking coefficient governing precursor deposition εᵢ, which is a necessary input to ALD-Mo step coverage models."

> "Wordline saturation time increases as ALD-Mo deposition progresses towards wordline seam closure."

> "However, by adjusting deposition conditions from the beginning of the process, when the wordlines are wide open, to seam closure at the end, we can maintain complete saturation with reasonable dose times."

> "Lam ALTUS Halo ALD-Mo systems utilize a 4-station chamber architecture, which allows users to optimize wafer temperature, gas timing, and gas flows from station to station."

> "We can now produce conformal, low resistivity ALD-Mo directly on dielectric substrates (no TiN). This change is one of the key drivers for the current transition from TiN-W to ALD-Mo."

> "At Lam we guide our process development with chemistry and transport modeling to help speed our solutions to market."

---
# Note

오랜만에 논문 읽기 업데이트다. 예전엔 포스팅 하나 편집하는데 몇 시간씩 걸렸는데 시간이 좀 줄어든 것 같기도 하다. 마음이 여유로웠으면 중요한 키워드들에 도움이 될만한 외부 링크들도 걸겠지만 그건 나중에 시간날 때 따로 업데이트 하련다. 마크다운 편집하기 너무 힘들다.

이번에 읽은 논문은 생업과 밀접한 관련이 있는 내용을 담고있다. Field Process Eng'r에서 Process Development Eng'r로 직무를 바꾸고 나니깐 개발팀 업무라는게 생각보다 생각했던 것보다 훨씬 어렵다는 생각이 들었다. 패턴에서도 작동할 수 있는 공정 레시피를 만들어야 하는 상황이라 고민이 많은 상황에서 읽게된 논문이라 도움이 많이 됐다. 3D NAND와 같은 HAR 구조에서 균일하게 증착하기 위해서는 결국 전구체의 농도와 노출 시간이 핵심이란 생각을 계속 해왔고 경험적으로도 그렇다고 알고 있었는데, 이렇게 정리된 논문으로 알고있던 내용들이 정리가 되니깐 좀 더 속시원한 기분이 들었다. NAND 타겟으로 하는 ALD Mo도 KTC에서 개발하고 있을려나? 논문과 별개로 그게 제일 궁금하다. 다음에는 삼성에서 발표한 논문을 읽고 포스팅을 해볼까 싶다. 오늘은 여기까지.