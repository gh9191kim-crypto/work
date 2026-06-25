# Eye Care Switchable + Privacy — LVA 시뮬레이터 (Vercel 배포판)

`eye-care-lva` 와 **동일한 앱**(단일 `index.html`)을 Vercel에 그대로 배포할 수 있게
정적 프로젝트로 패키징한 버전입니다. 빌드 단계 없이 정적 호스팅됩니다.

## 구성
```
eye-care-lva-vercel/
├── index.html     # 앱 본체 (eye-care-lva/index.html 과 동일)
├── vercel.json    # 정적 호스팅 설정 (cleanUrls 등)
└── README.md
```

## 배포 방법

### A. GitHub 연동 (권장)
1. https://vercel.com/new 에서 이 레포(`gh9191kim-crypto/work`)를 Import.
2. **Root Directory** 를 `eye-care-lva-vercel` 로 지정.
3. Framework Preset = **Other**, Build Command/Output 비움(정적).
4. **Deploy** → `https://<프로젝트>.vercel.app` 로 공개.

> Production 브랜치는 기본 `main` 입니다. 이 작업 브랜치
> `claude/eye-care-viewing-angle-ong5mr` 를 배포하려면 Vercel 프로젝트 설정의
> Production Branch 를 바꾸거나, 푸시 시 생성되는 Preview URL 을 사용하세요.

### B. Vercel CLI
```bash
cd eye-care-lva-vercel
npx vercel        # 프리뷰 배포
npx vercel --prod # 프로덕션 배포
```

## 참고
- 앱은 100% 클라이언트 사이드(순수 HTML/Canvas/JS)라 서버/환경변수가 필요 없습니다.
- 계산 모델·파라미터·가정은 상위 폴더 `eye-care-lva/README.md` 와 동일합니다.
