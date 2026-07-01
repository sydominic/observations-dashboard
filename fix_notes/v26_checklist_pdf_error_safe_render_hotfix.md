# v26 Checklist PDF Error Safe Render Hotfix

## 수정 대상
- Analysis Report 탭의 내부 감시용 Checklist PDF 생성 오류

## 원인 보강
- 내부 감시용 Checklist는 원문 지적사항/선정사유를 Matplotlib PDF 텍스트로 직접 렌더링합니다.
- 일부 원문 특수문자(`$`, `\\`, 제어문자 등)가 Matplotlib mathtext 처리와 충돌할 수 있어 특정 기간 선택 시 PDF 생성이 중단될 수 있습니다.

## 반영 내용
- PDF 렌더링용 안전 문자열 처리 함수 추가
  - `$` → 전각 문자로 치환
  - `\\` → 전각 문자로 치환
  - 제어문자 제거
- 내부 감시용 Checklist의 주요 원문/선정사유/대표지적/질문/감시분야 텍스트에 안전 처리 적용
- 숫자 변환 안전 처리 추가
- 내부 감시용 Checklist rich layout 생성 실패 시 간이 Checklist PDF로 대체 생성하는 fallback 추가
- Checklist 캐시 키를 `checklist_v4_safe`로 변경하여 기존 실패 캐시 재사용 방지

## 검증
- `python -m py_compile streamlit_app.py` 통과
- 2026-1Q 테스트 데이터로 내부 감시용 Checklist PDF 생성 확인
- 특수문자 `$`, `\\` 포함 원문 데이터로 내부 감시용 Checklist PDF 생성 확인
- One page Checklist PDF 생성 확인
