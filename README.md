# karpathy-skills

Andrej Karpathy 스타일 코딩 규율을 Claude Code의 글로벌 Skill로 패키징한 것.

> **한 줄로**: 모든 프로젝트에서 Claude가 *명확화 → 최소코드 → 외과적수정 → 검증* 4단계를 따르게 한다.

---

## 무엇이 들어있나

```
skills/
└── karpathy/
    └── SKILL.md      ← 4단계 체크리스트 본체
```

| 단계 | 핵심 |
|---|---|
| 1. FRAME | 코드 쓰기 전 요구·가정·불확실성·완료조건 표면화 |
| 2. WRITE | 최소 코드, 투기적 추상화 차단, "삭제 테스트" |
| 3. EDIT  | 무관한 줄 건드리지 않기, diff 라인 수 사전 선언 |
| 4. VERIFY | GOAL/DONE WHEN/VERIFY BY 선언, 3회 실패 시 보고 |

자세한 내용은 [`skills/karpathy/SKILL.md`](skills/karpathy/SKILL.md) 참조.

---

## 설치 (글로벌 적용)

Claude Code의 글로벌 skills 디렉토리(`~/.claude/skills/`)로 배포하면 모든 프로젝트에서 자동 발동된다.

### 옵션 A: 단순 복사

**Windows (PowerShell):**
```powershell
Copy-Item -Recurse -Force `
  .\skills\karpathy `
  $env:USERPROFILE\.claude\skills\karpathy
```

**macOS / Linux:**
```bash
mkdir -p ~/.claude/skills
cp -R ./skills/karpathy ~/.claude/skills/karpathy
```

장점: 간단. 단점: 수정할 때마다 다시 복사해야 함.

### 옵션 B: 심볼릭 링크 (권장)

**Windows (관리자 권한 PowerShell):**
```powershell
New-Item -ItemType SymbolicLink `
  -Path  $env:USERPROFILE\.claude\skills\karpathy `
  -Target (Resolve-Path .\skills\karpathy)
```

**macOS / Linux:**
```bash
mkdir -p ~/.claude/skills
ln -s "$(pwd)/skills/karpathy" ~/.claude/skills/karpathy
```

장점: 1소스 자동 동기화. 이 repo에서 편집하면 바로 반영.
참고: macOS/Linux는 관리자 권한 불필요. Windows만 관리자 권한 또는 개발자 모드 필요.

---

## 사용

설치 후 별도 동작 불필요. Claude가 다음 상황에서 자동 발동한다:

- 모호한 지시 ("구현해줘", "고쳐줘")
- 새 추상화·클래스·헬퍼·래퍼 추가 직전
- 기존 파일 수정 시
- 다단계 작업의 시작

명시 호출도 가능:

```
/karpathy
```

---

## 비활성화

별도 명령 없음. Skill은 Claude가 호출해야 발동되는 구조이므로, 호출되지 않으면 영향 없다.

특정 프로젝트에서 다른 정책을 쓰고 싶으면 해당 프로젝트의 `CLAUDE.md`에 명시한다 — 프로젝트 규칙이 skill보다 우선한다.

---

## 설계 원칙

- **1 skill, 4 phase, 0 친구** — 5개로 쪼개지 않고 하나로 통합. 기억할 이름은 `karpathy` 하나.
- **순서로 경계 구분** — 단계가 시간순(1→4)이라 트리거 충돌 없음.
- **opt-in 호출 모델** — off 스위치 불필요. 호출 안 하면 그게 off.
- **프로젝트 우선** — 글로벌 규율이 프로젝트 정책을 덮지 않는다.

---

## 유지보수

- 본 repo가 single source of truth.
- 수정은 `skills/karpathy/SKILL.md`에서. 옵션 B로 설치했다면 자동 반영.
- description 표현이 자동 발동에 영향이 크다 — 오발동/누락 발견 시 frontmatter `description`을 먼저 조정한다.
