🌐 English | [日本語](README.ja.md) | [한국어](README.ko.md) | [Français](README.fr.md) | [Deutsch](README.de.md) | [Русский](README.ru.md) | [हिन्दी](README.hi.md) | [Türkçe](README.tr.md) 

---
name: data-analysis
description: EvoLink API를 사용한 AI 기반 데이터 분석. 의사결정 우선 방법론과 통계적 엄격성. Powered by evolink.ai
---

# 데이터 분석 어시스턴트

의사결정 우선 방법론과 통계적 엄격성을 갖춘 AI 기반 데이터 분석.

Powered by [Evolink.ai](https://evolink.ai?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)

## 사용 시기

다음과 같은 경우 이 스킬을 사용하세요:
- CSV, Excel, JSON 파일의 데이터 분석
- 패턴, 트렌드 또는 이상 징후 발견
- 메트릭 및 KPI 이해
- 가설 또는 A/B 테스트 검증
- 코호트 또는 퍼널 분석 수행
- 데이터 품질 문제 디버깅
- 의사결정을 위한 인사이트 생성

**핵심 원칙**: 의사결정이 없는 분석은 단순한 산술일 뿐입니다. 분석 결과가 X 또는 Y를 보여줄 때 무엇이 달라질지 항상 명확히 하세요.

## 사용법

```bash
{baseDir}/scripts/analyze.sh <file_path> "<analysis_question>"
```

## 설정

| 변수 | 기본값 | 필수 | 설명 |
|---|---|---|---|
| `EVOLINK_API_KEY` | - | 예 | EvoLink API 키 |
| `EVOLINK_MODEL` | `[REDACTED]` | 아니오 | 분석용 모델. [EvoLink API](https://docs.evolink.ai/en/api-manual/language-series/claude/claude-messages-api?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)에서 지원하는 모든 모델로 전환 가능 |
| `DATA_ANALYSIS_SAFE_DIR` | `$HOME/.openclaw/workspace` | 아니오 | 로컬 파일 접근 허용 디렉토리 |

👉 [무료 API 키 받기](https://evolink.ai/signup?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)

## 예제

```bash
bash scripts/analyze.sh sales_data.csv "이번 분기 상위 3개 매출 동인은 무엇인가요?"
```

출력:
```
📊 분석 중: sales_data.csv
❓ 질문: 이번 분기 상위 3개 매출 동인은 무엇인가요?

🔍 분석 결과:

1. **제품 카테고리 A** - $2.4M (전체의 40%)
   - 지난 분기 대비 15% 성장
   - 엔터프라이즈 부문 주도

2. **지리적 확장** - $1.8M (전체의 30%)
   - APAC 지역 신규 시장
   - 지난 분기 대비 3배 성장

3. **기존 고객 업셀** - $1.2M (전체의 20%)
   - 업그레이드 제안 전환율 25%
   - 평균 거래 규모: $50K

📈 신뢰도: 높음 (n=1,247 거래)
⚠️  주의사항: 4분기에는 연말 시즌 효과 포함
💡 권장사항: APAC 확장 및 엔터프라이즈 업셀에 집중
```

## 방법론

### 1. 의사결정 우선
- 의사결정권자와 질문 식별
- 결과에 따라 무엇이 달라질지 명확히 함
- 계산 전에 마감일 설정

### 2. 통계적 엄격성
- 표본 크기 충분성 확인
- 공정한 비교 그룹 보장
- 다중 비교 고려
- 불확실성 정량화 (신뢰 구간)
- 효과 크기의 의미 검증

### 3. 출력 기준
- 방법론이 아닌 인사이트로 시작
- 불확실성 정량화 (점 추정이 아닌 범위)
- 제한사항 명확히 명시
- 다음 단계 권장

## 보안

**⚠️ 데이터 전송 경고**

이 스킬은 데이터 파일의 **전체 내용**을 읽어 `api.evolink.ai`로 분석을 위해 전송합니다. **다음을 포함하는 파일에는 이 스킬을 사용하지 마세요:**
- API 키, 토큰 또는 자격 증명
- 개인 식별 정보(PII)
- 기밀 비즈니스 데이터
- 외부 서비스로 전송하고 싶지 않은 민감한 정보

스크립트는 보안 검사(디렉토리 제약, 심볼릭 링크 거부, 파일명 블랙리스트, 크기/MIME 검증)를 구현하지만, 임의의 데이터 파일에 비밀 정보가 없음을 **보장할 수 없습니다**.

**자격 증명 및 네트워크**

EvoLink API 호출을 위해 `EVOLINK_API_KEY`가 필요합니다. 데이터 파일 내용과 분석 질문이 처리를 위해 `api.evolink.ai`로 전송됩니다. EvoLink는 데이터를 처리하고 분석 결과를 반환합니다. 처리 후 데이터는 저장되지 않습니다.

**파일 접근**

이 스킬은 로컬 파일시스템에서 지정된 데이터 파일(CSV, Excel, JSON)을 읽습니다. 파일은 `DATA_ANALYSIS_SAFE_DIR` 내에 있어야 합니다(기본값: `$HOME/.openclaw/workspace`). 

보안 검증:
- `realpath -e`를 통한 경로 해석 (파일 존재 필요, 심볼릭 링크 해석)
- 심볼릭 링크 입력 명시적 거부
- 후행 슬래시 비교를 통한 디렉토리 제약
- 파일명 블랙리스트: `.env*`, `*.key`, `*.pem`, `*.p12`, `*.pfx`, `id_rsa*`, `authorized_keys`, `config.json`, `.bash_history`, `.ssh`, `shadow`, `passwd`
- 파일 크기 제한: 최대 50MB
- MIME 검증: `text/csv`, `text/plain`, `application/json`, `application/vnd.ms-excel`, `application/vnd.openxmlformats-officedocument.spreadsheetml.sheet`만 허용

**네트워크 접근**

- **EvoLink API** (`api.evolink.ai`) - 데이터 전송 및 분석 수신

모든 네트워크 호출은 curl을 사용하며 스크립트 소스에서 감사할 수 있습니다.

**지속성 및 권한**

이 스킬은 다른 스킬이나 시스템 설정을 수정하지 않습니다. 상승된 권한이나 지속적인 권한을 요청하지 않습니다.

## 링크

- [GitHub](https://github.com/EvoLinkAI/data-analysis-skill-for-openclaw)
- [API 참조](https://docs.evolink.ai/en/api-manual/language-series/claude/claude-messages-api?utm_source=clawhub&utm_medium=skill&utm_campaign=data-analysis)
- [커뮤니티](https://discord.com/invite/5mGHfA24kn)
- [지원](mailto:support@evolink.ai)
