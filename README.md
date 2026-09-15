# 두데이식스 DO DAY 6IX — 소개 페이지 (v2)

영어학원 원장님을 대상으로 한 학습 관리 프로그램 소개 페이지입니다.
빌드 과정 없이 `index.html` 하나로 동작합니다.

## v2에서 바뀐 것

| 항목 | v1 | v2 |
|---|---|---|
| 문서 구조 | `<!doctype>` · `<head>` · `<body>` 없음 | 완전한 HTML 문서 |
| 인코딩 | `<meta charset>` 없음 (한글 깨질 수 있음) | UTF-8 명시 |
| 모바일 | viewport 태그 없음 | 반응형 정상 동작 |
| 브랜드 | 투데이식스 / TODAY6 / 두데이식스 혼재 | **두데이식스 · DO DAY 6IX** 로 통일 |
| 공유 | 없음 | 카카오톡·네이버 공유 미리보기 (OG 태그 + 이미지) |
| 파비콘 | 없음 | 로고의 `6` 그라데이션 |
| 검색 | 없음 | description, JSON-LD 구조화 데이터 |

v1은 `<head>`가 아예 없어서 브라우저로 바로 열면 한글이 깨지거나
모바일에서 PC 화면 그대로 축소돼 보일 수 있었습니다. v2는 그 문제가 없습니다.

## 파일

```
index.html            페이지 본체 (로고 포함, 이것만 있어도 열림)
logo.png              원본 워드마크 960×180
favicon.png           브라우저 탭 아이콘 512×512
apple-touch-icon.png  아이폰 홈 화면 아이콘 180×180
og-image.png          카톡·네이버 공유 썸네일 1200×630
.nojekyll             GitHub Pages 설정 (지우지 마세요)
README.md             이 문서
```

## GitHub Pages로 공개하기

1. GitHub에서 새 저장소(repository)를 만듭니다 — Public
2. 이 폴더의 **파일 7개를 전부** 드래그해서 올립니다
3. 저장소 → **Settings → Pages**
4. Source를 **Deploy from a branch**, 브랜치를 **main / (root)**
5. 1~2분 뒤 `https://<아이디>.github.io/<저장소명>/` 에서 열립니다

터미널을 쓰신다면:

```bash
git init
git add .
git commit -m "두데이식스 소개 페이지 v2"
git branch -M main
git remote add origin https://github.com/<아이디>/<저장소명>.git
git push -u origin main
```

## 배포 직후 꼭 고칠 것 한 곳

주소가 나오면 `index.html` 상단의 `https://USERNAME.github.io/REPO/` 4곳을
실제 주소로 바꿔 주세요. **카카오톡에 링크를 붙였을 때 썸네일이 뜨는지**가
여기서 갈립니다. 파일 안에 `★` 표시로 찾기 쉽게 주석을 달아 뒀습니다.

## 아직 임시값인 것

- **문의 메일 `hello@today6.com`** — `index.html`에서 `★ 문의처` 주석으로 표시해 뒀습니다.
  전화로 바꾸시려면 `href="tel:01012345678"` 형태로 쓰시면 됩니다.
- **후기 10건** — 실제 도입 학원 후기로 교체 예정

## 로고

`DO DAY 6IX` 워드마크는 원본 이미지를 그대로 씁니다. 하프톤 점 텍스처와
오프셋 그림자가 들어간 그래픽이라 웹폰트로는 글자꼴이 재현되지 않습니다.
`index.html` 안에 data URI로 박혀 있어 히어로와 상단 내비가 같이 씁니다.

로고를 바꾸려면 `logo.png`를 교체한 뒤 base64로 다시 인코딩해
`:root`의 `--logo` 값만 갈아 끼우면 됩니다.

```bash
base64 -i logo.png | tr -d '\n' | pbcopy
```

## 구성

| 섹션 | 내용 |
|---|---|
| 메인 | DO DAY 6IX 워드마크 |
| 프로그램 | 6개 프로그램을 한 화면에 하나씩, 스크롤 연동 3D |
| 요약 | 여섯 가지를 묶은 결론 |
| 실제 후기 | 가로로 흐르는 후기 카드 |
| 도입 효과 | 원장님이 덜어내는 일 6가지 |
| 도입 절차 | 4단계 |

### 프로그램 순서

```
01 레벨테스트    학습
02 메일보카      학습
03 그래머헌터    학습
04 리딩원정대    학습
05 보상 시스템   관리
06 일일보고서    관리
```

## 외부 의존성

전부 CDN에서 불러옵니다. 저장소에 넣을 파일이 따로 없습니다.

- three.js r128 — 등각 3D 오브젝트
- GSAP + ScrollTrigger 3.12.5 — 패널 등장
- Google Fonts — Plus Jakarta Sans, Noto Sans KR, JetBrains Mono

WebGL을 쓸 수 없는 환경에서는 각 프로그램의 SVG 그림으로 자동 대체됩니다.
CDN이 막힌 망에서도 글과 SVG는 그대로 나옵니다 — 동작 확인 완료.

## 수정할 때

### 프로그램 순서 바꾸기

`<section class="panel">` 블록을 원하는 순서로 옮기고, 블록 안의 번호 네 곳
(`프로그램 0N / 06`, `data-panel`, `panel-mark`의 번호, `obj-meta`의 번호)과
요약 섹션의 칩 순서를 맞추면 됩니다.

3D는 순번이 아니라 패널의 `data-obj` 속성으로 연결되어 있어서
(`result` `cards` `console` `trail` `gift` `phone`) 섹션을 옮겨도 따라옵니다.

### 색 바꾸기

`:root`의 토큰 네 개만 고치면 3D를 포함해 전체가 따라옵니다.

```css
--brand:#2A8BD4;        /* 선·채움·3D 포인트 */
--brand-deep:#1B6FAF;   /* 텍스트·버튼 (흰 바탕 대비 5.33) */
--brand-soft:#EDF5FC;   /* 옅은 바탕 */
--brand-line:#B9D9F1;   /* 옅은 테두리 */
```

로고 `6`의 그라데이션(보라 → 청록)에서 파랑 구간을 뽑아 채도를 올린 값입니다.
`--brand`는 그래픽용이라 대비 3.66이면 충분하고, `--brand-deep`은 본문 텍스트와
흰 글자 배경에 쓰이므로 양방향 5.33으로 잡아 WCAG AA를 넘깁니다.

3D 오브젝트의 포인트 색은 `var ACC = 0x3A96DC;` 입니다.
