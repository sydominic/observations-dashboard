# v22 Letter/Checklist PDF fix

- Letter PDF 다운로드 시 기간을 바꿔도 1페이지가 이전 기간 값으로 보일 수 있는 위험을 줄이기 위해 PDF 캐시 키에 기간/데이터 지문을 명시적으로 추가했습니다.
- Report, 내부 감시용 Checklist, One page Checklist 캐시 호출 모두 동일하게 보강했습니다.
- Letter 1페이지 4단계 문구는 각 부서 자체 책임과 QA 지원 역할이 드러나도록 조정했습니다.
- 내부 감시용 Checklist PDF 생성 중 일부 환경에서 Matplotlib bbox/clip 속성 호환성 문제로 AttributeError가 발생할 수 있어 순번 박스를 명시적 Rectangle 패치로 변경하고 clip_on 의존성을 제거했습니다.
