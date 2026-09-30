# site-ops-log

13개 사이트 일일 통합관리(저장소 점검 + SEO 최적화) 자동화 루틴의 실행 결과를 구글시트("통합관리로그" 탭)로 전달하기 위한 중계 저장소입니다.

## 동작 방식

1. 통합관리 클라우드 루틴이 실행을 마치면 `reports/YYYY-MM-DD.json` 파일을 이 저장소에 커밋·푸시합니다. (클라우드 샌드박스는 GitHub 외 외부 사이트 접속이 막혀 있어 구글시트에 직접 쓸 수 없기 때문입니다.)
2. `reports/*.json` 경로에 push가 발생하면 `.github/workflows/log-to-sheet.yml` 워크플로가 실행되어, 해당 파일 내용을 구글 Apps Script 웹앱으로 전달합니다.
3. Apps Script가 `type: "seo_log"` 페이로드를 받아 "통합관리로그" 탭에 한 줄씩 기록합니다.

## reports/*.json 스키마

```json
{
  "type": "seo_log",
  "date": "2026-10-01",
  "entries": [
    {"site": "1등급만들기", "status": "applied", "category": "C", "detail": "FAQPage 구조화 데이터 추가", "commit": "abc1234"},
    {"site": "탄탄과외", "status": "skipped", "category": "", "detail": "오늘 변경 필요 없음", "commit": ""}
  ],
  "flagged": ["easyiil: 이미지 alt 누락 확인 필요 (수동 검토)"]
}
```

## 필요한 저장소 설정

Settings → Secrets and variables → Actions 에 `APPS_SCRIPT_URL` 시크릿을 등록해야 워크플로가 동작합니다 (값: 기존 12개 사이트 신청폼이 쓰는 것과 동일한 Apps Script `/exec` URL).
