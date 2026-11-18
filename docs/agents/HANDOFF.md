# HANDOFF

Claude가 Antigravity(agy)와 리뷰 서브에이전트에 맡긴 검증 기록. 새 항목을 맨 위에 추가한다. 항목 형식은 공통 스킬 `handoff` 를 따른다.

---

### [2026-11-18 22:00] Claude → Antigravity · HuliacDev Template 26.11.18-1 반영 (main)
- 요청: Template에서 숨은 종료 버튼을 빼고, Alt+F4로 끈 앱도 자동 재시작(`F` 키로 함께 끄고 켬)하며 시작·종료 로그 문구를 바꾼 뒤 0_Startup을 다시 git 참조로 되돌리기.
- 변경: `packages-lock.json` 고정 커밋 9f34e0c → b741851(26.11.18-1). `Settings.json` 에서 `closeSetting` 삭제. README의 설정 파일·자동 재시작·단축키 설명과 CHANGELOG 갱신. Template 작업 중에는 manifest를 `file:../../Template`·`testables` 로 바꿨다가 원래대로 되돌림.
- Claude 확인: 열려 있는 Editor를 건드리지 않도록 프로젝트 사본을 배치 모드로 열어 Template을 git에서 받고 lock을 만듦(고정 커밋이 머지 커밋과 같음), 컴파일 오류 0. 그 lock을 0_Startup에 옮김. Template 검증 결과(PlayMode 187/187, 교차 리뷰)는 Template `docs/agents/HANDOFF.md` 에 있음.
- agy (gemini-4-argon-high): `Settings.json` 유효성·lock 한 줄 변경, README·CHANGELOG·TODO 문구와 형식 → 통과
- 남은 것: 열려 있는 Editor는 다음에 포커스를 받을 때 새 패키지를 받아 컴파일함. 실제 빌드에서 Alt+F4 재시작·`F` 키 표시는 확인하지 않음.

### [2026-11-18 15:41] Claude → Antigravity + 서브에이전트 · Unity Cloud 연결 값 비우기 (chore/clear-cloud-project-link)
- 요청: 프로젝트 설정에 든 조직 ID를 새 프로젝트가 물려받지 않게 비우기.
- 변경: ProjectSettings.asset 의 `cloudProjectId`·`projectName`·`organizationId` 를 Unity 빈 문자열 형식(`key: `)으로 비움, Version 26.11.18.
- Claude 확인: 배치 모드로 열고 닫아 return code 0, error CS 0, 설정 파일이 다시 쓰이지 않음. 다른 추적 파일에 옛 값 없음(git grep). Template 패키지 코드는 이 값을 쓰지 않고, Addressables는 CCD에서만 쓰는데 이 프로젝트에는 Addressables 설정이 없음.
- 교차 검증 (agy gemini-4-argon-high + 서브에이전트, 1묶음):
  - 빈 값 형식·다른 줄 변경 없음, bundleVersion 규칙 → 둘 다 통과
  - 비웠을 때 영향(cloudEnabled 0, UnityConnectSettings 서비스 모두 꺼짐, 서비스 패키지·사용 코드 없음) → 둘 다 통과
  - agy는 첫 요청이 5분 시간 제한에 걸려 빈 결과 → 항목을 둘로 쪼개 다시 맡겨 통과
- 서브에이전트 (1묶음):
  - 재검증(11-18): 머지 전 문서 묶음(TODO·HANDOFF의 개인 정보·설정 diff와 사실 일치·형식) → 통과
- 남은 것: 옛 값은 git 기록에 남음(비밀값이 아니라 기록은 다시 쓰지 않음). 배치 모드는 로그인 없이 열려, 로그인한 Editor에서 열 때 다시 채워지지 않는지는 확인하지 않음.

### [2026-11-03 12:35] Claude → Antigravity + 서브에이전트 · 회사 기술 문서 10장 기준 맞추기 (chore/bible-folder-structure)
- 요청: 폴더 구조를 Assets/_Project 기준으로 옮기고, 새 프로젝트용 README 틀을 두고, 자동 재시작·Build 메뉴가 들어간 Template으로 올리기.
- 변경: Scenes·Scripts·Prefabs·Input·Settings와 루트 설정 에셋 2개를 Assets/_Project 아래로(Unity 안에서 이동), 빈 UI 폴더 삭제, Art·Audio·Video에 .gitkeep. `README.project.md` 추가와 README·CLAUDE.md 틀 갱신. Template 26.11.3-2와 NuGet 의존성 업데이트, Settings.json `autoRestart` 켬, Version 26.11.3.
- Claude 확인: 이동 뒤 GUID 유지(이름 바꾸기 R100), VContainer Root Lifetime Scope·Preloaded Assets·Graphics·Quality·URP Global Settings·전역 입력 액션·빌드 씬이 새 경로를 가리킴, 콘솔 오류 0, Windows 빌드 성공(이동 직후, 패키지 업데이트 뒤 각 1번, 빌드 부산물은 되돌림). UPM ZLogger.Unity 2.6.0 + NuGet ZLogger 2.5.10 조합은 로거 생성은 됐지만 README의 지원 조합에 맞춰 둘 다 2.6.0으로 올림.
- 교차 검증 (agy gemini-4-argon-high + 서브에이전트, 1묶음):
  - 폴더 이동(GUID·설정 참조·고아 .meta·그대로 둘 폴더·생성 코드 변경 범위) → 둘 다 통과
- 서브에이전트 (1묶음):
  - 문서·설정 → 지적(낮음): 재시작 간격이 최대 10분까지 늘어난다는 설명 없음, 강제 종료 뒤 다시 켜지는 점을 Breaking에 적지 않음, UPM·NuGet ZLogger 짝이 어긋남 → 모두 고침
- agy (gemini-4-argon-high, 1묶음): 문서·설정 최종본 → 지적(낮음): README의 Build 메뉴 설명에 복사 폴더를 비우고 최신만 둔다는 말이 없음 → 고침. 설정 JSON·패키지 버전 짝·개인 경로 → 통과
- 남은 것: 사용자가 NuGet 창의 Update All로 올린 결과는 `etc: 사용자 변경` 커밋으로 따로 남김(의존성 3개가 내려간 상태였고, 이어서 메뉴로 다시 올림).

### [2026-10-16 10:59] Claude → Antigravity · HuliacDev Template 26.10.16-1 업데이트 (main)
- 요청: Template을 최신으로 올리고, 바뀐 내용과 이 프로젝트에 걸리는 것이 있는지 확인.
- 변경: packages-lock.json 고정 커밋 1c5e127(26.10.12-1) → a036657(26.10.16-1, 항상 위 옵션 `alwaysOnTop`). Settings.json에 `"alwaysOnTop": false` 추가, bundleVersion 26.10.16, CHANGELOG에 10-12·10-16 운영자 영향 항목 추가.
- Claude 확인: 배치 모드로 a03665700c 패키지 받기·스크립트 컴파일, error CS 0, return code 0. 프로젝트 코드는 `ConfigureWindowFocus`·`ConfigureProjectMessageBrokers`·`GetTextWithRetryAsync` 를 override·호출하지 않음.
- agy (gemini-3.8-flash-high, 2묶음):
  - 프로젝트 코드와 바뀐 Template API 충돌, Settings.json 키 이름·기본 동작 → 통과
  - CHANGELOG 사실 일치, 배치 모드 로그, lock 해시·bundleVersion·README → 통과(로그 근거 줄 440·866 직접 확인)
