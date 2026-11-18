# 0_Startup

전시·키오스크형 Unity 앱을 빠르게 시작하기 위한 프로젝트 템플릿입니다. 자주 쓰는 패키지와 설정, 공통 매니저 연결이 미리 들어 있습니다.

## 주요 기능

- 스택 패키지와 공통 코드 [HuliacDev Template](https://github.com/wonjeong97/Template)이 설치·연결돼 있어, 필요한 파일만 복사하면 바로 개발을 시작합니다.
- DI 루트 `Assets/_Project/Prefabs/GameLifetimeScope.prefab` 이 VContainerSettings의 Root Lifetime Scope라 씬에 두지 않아도 자동으로 생기며, GameManager, APIManager, InactivityTimer(무입력 복귀), ShutdownScheduler(예약 종료), Reporter(로그 뷰어)가 들어 있습니다.
- 현장 설정(`Settings.json`, `ShutdownSettings.json`)을 빌드한 뒤에도 고칠 수 있고, Windows 빌드는 크래시·강제 종료·응답 없음 뒤 자동으로 다시 켜집니다.
- 에디터 메뉴로 스택 패키지를 한 번에 올리고(`Tools > Update Stack Packages`), Windows 빌드를 만들어 zip으로 보관·복사합니다(`Build > Windows Build`).
- 새 프로젝트용 README 틀(`README.project.md`)과 프로젝트 설명 틀(`CLAUDE.md`, Claude Code용)이 들어 있습니다.

## 기술 스택

- Unity 2022.3.62f3
- 렌더 파이프라인: URP 14 (UI 위주 성능 설정 URP-Performant, 카메라 후처리 꺼짐. 3D를 쓰면 High Fidelity와 후처리를 켭니다)
- 구조: [VContainer](https://github.com/hadashiA/VContainer)(DI), [UniTask](https://github.com/Cysharp/UniTask)(async), [MessagePipe](https://github.com/Cysharp/MessagePipe)(Pub/Sub), [R3](https://github.com/Cysharp/R3)(리액티브)
- 유틸: [ZLogger](https://github.com/Cysharp/ZLogger)(로깅), [ZString](https://github.com/Cysharp/ZString)(문자열), [DOTween](https://dotween.demigiant.com/)(트윈)
- 공통 코드: [HuliacDev Template](https://github.com/wonjeong97/Template)(`com.huliacdev.template`) — GameManagerBase, 무입력 복귀, 자동 종료, UI·Fade·Sound·Video 매니저, API·Arduino 매니저, 로그 보관
- Unity 패키지: Input System, Addressables, TextMeshPro, Timeline, Visual Scripting, Test Framework, Memory Profiler, Profile Analyzer
- 개발 도구: [NuGetForUnity](https://github.com/GlitchEnzo/NuGetForUnity), [MCP for Unity](https://github.com/CoplayDev/unity-mcp)(Claude Code 연동)

## 폴더 구조

프로젝트에서 만드는 에셋은 `Assets/_Project/` 아래에 종류별로 두고, 외부에서 받은 것과 도구가 위치를 정해 둔 폴더는 `Assets/` 바로 아래에 그대로 둡니다.

```
Assets/
├── _Project/
│   ├── Scenes/           # 씬 (0_Idle.unity가 첫 씬)
│   ├── Scripts/          # 스크립트, 기능별 하위 폴더 (App, Core, Network …)
│   ├── Prefabs/          # 프리팹 (GameLifetimeScope.prefab, UI 프리팹)
│   ├── Art/              # 이미지, 3D 모델, 머티리얼 (UI 이미지는 Art/UI)
│   ├── Audio/            # 사운드
│   ├── Video/            # 영상 (교체할 영상은 StreamingAssets)
│   ├── Input/            # 프로젝트 입력 액션과 생성된 C# 클래스
│   └── Settings/         # URP 에셋, URP Global Settings, VContainerSettings
├── Plugins/              # 외부 플러그인 (DOTween, Roslyn — Editor 전용)
├── StreamingAssets/      # 설정 파일, 교체 가능한 영상·데이터 (Settings.json, ShutdownSettings.json)
├── Packages/             # NuGet 패키지 (NuGetForUnity가 이 위치를 씀)
├── Resources/            # DOTweenSettings (DOTween이 이 위치를 씀)
├── TextMesh Pro/         # TMP 기본 리소스 (TMP가 이 위치를 씀)
├── csc.rsp               # 컴파일러 옵션 (Assets 바로 아래에 있어야 함)
└── NuGet.config, packages.config  # NuGetForUnity 설정 (Assets 바로 아래에 있어야 함)
Packages/
├── manifest.json       # 패키지 목록
└── packages-lock.json  # 패키지 버전 고정
```

- 빈 폴더(`Art`, `Audio`, `Video`)의 `.gitkeep` 은 git이 빈 폴더를 저장하지 않아 clone한 쪽에서 Unity가 폴더의 `.meta` 를 지우는 것을 막습니다. 에셋으로 보이지 않으며, 폴더에 파일을 넣은 뒤에는 지워도 됩니다.
- 에셋은 Unity Editor 안에서 옮깁니다. 탐색기에서 옮기면 `.meta` 가 따라가지 않아 씬·프리팹의 참조가 끊길 수 있습니다.

## 사용법 및 설정

- 준비물: Unity 2022.3.62f3, Git(일부 패키지를 Git 주소로 받습니다)
- 템플릿 폴더: `git clone https://github.com/wonjeong97/0_Startup.git` 으로 한 번 받아 두고, 이 폴더에서는 작업하지 않습니다. 새 프로젝트를 시작하기 전에 `git pull` 로 최신으로 맞춥니다(Code > Download ZIP으로 받은 폴더는 다시 받아야 최신이 됩니다).
- 새 프로젝트 시작: 새 프로젝트는 템플릿의 git 기록 없이 시작합니다.
  1. 템플릿 폴더에서 `Assets/`, `Packages/`, `ProjectSettings/`, `.gitignore`, `.gitattributes`(큰 이미지·영상은 Git LFS로, DLL·서드파티 파일은 일반 파일로 저장하는 규칙), `CLAUDE.md`, `README.project.md` 일곱 개만 새 폴더로 복사합니다. `Library`, `.git` 등 나머지는 복사하지 않습니다.
  2. Unity Hub의 **Add > Add project from disk** 로 새 폴더를 엽니다. 처음 한 번은 패키지를 받느라 몇 분 걸립니다.
  3. **Edit > Project Settings > Player** 의 **Product Name** 을 새 프로젝트 이름으로 바꿉니다. 앱 로그 폴더와 PlayerPrefs 위치가 Company Name·Product Name으로 정해지므로, 그대로 두면 같은 PC의 다른 앱과 섞입니다.
  4. `README.project.md` 의 이름을 `README.md` 로 바꾸고 칸을 채웁니다. Unity 버전을 올리면 README의 Unity 버전도 함께 고칩니다.
- 패키지 업데이트: 이미 시작한 프로젝트에는 템플릿 변경이 자동으로 들어가지 않습니다. 공통 코드와 스택 패키지(NuGet 패키지 포함)는 `Tools > Update Stack Packages` 로 올리고, 그 밖의 변경은 필요한 파일만 골라 옮깁니다. 버전을 고정한 URP·Input System·Addressables와 `Assets/Plugins` 의 DOTween은 이 메뉴가 건드리지 않습니다. NuGet 창의 Update All은 일부 의존성을 오히려 내릴 수 있어 쓰지 않습니다.
- 설정 파일: `Assets/StreamingAssets/Settings.json`(무입력 복귀, 프레임레이트, 창 포커스 복구·항상 위, 자동 재시작, API 주소)과 `ShutdownSettings.json`(요일별 자동 종료). `Settings.json` 키 설명은 [HuliacDev Template 설정 안내](https://github.com/wonjeong97/Template/blob/main/docs/SETTINGS.md), `ShutdownSettings.json` 은 [예약 종료](https://github.com/wonjeong97/Template/blob/main/docs/USAGE.md#예약-종료-shutdownscheduler)에 있습니다.
- 자동 재시작: Windows 빌드가 크래시하거나 강제로 꺼지거나(`taskkill /F` 등) 60초 넘게 '응답 없음'이거나 Alt+F4·창 닫기로 꺼지면 5초 뒤 다시 켜고, 예약 종료로 끈 경우는 켜지 않습니다. 점검하려고 앱을 끌 때는 `F` 키로 Alt+F4 재시작을 끈 뒤(화면 위쪽에 OFF 표시) Alt+F4를 누릅니다. 현장 PC에 따로 설치할 것은 없고 `Settings.json` 의 `autoRestart` 로 끄거나 시간을 바꿉니다(`F` 로 끄고 Alt+F4로 끈 뒤 다시 켜야 반영). 기록은 앱 로그 폴더(`%USERPROFILE%\AppData\LocalLow\<Company Name>\<Product Name>\Logs`)의 `AutoRestart.log` 에 남고, 자세한 동작은 [HuliacDev Template 사용 안내](https://github.com/wonjeong97/Template/blob/main/docs/USAGE.md#자동-재시작-autorestartwatchdog-windows-전용)에 있습니다.
- 빌드: `Build > Windows Build` 가 Windows 빌드를 `Build/` 에 만들고, 버전 관리용 zip을 빌드 보관 폴더의 `<프로젝트 폴더 이름>/<YYMMDD>-<N>/` 에 보관하고(빌드마다 새 폴더), 현장 PC로 옮길 빌드를 복사 폴더의 `<프로젝트 폴더 이름>/` 에 복사합니다(복사할지는 '빌드 뒤 복사' 설정, 기본은 매번 묻기. 비운 뒤 최신 빌드만 둠). 두 폴더는 팀원·PC마다 다르므로 각자 `Edit > Preferences > HuliacDev Build`(또는 `Build > Setting`)에서 정하며, 처음 빌드할 때 정해져 있지 않으면 설정을 열지 묻습니다.
- 자동 종료 설정 프로그램: `ShutdownScheduleEditor.exe` 는 약 150MB라 저장소에 없습니다. 필요하면 `Tools > Build Shutdown Scheduler Exe` 로 만듭니다(.NET 8 SDK 필요).
- Roslyn: `Assets/Plugins/Roslyn` 은 MCP for Unity 전용이라 Editor 전용으로 설정돼 있습니다. 폴더를 지우고 다시 설치하면 Inspector에서 Editor만 다시 체크합니다.
- 단축키(템플릿 기본): `D` 로그 뷰어, `I` 런타임 인스펙터, `M` 마우스 커서 표시, `F` 창 포커스 복구(와 항상 위, Alt+F4 재시작) 끄기/켜기
- 다국어: 없음
