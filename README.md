# GMP 실태조사 Dashboard - Streamlit Cloud + Supabase Storage

## 구성

- `streamlit_app.py`: 기존 `gmp_dashboard_rebuild_v13.py` 기반 Cloud 배포용 앱
- `gmp_topic_learning_rules_v13.json`: 기존 정적 주제 보정 규칙
- Supabase Storage `topic_assignments.json`: 업로드 후 row_id별 확정 주제 저장소. 앱이 자동 생성/갱신합니다.
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
SUPABASE_ASSIGNMENTS_OBJECT = "topic_assignments.json"
SUPABASE_RUNTIME_RULES_OBJECT = "topic_learning_rules_runtime.json"
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
6. 업로드 시 앱은 row_id를 생성하고 `topic_assignments.json`에 기존 확정분류/신규 자동분류 결과를 저장합니다.
7. 이후 새 파일 업로드 전까지 모든 접속자는 Supabase Storage의 `latest.xlsx`와 `topic_assignments.json` 기준으로 Dashboard와 Analysis Report를 조회합니다.

## 주의

- 엑셀 원본은 정제하지 않고 기존 앱의 `load_workbook()`/`clean_sheet()` 로직으로 읽습니다.
- 업로드 시 최소 검증 컬럼은 `제조업체명`, `감시분야`, `등급`, `지적사항 요약`입니다.
- 최초 배포 후 latest.xlsx가 없으면 대시보드는 표시되지 않고 Admin Upload 안내가 표시됩니다.


## v6 자동학습/기존분류 유지 방식

- 대시보드 화면 구성은 기존과 동일합니다.
- 신규행수, 자동분류 완료 수 등은 일반 Dashboard/Report 화면에 노출하지 않습니다.
- row_id는 `등록월 + 제조업체명 + 실사기간 + 유형 + 완제제형 + 실사방식 + 등급 + 감시분야 + 지적사항 요약` 조합을 SHA-256으로 생성합니다.
- 지적 내용이 같아도 회사명, 등록월, 실사기간 등이 다르면 다른 row_id로 처리합니다.
- row_id가 이미 `topic_assignments.json`에 있으면 기존 확정 주제를 그대로 사용합니다.
- row_id가 없으면 기존 Python/JSON 분류엔진으로 한 번 분류하고 `topic_assignments.json`에 저장합니다.
- 따라서 v6 적용 후 첫 업로드가 현재 데이터의 기준선이 됩니다.
