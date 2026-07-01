# Letter PDF 자동생성 양식 변경

## 변경 대상
- `streamlit_app.py`
- `build_report_pdf()`
- Report 탭 다운로드 안내 문구

## 변경 내용
1. 기존 3페이지 Letter PDF를 4페이지 구조로 변경했습니다.
2. 신규 1페이지를 `2026-2Q_dashboard_report_page1_combined_original_watermark_fixed.pdf` 형식에 맞춰 추가했습니다.
   - KPI 카드: 총 지적, 최다 분야, 2순위 분야, 최다 세부유형
   - 이번 분기 핵심 메시지
   - Track 1~3 권장 점검 방향
   - 권장 실행 순서 4단계
3. 기존 1페이지 장문 트렌드 요약은 2페이지로 이동했습니다.
4. 기존 2페이지 Top5 및 주요 현황은 3페이지로 이동했습니다.
5. 기존 3페이지 감시분야 및 등급 분포 차트는 4페이지로 이동했습니다.
6. 모든 페이지에 기존 중앙 워터마크 생성 로직을 동일하게 적용했습니다.
7. 다운로드 안내 문구를 3페이지에서 4페이지 구조로 수정했습니다.

## 검증
- `python -m py_compile streamlit_app.py` 통과
- 더미 데이터 기준 `build_report_pdf()` 실행 및 4페이지 PDF 생성 확인
