# GMP 실태조사 Dashboard - Streamlit Cloud + Supabase Storage

## 구성

- `streamlit_app.py`: 기존 `gmp_dashboard_rebuild_v13.py` 기반 Cloud 배포용 앱
- `gmp_topic_learning_rules_v13.json`: 반복 지적사항/체크리스트 주제 보정 규칙
- `requirements.txt`: Streamlit Cloud Python 의존성
- `packages.txt`: 한글 PDF 폰트용 시스템 패키지
- `.streamlit/config.toml`: 기본 화면 설정
- `.streamlit/secrets.toml.example`: Streamlit Cloud Secrets 예시

## Supabase 준비

1. Supabase 프로젝트 생성
2. Storage에서 bucket 생성
   - 권장 bucket 이름: `gmp-dashboard`
   - private bucket 권장
3. Project Settings > API에서 아래 값 확인
   - Project URL
   - service_role key

## Streamlit Cloud Secrets

Streamlit Cloud 앱 설정의 Secrets에 아래 형식으로 입력합니다.

```toml
SUPABASE_URL = "https://xxxxxxxxxxxxxxxxxxxx.supabase.co"
SUPABASE_SERVICE_ROLE_KEY = "여기에 Supabase service_role key 입력"
SUPABASE_BUCKET = "gmp-dashboard"
SUPABASE_OBJECT = "latest.xlsx"
SUPABASE_META_OBJECT = "latest_meta.json"
ADMIN_PASSWORD = "관리자업로드비밀번호"
```

`SUPABASE_SERVICE_ROLE_KEY`와 `ADMIN_PASSWORD`는 GitHub에 올리면 안 됩니다.

## GitHub 업로드

이 폴더 안의 파일 전체를 새 repository에 업로드합니다.

Streamlit Cloud 배포 시 main file path는 아래로 지정합니다.

```text
streamlit_app.py
```

## 사용 방법

1. 앱 접속
2. 사이드바 `Admin Upload` 열기
3. `ADMIN_PASSWORD` 입력
4. 선생님이 작성한 GMP 실태조사 엑셀 파일 업로드
5. `업로드하고 latest.xlsx로 반영` 클릭
6. 이후 새 파일 업로드 전까지 모든 접속자는 Supabase Storage의 `latest.xlsx` 기준으로 Dashboard와 Analysis Report를 조회합니다.

## 주의

- 엑셀 원본은 정제하지 않고 기존 앱의 `load_workbook()`/`clean_sheet()` 로직으로 읽습니다.
- 업로드 시 최소 검증 컬럼은 `제조업체명`, `감시분야`, `등급`, `지적사항 요약`입니다.
- 최초 배포 후 latest.xlsx가 없으면 대시보드는 표시되지 않고 Admin Upload 안내가 표시됩니다.
