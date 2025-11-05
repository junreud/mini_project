# 🚀 Google Colab 사용 가이드

## 📌 Phase 1: 환경 구성 완벽 가이드

---

## 1️⃣ 노트북을 Colab에 업로드하는 방법

### 방법 A: GitHub 업로드 (추천)
1. 로컬 프로젝트를 GitHub에 푸시
2. Colab에서 `File → Open notebook → GitHub 탭`
3. 레포지토리 선택 → `phase1_colab_setup.ipynb` 열기

### 방법 B: 직접 업로드
1. [Google Colab](https://colab.research.google.com) 접속
2. `File → Upload notebook`
3. `notebooks/phase1_colab_setup.ipynb` 파일 선택

### 방법 C: Google Drive 사용
1. Google Drive에 `chatbot_project` 폴더 생성
2. 노트북 파일을 Drive에 업로드
3. 노트북을 더블클릭하면 Colab에서 열림

---

## 2️⃣ GPU 설정하기

### 무료 T4 GPU 활성화 (필수!)

```
상단 메뉴바 → 런타임(Runtime) → 런타임 유형 변경(Change runtime type)
→ 하드웨어 가속기: T4 GPU 선택 → 저장
```

### GPU 종류별 비교

| GPU | VRAM | 속도 | 사용 가능 |
|-----|------|------|-----------|
| None (CPU) | - | 매우 느림 | ❌ 학습 불가능 |
| T4 (무료) | 16GB | 보통 | ✅ 7B 모델 QLoRA 가능 |
| V100 (Pro) | 16GB | 빠름 | ✅ 7B 모델 LoRA 가능 |
| A100 (Pro+) | 40GB | 매우 빠름 | ✅ 13B 모델도 가능 |

**우리 프로젝트**: 무료 T4로 충분합니다! 🎉

---

## 3️⃣ 디렉토리 구조 (Colab + Drive)

```
Google Drive/MyDrive/
└── chatbot_project/          # 프로젝트 루트
    ├── data/
    │   ├── raw/              # 원본 데이터 (여기에 업로드)
    │   └── processed/        # 전처리된 데이터
    │
    ├── models/
    │   ├── checkpoints/      # 학습 중 저장 (자동)
    │   └── final/            # 최종 모델
    │
    └── outputs/
        └── logs/             # 학습 로그
```

### 중요! 💡
- `/content/` : 임시 폴더 (세션 끊기면 삭제됨)
- `/content/drive/MyDrive/` : 영구 저장 (여기에 저장!)

---

## 4️⃣ 실행 순서 (노트북 셀 순서대로)

### ✅ Step 1: GPU 확인
```python
# 셀 실행 → GPU 이름과 메모리 확인
# T4 GPU (약 15GB 사용 가능)가 보여야 합니다
```

### ✅ Step 2: Drive 마운트
```python
# 팝업창이 뜨면 Google 계정 선택
# "액세스 허용" 클릭
# ✅ 체크 표시가 뜨면 성공!
```

### ✅ Step 3: 디렉토리 생성
```python
# 자동으로 필요한 폴더들이 생성됩니다
# Drive에서 확인 가능
```

### ✅ Step 4: 라이브러리 설치
```python
# 약 3-5분 소요
# %%capture 때문에 로그가 안 보이는게 정상입니다
# (로그 보고 싶으면 해당 줄 삭제)
```

### ✅ Step 5: 설치 확인
```python
# 모든 라이브러리 버전이 출력되어야 합니다
```

### ✅ Step 6: 테스트 모델 실행
```python
# 간단한 한국어 텍스트 생성 테스트
# 생성된 텍스트가 보이면 성공!
```

---

## 5️⃣ 자주 발생하는 문제 해결

### ❌ GPU를 사용할 수 없습니다
**원인**: GPU 설정이 안 되어 있음  
**해결**: 런타임 → 런타임 유형 변경 → T4 GPU 선택

### ❌ Colab이 너무 느립니다
**원인**: 무료 버전의 제한  
**해결**: 
- 불필요한 출력 줄이기 (`%%capture` 사용)
- 작은 배치 사이즈 사용
- 세션을 새로 시작하기

### ❌ 세션이 자꾸 끊깁니다
**원인**: 무료 버전은 약 12시간 제한  
**해결**:
- 체크포인트를 자주 저장 (자동)
- 중요한 파일은 Drive에 저장
- 긴 학습은 밤에 돌리기

### ❌ 메모리 부족 오류
**원인**: GPU 메모리 초과  
**해결**:
```python
import gc
gc.collect()
torch.cuda.empty_cache()
```

### ❌ Drive 마운트가 안 됩니다
**원인**: 권한 문제  
**해결**:
1. 팝업 차단 해제
2. 다른 Google 계정으로 시도
3. 브라우저 캐시 삭제

---

## 6️⃣ Colab 사용 꿀팁 🍯

### 단축키
- `Ctrl/Cmd + Enter`: 현재 셀 실행
- `Shift + Enter`: 현재 셀 실행 후 다음 셀로
- `Ctrl/Cmd + M B`: 아래에 셀 추가
- `Ctrl/Cmd + M D`: 셀 삭제

### 빠른 명령어
```python
# GPU 상태 확인
!nvidia-smi

# 현재 경로 확인
!pwd

# Drive 파일 목록
!ls /content/drive/MyDrive/chatbot_project/

# 메모리 사용량
!free -h

# 디스크 사용량
!df -h
```

### 코드 스니펫 활용
- 왼쪽 사이드바 `< >` 아이콘
- 카메라, 파일 업로드 등 유용한 코드

### 파일 업로드 (작은 파일)
```python
from google.colab import files
uploaded = files.upload()
```

### 결과 다운로드
```python
from google.colab import files
files.download('/content/drive/MyDrive/chatbot_project/outputs/result.txt')
```

---

## 7️⃣ 학습 중 주의사항 ⚠️

### DO ✅
- 학습 시작 전 Drive 마운트 확인
- 중요한 파일은 Drive에 저장
- 체크포인트 자주 저장 (자동 설정됨)
- 세션 끊기기 전에 모델 저장 확인

### DON'T ❌
- 학습 중 탭 닫기 (세션 끊김)
- `/content/`에 중요한 파일 저장
- 너무 큰 배치 사이즈 사용 (메모리 부족)
- 여러 모델 동시 로딩 (메모리 부족)

---

## 8️⃣ 비용 관련 정보 💰

### 무료 버전 (Colab Free)
- ✅ T4 GPU 사용 가능
- ⏰ 세션 시간: 최대 12시간
- 💾 저장 공간: Google Drive (15GB 무료)
- 💵 비용: **무료**

### 유료 버전 (필요시)
- **Colab Pro** ($10/월): 더 긴 세션, V100 GPU
- **Colab Pro+** ($50/월): A100 GPU, 백그라운드 실행
- **우리 프로젝트**: 무료로 충분! ✅

---

## 9️⃣ 다음 단계 준비

Phase 1이 완료되면 다음을 준비하세요:

1. **데이터 수집**: 50-100개의 한국어 대화 데이터
2. **포맷**: JSON 형식으로 작성
3. **업로드**: Drive의 `data/raw/` 폴더에 저장

예시 데이터 형식:
```json
[
  {
    "instruction": "파이썬이 뭐야?",
    "input": "",
    "output": "파이썬은 배우기 쉽고 강력한 프로그래밍 언어입니다."
  }
]
```

---

## 🆘 도움이 필요하면

1. **오류 메시지**: 전체 오류 메시지 복사해서 검색
2. **Colab 공식 문서**: [colab.research.google.com](https://colab.research.google.com)
3. **커뮤니티**: Reddit r/GoogleColab, Stack Overflow

---

**준비 완료!** 🎉 이제 `phase1_colab_setup.ipynb`를 Colab에서 실행하세요!
