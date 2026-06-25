# Eye Care Switchable + Privacy — LVA 시뮬레이터 (Vercel 전용 배포 브랜치)

이 브랜치(`vercel-lva`)는 **앱 파일만 루트에 둔 배포 전용 분리 브랜치**입니다.
`main`의 다른 코드와 섞이지 않으며, Vercel에서 Root Directory를 바꿀 필요 없이
(`./` 그대로) 그대로 배포됩니다.

```
/ (root)
├── index.html   # 시뮬레이터 본체
└── vercel.json  # 정적 호스팅 설정
```

## 배포
1. Vercel 프로젝트 → Settings → Git → **Production Branch** 를 `vercel-lva` 로 지정
   (또는 새 프로젝트 Import 시 이 브랜치 선택)
2. **Root Directory = `./`**, Framework = **Other**, Build 비움
3. Deploy → `https://<프로젝트>.vercel.app`

> 앱은 100% 클라이언트 사이드(순수 HTML/Canvas/JS)라 서버·환경변수가 필요 없습니다.
> 계산 모델/가정 설명은 `main` 브랜치의 `eye-care-lva/README.md` 참고.
