# Eye Care Switchable Display + Privacy — LVA(시야각) 시뮬레이터

모바일 OLED(Pentile RGBG, Diamond, **원형 Anode**)에서
**Eye Care Switchable Display + Privacy**를 결합했을 때, BM(Black Matrix) 구조에
의한 빛의 윈도우 출사각(시야각/LVA)을 **기하광학(ray clipping / vignetting)** 으로
계산하고 단면을 시각적으로 보여주는 단일 파일 웹앱입니다.

## 실행
`index.html`을 브라우저로 열기만 하면 됩니다. (빌드/서버 불필요)

## 화면 구성
- **좌측**: 모든 구조 파라미터 슬라이더(해상도, Anode 지름, PDL Gap, BM 스택업, 개선안)
- **중앙**: 단면도(µm 스케일, 통과광선=초록/차단광선=빨강), Diamond 평면도
- **우측**: 휘도-각도 곡선, 핵심 결과(손해시작각/반치각/차단각), Sky 유무 비교, PPI별 요약표

## 핵심 사양 (입력값)
| 항목 | 값 |
|---|---|
| 해상도 | 499 / 566 / 610 PPI (선택) |
| Privacy 유효 해상도 | PPI / √2 → 353 / 400 / 430 PPI (1/2 면적 배치) |
| Anode 지름 | Blue 15.2 · Green 13.44 · **Red 14.6(추정)** · SkyBlue 15.2 µm |
| PDL Gap(499) | Blue↔Green 21.67 · Red↔Green 22.56 µm (타 PPI는 pitch 비율 자동 스케일) |
| BM2/BM3 두께 | 2.0 µm · 최소 선폭 6.0 µm |

## BM 스택업 (z=0 = Anode 발광면, 단위 µm)
입력 규칙으로부터 자동 계산되며 모두 슬라이더 조정 가능:

| 층 | 계산 | 기본 z범위 |
|---|---|---|
| BM1 | Anode + 9.5, 두께 4.3 | 9.5 ~ 13.8 |
| BM2 | BM1.top + 6, 두께 2 | 19.8 ~ 21.8 |
| BM3 | **BM2.bottom + 7.5**, 두께 2 | 27.3 ~ 29.3 |
| OC | BM3.top + 2.5 (윈도우 부착면) | top = 31.8 |

## 계산 모델
- **원형 Anode**: 단면에서 발광폭 = 지름, 발광면 z=0. 투과율은 지름 구간을 21점 샘플 평균.
- **BM bar**: 인접 subpixel 사이 PDL Gap마다 생성. BM1은 Gap 전체를 덮는 불투명 bar,
  BM2/BM3/BM4는 Gap 중앙에 **선폭(line width)** 만큼의 bar. 층층이 쌓여 louver(시준)처럼 작동.
- **차단 판정**: 광선 `x(z)=x0+z·tanθ` 가 어떤 BM 사각형과 겹치면 차단.
  모든 bar를 통과해 윈도우 상면 도달 시 출사. 인접 Privacy Blue의 BM2/BM3에 의한
  **Normal Green 차폐**가 자연히 포함됨.
- **굴절**: 윈도우(n≈1.5, 조정 가능) → 공기 Snell `sinθair = n·sinθint`.
  `n·sinθint > 1` 이면 전반사로 출사 불가(내부 임계각 ≈ 41.8°).
- **손해 시작각**: 투과율이 임계%(기본 95%) 아래로 내려가는 최소 |θair|.
  반치각(50%), 완전차단각(0%)도 함께 산출. 좌/우 비대칭 시 더 빨리 손해보는 쪽 채택.

## 분석 시나리오
1. **Normal Green vs Privacy Blue**: 대상=Green일 때가 기준. 다른 subpixel은 기준 대비
   손해(°)를 우측 패널에 표시하고, 단면에서 차단광선을 빨간색으로 강조.
2. **Sky Blue 유무**: 우측 표에서 Sky 없음/있음의 Green 손해시작각·반치각·Δ를 직접 비교.
3. **PPI별 요약**: 499/566/610 각각의 Green/Blue 손해시작각 자동 표.

## 개선안 (시야각 동일화)
1. **픽셀 배열 변경** — `Blue계열 lateral shift` 슬라이더. R-R / G-G 등간격은 유지,
   Blue·SkyBlue만 비대칭 이동(요청 제약 반영).
2. **PDL Gap 수정** — B-G / R-G Gap 개별 슬라이더.
3. **BM1/BM2 높이·선폭 조정** — 각 BM z위치·두께·선폭 슬라이더.
4. **BM4 추가** — z위치·두께·선폭 지정하여 삽입/삭제.
- **[시야각 매칭 자동탐색]** 버튼: BM2/BM3 선폭과 Blue shift를 스윕하여
  Sky-on Green 손해시작각을 Sky-off 기준에 맞추는 파라미터를 추천.

## 가정 / 한계 (실측값으로 교체 권장)
- **Red Anode 14.6 µm**: 미제공 추정값(Blue > Red > Green). 좌측 슬라이더로 교정.
- **윈도우 n = 1.5**: 대표값. 실제 윈도우/OCA 굴절률로 조정.
- 단면을 1D subpixel 행으로 추상화(Diamond의 대각 배치는 평면도에서만 표현).
  반복단위: Sky off = `R G B G`, Sky on = `R G B G R G SkyB G`.
- BM1은 9.5µm를 **bottom 기준**으로 해석. center 기준이면 `zLayers()` 한 줄만 수정.
