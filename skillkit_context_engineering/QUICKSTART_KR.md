# SkillKit 사용법 완전 가이드

> **이 문서는 SkillKit: Context Engineering을 처음부터 끝까지 사용하는 방법을 설명합니다.**

---

## 목차

1. [개요: 이 Kit이 뭔가요?](#1-개요-이-kit이-뭔가요)
2. [설치 및 초기 설정](#2-설치-및-초기-설정)
3. [터미널 3개 준비하기](#3-터미널-3개-준비하기)
4. [첫 번째 Run 만들기](#4-첫-번째-run-만들기)
5. [Phase 1: Gemini 분석](#5-phase-1-gemini-분석)
6. [Phase 2: Codex 계획](#6-phase-2-codex-계획)
7. [Phase 3: Claude 구현](#7-phase-3-claude-구현)
8. [Phase 4: Gate 실행](#8-phase-4-gate-실행)
9. [완료 및 아카이브](#9-완료-및-아카이브)
10. [문제 해결](#10-문제-해결)

---

## 1. 개요: 이 Kit이 뭔가요?

### 1.1 한 문장 설명
**3개의 AI CLI 터미널(Gemini, Codex, Claude)을 수동으로 조율하여 고품질 개발을 수행하는 워크플로우 Kit입니다.**

### 1.2 왜 필요한가?
```
일반적인 AI 코딩:
  "코드 짜줘" → AI가 대충 짬 → 버그 발생 → 수정 요청 → 반복...

SkillKit 방식:
  스펙 정의 → 제약 확인 → 분석 → 계획 → 구현 → 검증 → 완료
  (각 단계에서 품질 게이트 통과해야 다음으로 진행)
```

### 1.3 핵심 원리
```
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│   GEMINI    │───▶│    CODEX    │───▶│   CLAUDE    │
│  (분석가)    │    │  (계획자)    │    │  (실행자)    │
│             │    │             │    │             │
│ • 스펙 읽기  │    │ • 분석 읽기  │    │ • 계획 읽기  │
│ • 제약 확인  │    │ • 파일 계획  │    │ • 코드 구현  │
│ • 설계 제안  │    │ • 스켈레톤   │    │ • 테스트    │
└─────────────┘    └─────────────┘    └─────────────┘
       │                  │                  │
       ▼                  ▼                  ▼
┌─────────────┐    ┌─────────────┐    ┌─────────────┐
│gemini_      │    │codex_       │    │claude_      │
│analysis.md  │    │plan.md      │    │impl.md      │
└─────────────┘    └─────────────┘    └─────────────┘
```

### 1.4 워크플로우 흐름
```
1. Casefile 작성 (무엇을 할 것인지)
        ↓
2. Gemini 분석 (어떻게 할 것인지)
        ↓
3. Codex 계획 (구체적으로 어떤 파일을)
        ↓
4. Claude 구현 (실제 코드 작성)
        ↓
5. Gate 실행 (품질 검증)
        ↓
6. 완료 또는 수정 반복
```

---

## 2. 설치 및 초기 설정

### 2.1 Kit 설치

```bash
# 방법 1: 이미 프로젝트에 있는 경우
cd /your/project
ls skillkit_context_engineering/
# OPERATOR_GUIDE.md  README.md  checklists/  eval/  gates/  ops/  reports/  skills/  spec/

# 방법 2: 다른 프로젝트에서 복사하는 경우
cp -r /source/skillkit_context_engineering /your/project/
```

### 2.2 환경 변수 설정 (선택사항)

```bash
# ~/.bashrc 또는 ~/.zshrc에 추가
export KIT="skillkit_context_engineering"

# 적용
source ~/.bashrc  # 또는 source ~/.zshrc
```

### 2.3 설치 확인

```bash
# 필수 파일 확인
ls skillkit_context_engineering/spec/
# 00_core.md  01_constraints.md  02_interfaces.md  03_glossary.md  90_change_policy.md

ls skillkit_context_engineering/ops/PROMPTS/
# CLAUDE_IMPLEMENT.md  CODEX_PLAN.md  GEMINI_ANALYZE.md

echo "✓ Kit 설치 완료"
```

---

## 3. 터미널 3개 준비하기

### 3.1 터미널 배치

**화면을 3등분하여 터미널 배치:**

```
┌────────────────────┬────────────────────┬────────────────────┐
│                    │                    │                    │
│   TERMINAL 1       │   TERMINAL 2       │   TERMINAL 3       │
│   ============     │   ============     │   ============     │
│                    │                    │                    │
│   🟢 GEMINI        │   🟡 CODEX         │   🔵 CLAUDE        │
│                    │                    │                    │
│   분석 담당         │   계획 담당         │   구현 담당         │
│                    │                    │                    │
└────────────────────┴────────────────────┴────────────────────┘
```

**macOS iTerm2 사용자:**
```bash
# 터미널 분할: Cmd + D (가로), Cmd + Shift + D (세로)
```

**VS Code 사용자:**
```bash
# View > Terminal > Split Terminal
```

### 3.2 각 터미널에서 프로젝트로 이동

**모든 터미널에서:**
```bash
cd /path/to/your/project
```

### 3.3 각 터미널에서 AI CLI 실행

**Terminal 1 (Gemini):**
```bash
# Gemini CLI 실행 (또는 웹 인터페이스 사용)
gemini
# 또는
# Google AI Studio 웹 열기
```

**Terminal 2 (Codex):**
```bash
# Codex/OpenAI CLI 실행
codex
# 또는
# ChatGPT 웹 열기
```

**Terminal 3 (Claude):**
```bash
# Claude CLI 실행
claude
# 또는
# Claude 웹 열기
```

---

## 4. 첫 번째 Run 만들기

### 4.1 Run 이름 결정

```bash
# 형식: YYYY-MM-DD__설명-하이픈-구분
# 예시:
#   2026-01-02__add-user-auth
#   2026-01-02__fix-login-bug
#   2026-01-02__refactor-api-layer

export RUN_NAME="2026-01-02__add-user-auth"
echo "Run 이름: $RUN_NAME"
```

### 4.2 Run 디렉토리 생성

```bash
# 템플릿에서 복사
cp -r skillkit_context_engineering/ops/RUNS/_TEMPLATE_RUN \
      skillkit_context_engineering/ops/RUNS/$RUN_NAME

# 확인
ls skillkit_context_engineering/ops/RUNS/$RUN_NAME/
# 00_casefile.md  01_inputs/  02_outputs/  03_notes.md
```

### 4.3 Casefile 작성

```bash
# 에디터로 열기
vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md
# 또는
code skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md
```

**반드시 작성해야 할 섹션:**

```markdown
## Metadata
| Field | Value |
|-------|-------|
| **Run ID** | `2026-01-02__add-user-auth` |       ← 실제 Run 이름
| **Created** | `2026-01-02T13:00:00+09:00` |       ← 현재 시간
| **Status** | `OPEN` |
| **Work Type** | `feature` |                        ← feature/bugfix/refactor/docs

## 1. Objective
### 1.1 Primary Goal
Add JWT-based authentication to protect `/api/users/*` endpoints.
← 이 줄을 실제 목표로 교체!!!

### 1.2 Success Criteria
| # | Criterion | Verification Method |
|---|-----------|---------------------|
| 1 | Login returns JWT | curl test |                ← 실제 기준으로 교체
| 2 | Protected routes block without token | curl without auth |

## 3. Scope
### 3.1 In Scope
- [ ] Add login endpoint                           ← 실제 할 일 목록
- [ ] Add token validation middleware

### 3.2 Out of Scope
| Item | Reason |
|------|--------|
| OAuth | Defer to v2 |                            ← 안 할 것 명시

## 4. Constraints
### 4.2 Process Constraints
| Constraint ID | Description | How It Applies |
|---------------|-------------|----------------|
| CON-050 | No secrets | Use env vars |             ← 적용될 제약 조건
```

### 4.4 Casefile 완성 확인

```bash
# 필수 섹션 있는지 확인
grep "## 1. Objective" skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md && \
grep "## 3. Scope" skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md && \
echo "✓ Casefile 기본 섹션 완료"

# 플레이스홀더 남아있는지 확인
grep "\[Write here\]" skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md
# 아무것도 출력 안 되면 OK!
```

---

## 5. Phase 1: Gemini 분석

### 5.1 프롬프트 준비

```bash
# 1. 프롬프트 파일 열기
cat skillkit_context_engineering/ops/PROMPTS/GEMINI_ANALYZE.md

# 2. 전체 내용 복사 (Cmd+A, Cmd+C)

# 3. 텍스트 에디터에서 <RUN_NAME> 치환
#    찾기: <RUN_NAME>
#    바꾸기: 2026-01-02__add-user-auth  (실제 Run 이름)
```

### 5.2 Gemini에 프롬프트 전달

**Terminal 1 (Gemini)에서:**

1. 치환된 프롬프트 전체를 붙여넣기
2. Enter 눌러서 실행
3. Gemini가 분석 작업 수행
4. 결과가 나오면 확인

### 5.3 Gemini 출력 저장

**Gemini가 생성한 내용을 저장:**

```bash
# 방법 1: Gemini가 직접 파일에 저장한 경우
cat skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md

# 방법 2: 출력을 수동으로 복사해서 저장해야 하는 경우
vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md
# Gemini 출력 내용 붙여넣기
# :wq 로 저장
```

### 5.4 Gemini 출력 검증

```bash
# 필수 섹션 확인
echo "=== Gemini 출력 확인 ==="
grep "## 1. Request Summary" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md && echo "✓ 섹션 1"
grep "## 2. Architectural Impact" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md && echo "✓ 섹션 2"
grep "## 3. Constraints Check" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md && echo "✓ 섹션 3"
grep "## 4. High-Level Design" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md && echo "✓ 섹션 4"
grep "## 8. Handoff to Codex" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md && echo "✓ 섹션 8 (Handoff)"

# 제약 조건 확인됐는지
echo ""
echo "=== 확인된 제약조건 ==="
grep "CON-" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md | head -10

# BLOCKER가 있으면 멈춤!
echo ""
echo "=== BLOCKER 확인 ==="
grep -i "BLOCKER.*FAIL\|FAIL.*BLOCKER" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md
# 아무것도 없으면 → 다음 단계로 진행
# 뭔가 있으면 → 해결 후 Gemini 재실행
```

---

## 6. Phase 2: Codex 계획

### 6.1 프롬프트 준비

```bash
# 1. 프롬프트 파일 열기
cat skillkit_context_engineering/ops/PROMPTS/CODEX_PLAN.md

# 2. 전체 내용 복사

# 3. <RUN_NAME> 치환
#    찾기: <RUN_NAME>
#    바꾸기: 2026-01-02__add-user-auth
```

### 6.2 Codex에 프롬프트 전달

**Terminal 2 (Codex)에서:**

1. gemini_analysis.md 내용을 먼저 컨텍스트로 제공 (필요시)
2. 치환된 프롬프트 전체를 붙여넣기
3. Enter 눌러서 실행
4. Codex가 계획 작업 수행

### 6.3 Codex 출력 저장

```bash
# Codex 출력을 저장
vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md
# 붙여넣기 후 저장
```

### 6.4 Codex 출력 검증

```bash
echo "=== Codex 출력 확인 ==="
grep "## 2. File Changes" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md && echo "✓ 파일 변경 목록"
grep "## 4. Implementation Details" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md && echo "✓ 구현 상세"
grep "## 5. Dependency Order" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md && echo "✓ 의존성 순서"
grep "## 7. Rollback Plan" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md && echo "✓ 롤백 계획"
grep "## 8. Handoff to Claude" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md && echo "✓ Claude 핸드오프"

echo ""
echo "=== 생성/수정 예정 파일 ==="
grep -E "\[NEW\]|\[MODIFY\]|\[DELETE\]" skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md | head -10
```

---

## 7. Phase 3: Claude 구현

### 7.1 프롬프트 준비

```bash
# 1. 프롬프트 파일 열기
cat skillkit_context_engineering/ops/PROMPTS/CLAUDE_IMPLEMENT.md

# 2. 전체 내용 복사

# 3. <RUN_NAME> 치환
```

### 7.2 Claude에 프롬프트 전달

**Terminal 3 (Claude)에서:**

1. codex_plan.md 내용을 컨텍스트로 제공 (권장)
2. 치환된 프롬프트 전체를 붙여넣기
3. Enter 눌러서 실행
4. Claude가 실제 구현 수행

### 7.3 중요: Claude가 하는 일

```
Claude는 두 가지를 동시에 수행:
1. 실제 파일 생성/수정 (코드 구현)
2. 구현 로그 작성 (claude_impl.md)
```

### 7.4 Claude 출력 저장

```bash
# 구현 로그 저장
vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md
```

### 7.5 변경 사항 확인

```bash
# git으로 변경된 파일 확인
git status

# 변경 내용 미리보기
git diff

# 예상
# modified:   src/auth/__init__.py
# new file:   src/auth/jwt.py
# modified:   src/api/routes.py
# new file:   tests/test_auth.py
```

### 7.6 증거 수집 (Claude가 해야 하지만 확인)

```bash
# 테스트 실행 및 로그 저장
pytest tests/ -v 2>&1 | tee skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/test.log

# 문법 확인
python -m py_compile src/auth/jwt.py 2>&1 | tee -a skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/syntax.log

# 시크릿 스캔
grep -rn "password = " src/ 2>&1 | tee skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/secrets_scan.log
```

---

## 8. Phase 4: Gate 실행

### 8.1 Preflight Gate (L0) — 필수

```bash
echo "╔═══════════════════════════════════════╗"
echo "║      PREFLIGHT GATE (L0)              ║"
echo "╚═══════════════════════════════════════╝"

BLOCKERS=0

# PRE-001: Kit 파일 확인
echo -n "PRE-001 Kit files...     "
if [ -f "skillkit_context_engineering/spec/00_core.md" ]; then
  echo "✓ PASS"
else
  echo "✗ FAIL [BLOCKER]"
  BLOCKERS=$((BLOCKERS + 1))
fi

# PRE-002: Casefile 존재
echo -n "PRE-002 Casefile...      "
if [ -f "skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md" ]; then
  echo "✓ PASS"
else
  echo "✗ FAIL [BLOCKER]"
  BLOCKERS=$((BLOCKERS + 1))
fi

# PRE-003: Output 존재
echo -n "PRE-003 Outputs...       "
if [ -f "skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md" ] && \
   [ -f "skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md" ] && \
   [ -f "skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md" ]; then
  echo "✓ PASS"
else
  echo "✗ FAIL [BLOCKER]"
  BLOCKERS=$((BLOCKERS + 1))
fi

# PRE-004: 시크릿 스캔
echo -n "PRE-004 No secrets...    "
SECRETS=$(grep -rn --include="*.py" "password\s*=" src/ 2>/dev/null | grep -v test | wc -l)
if [ "$SECRETS" -eq 0 ]; then
  echo "✓ PASS"
else
  echo "✗ FAIL [BLOCKER] ($SECRETS found)"
  BLOCKERS=$((BLOCKERS + 1))
fi

echo "═══════════════════════════════════════"
if [ "$BLOCKERS" -eq 0 ]; then
  echo "Result: ✓ PASS — Preflight 통과!"
else
  echo "Result: ✗ FAIL — $BLOCKERS blockers. 수정 후 재실행."
fi
```

### 8.2 CI Gate (L1) — 테스트

```bash
echo "╔═══════════════════════════════════════╗"
echo "║          CI GATE (L1)                 ║"
echo "╚═══════════════════════════════════════╝"

# 테스트 실행
echo "Running tests..."
pytest tests/ -v --tb=short 2>&1 | tee skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/test.log
TEST_EXIT=$?

if [ "$TEST_EXIT" -eq 0 ]; then
  echo "═══════════════════════════════════════"
  echo "Result: ✓ PASS — 모든 테스트 통과!"
else
  echo "═══════════════════════════════════════"
  echo "Result: ✗ FAIL — 테스트 실패. 로그 확인:"
  echo "  cat skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/test.log"
fi
```

### 8.3 Semantic Gate (L2) — 수동 검토

```bash
echo "╔═══════════════════════════════════════╗"
echo "║       SEMANTIC GATE (L2)              ║"
echo "╚═══════════════════════════════════════╝"
echo ""
echo "다음 파일들을 수동으로 검토하세요:"
echo ""
echo "1. gemini_analysis.md — 제약 조건 체크 확인"
echo "   vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/gemini_analysis.md"
echo ""
echo "2. codex_plan.md — 롤백 계획 확인"
echo "   vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/codex_plan.md"
echo ""
echo "3. claude_impl.md — 증거 수집 확인"
echo "   vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/02_outputs/claude_impl.md"
echo ""
echo "체크리스트:"
echo "  [ ] 모든 숫자 주장에 출처가 있나? (rubric-evidence)"
echo "  [ ] 논리적 모순이 없나? (rubric-logic)"
echo "  [ ] 명령어가 모두 재현 가능한가? (rubric-repro)"
```

### 8.4 Gate 결과 저장

```bash
# JSON으로 결과 저장
cat > skillkit_context_engineering/reports/latest/gate_preflight.json << EOF
{
  "gate": "preflight",
  "run": "$RUN_NAME",
  "timestamp": "$(date -Iseconds)",
  "status": "PASS",
  "blockers": 0
}
EOF

cat > skillkit_context_engineering/reports/latest/gate_ci.json << EOF
{
  "gate": "ci",
  "run": "$RUN_NAME",
  "timestamp": "$(date -Iseconds)",
  "status": "PASS",
  "tests_passed": true
}
EOF

echo "✓ Gate 결과 저장됨"
```

---

## 9. 완료 및 아카이브

### 9.1 Summary 작성

```bash
cat > skillkit_context_engineering/reports/latest/summary.md << EOF
# Run Summary: $RUN_NAME

## Metadata
| Field | Value |
|-------|-------|
| **Run** | \`$RUN_NAME\` |
| **Completed** | \`$(date -Iseconds)\` |
| **Status** | \`SUCCESS\` |

## Gate Results
| Gate | Status |
|------|--------|
| Preflight (L0) | PASS |
| CI (L1) | PASS |
| Semantic (L2) | PASS |

## Changes Made
$(git diff --stat HEAD~1 2>/dev/null || echo "See claude_impl.md for details")

## Next Steps
1. Review changes: \`git diff\`
2. Commit: \`git commit -am "feat: $RUN_NAME"\`
3. Push: \`git push\`
EOF

echo "✓ Summary 작성됨"
cat skillkit_context_engineering/reports/latest/summary.md
```

### 9.2 Git 커밋

```bash
# 변경사항 확인
git status
git diff

# 스테이징
git add .

# 또는 특정 파일만
# git add src/auth/ tests/test_auth.py

# 커밋
git commit -m "feat: Add JWT authentication

Run: $RUN_NAME
- Added login endpoint
- Added token validation middleware
- Tests passing
"
```

### 9.3 아카이브 (선택)

```bash
# 이번 run의 결과를 아카이브
ARCHIVE_DIR="skillkit_context_engineering/reports/archive/$(date +%Y-%m-%d)"
mkdir -p $ARCHIVE_DIR
cp skillkit_context_engineering/reports/latest/* $ARCHIVE_DIR/
echo "✓ 아카이브됨: $ARCHIVE_DIR"
```

---

## 10. 문제 해결

### 10.1 "RUN_NAME이 설정되지 않았습니다"

```bash
# 확인
echo $RUN_NAME

# 설정
export RUN_NAME="2026-01-02__your-task"
```

### 10.2 "Casefile이 없습니다"

```bash
# 템플릿에서 복사
cp -r skillkit_context_engineering/ops/RUNS/_TEMPLATE_RUN \
      skillkit_context_engineering/ops/RUNS/$RUN_NAME
```

### 10.3 "테스트가 실패합니다"

```bash
# 로그 확인
cat skillkit_context_engineering/ops/RUNS/$RUN_NAME/01_inputs/test.log

# 특정 테스트만 실행
pytest tests/test_auth.py -v -s

# 디버깅 모드
pytest tests/test_auth.py --pdb
```

### 10.4 "시크릿이 감지되었습니다"

```bash
# 어디서 감지됐는지 확인
grep -rn "password\s*=" src/ | grep -v test

# 수정: 환경변수로 변경
# 전: password = "secret123"
# 후: password = os.getenv("DB_PASSWORD")
```

### 10.5 "롤백이 필요합니다"

```bash
# 모든 변경사항 취소
git checkout HEAD -- src/ tests/

# 특정 파일만
git checkout HEAD -- src/auth/jwt.py

# 새로 만든 파일 삭제
git clean -fd src/auth/
```

### 10.6 "다음 Run을 시작하고 싶습니다"

```bash
# 새 Run 이름 설정
export RUN_NAME="2026-01-02__new-task"

# 새 Run 디렉토리 생성
cp -r skillkit_context_engineering/ops/RUNS/_TEMPLATE_RUN \
      skillkit_context_engineering/ops/RUNS/$RUN_NAME

# Casefile 작성부터 다시 시작
vim skillkit_context_engineering/ops/RUNS/$RUN_NAME/00_casefile.md
```

---

## 빠른 참조 카드

```
╔═══════════════════════════════════════════════════════════════╗
║                    SKILLKIT 빠른 참조                          ║
╠═══════════════════════════════════════════════════════════════╣
║                                                               ║
║  시작하기                                                      ║
║  ────────                                                     ║
║  export RUN_NAME="$(date +%Y-%m-%d)__task-name"               ║
║  cp -r .../ops/RUNS/_TEMPLATE_RUN .../ops/RUNS/$RUN_NAME      ║
║  vim .../ops/RUNS/$RUN_NAME/00_casefile.md                    ║
║                                                               ║
║  3단계 실행                                                    ║
║  ──────────                                                   ║
║  T1: GEMINI_ANALYZE.md 복붙 → gemini_analysis.md              ║
║  T2: CODEX_PLAN.md 복붙 → codex_plan.md                       ║
║  T3: CLAUDE_IMPLEMENT.md 복붙 → claude_impl.md + 코드          ║
║                                                               ║
║  Gate 실행                                                     ║
║  ─────────                                                    ║
║  L0: 스펙 존재, 시크릿 없음, casefile 완료                       ║
║  L1: 테스트 통과, 빌드 성공                                     ║
║  L2: 루브릭 점수 >= 임계값                                      ║
║                                                               ║
║  실패 시                                                       ║
║  ───────                                                      ║
║  BLOCKER → 즉시 중단 → 수정 → T1부터 재시작                      ║
║  MAJOR → 문서화 → 계속 진행 가능                                 ║
║  MINOR → 메모 → 계속 진행                                       ║
║                                                               ║
║  롤백                                                          ║
║  ─────                                                        ║
║  git checkout HEAD -- src/                                    ║
║  git clean -fd                                                ║
║                                                               ║
╚═══════════════════════════════════════════════════════════════╝
```

---

**Version**: 1.0.0  
**Last Updated**: 2026-01-02
