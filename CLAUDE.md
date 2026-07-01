# UE_Development

언리얼 엔진 개발 문서화 프로젝트. 핵심 산출물은 `design-guide/`의 디자인 시스템과 그 기준으로 `ue-wiki/`에 생성하는 문서(HTML)다.

## 폴더 구조
- `design-guide/` — 템플릿·마스터 CSS·규약(편집 대상). `design-guide-ue-master.css`, `design-guide-ue-master-visual.html`, `01-ue-asset-analysis.html`, `02-ue-architecture.html`, `DOC-GENERATION-PLAYBOOK.md`.
- `ue-wiki/ue-asset/` — 에셋 분석 결과 문서 저장.
- `ue-wiki/ue-architect/` — 아키텍처/기능 구조 문서 저장 + 문서 허브(`design-guide-ue-index.html`).

## 문서 제작 요청 시 (필수)

사용자가 언리얼 관련 **설명/분석/설계 문서 제작**을 요청하면(예: "~~ 시스템 구조 문서 만들어줘", "이 에셋 분석해줘", "~~ 기능 구현 문서"), **반드시 `design-guide/DOC-GENERATION-PLAYBOOK.md` 규약을 처음부터 끝까지 따른다.**

규약 요약(자세한 건 playbook 참조):
1. **디자인 소스**: `design-guide/design-guide-ue-master.css`(스타일 기준) + `design-guide-ue-master-visual.html`(시각 배치 참고).
2. **포맷 판별**: 에셋 분석 → `01-ue-asset-analysis.html` 구조 / 아키텍처·기능 구현 → `02-ue-architecture.html` 구조(템플릿 A·B).
3. **갭 발생 시**: 기존 마스터로 표현 불가한 디자인이 필요하면 → `master.css`·`master-visual.html`을 먼저 갱신(버전+1)한 뒤, 갱신된 기준으로 작업을 이어간다.
4. **변경 이력**: 초기 버전과 달라진 점은 각 파일 상단 CHANGELOG 주석에 빠짐없이 기록(형식: 소스 주석 블록).

**작성 원칙(필수)**: 생성하는 모든 문서는 초보자도 혼자 따라 할 수 있도록 용어 풀이·구체 절차(메뉴/노드/경로/클래스 이름)·전제와 결과·근거를 빠짐없이 담는다. "안다고 가정하고 건너뛰기" 금지.

**결과물 출력 방식**: CSS를 `<style>`에 inline 복사(단일 파일·오프라인), C++ 하이라이팅은 hljs `<span>`을 미리 구워넣기, 탭/복사만 inline JS.

**출력 위치**: 에셋 분석 결과 → `ue-wiki/ue-asset/`, 아키텍처/기능 결과 → `ue-wiki/ue-architect/`. 결과물은 CSS를 inline해 단독으로 열린다(상대경로 링크 X).

**문서 허브 갱신(필수)**: 문서를 만들 때마다 `ue-wiki/ue-architect/design-guide-ue-index.html`의 문서 목록에 등록하고, 어려운 용어는 용어집에 추가(용어 설명 위치에 `id="term-…"` 앵커 부여해 링크). 용어는 처음 1회만 풀어 설명하고 이후엔 용어만 쓰되, 핵심 용어 블록·재힌트·한 개념 한 용어 원칙을 지킨다.
