# OCR 기반 복약안내문/처방전 추출 서비스

이 프로젝트는 **Flask + Google Cloud Vision API + OpenAI API**를 사용하여  
이미지에서 텍스트(OCR)를 추출하고, 정규식/AI 요약을 통해 **처방전·복약안내문 정보를 구조화**하여 반환하는 서비스입니다.  
프론트엔드(React 등)에서 API를 호출하면 **TTS로 읽기 적합한 한 문장 요약**을 받을 수 있습니다.

---

## 주요 기능
- **OCR 처리**: Google Cloud Vision API를 이용해 이미지에서 텍스트 추출
- **처방전 처리**: 병원명, 성명, 약목록(약 이름, 용법, 투약일수) 추출
- **복약안내문 처리**: 약효, 처방례, 주의사항을 OpenAI API로 **한 문장 핵심 요약**
- **Swagger UI**: API 문서 자동 제공
- **CORS 지원**: 프론트엔드(React 등)와 연동 가능

---

## 기술 스택
- **Backend**: Python, Flask
- **OCR**: Google Cloud Vision API
- **AI 요약**: OpenAI API (GPT-4o-mini)
- **환경 관리**: python-dotenv
- **문서화**: Flasgger (Swagger UI)

---

## 설치 및 실행

### 1. 저장소 클론
```bash
git clone <repository-url>
cd <project-folder>

2. 가상환경 생성 및 활성화
python -m venv venv
source venv/bin/activate   # Linux/Mac
venv\Scripts\activate      # Windows

3. 패키지 설치
pip install -r requirements.txt

4. 환경변수 설정
프로젝트 루트에 .env 파일 생성:

env
OPENAI_API_KEY=sk-xxxx...
GOOGLE_APPLICATION_CREDENTIALS=backend/gcp-key.json

5. 실행

python app.py
API 엔드포인트
POST /ocr
이미지를 업로드하면 OCR → 추출/요약 → JSON 결과 반환

Request (form-data)

image: 업로드할 이미지 파일

Response (예시)

{
  "type": "복약안내문",
  "성명": "양준모",
  "나이": "52",
  "병원명": "세란병원",
  "약효목록": [
    {
      "효능": "소염진통제",
      "요약": "Airtal은 관절염과 통증을 줄이는 소염진통제로, 위장 장애와 출혈 위험에 주의해야 합니다."
    },
    {
      "효능": "위장관운동조절제",
      "요약": "이 약은 소화불량을 개선하는 위장관운동조절제로, 설사와 어지러움이 나타날 수 있습니다."
    }
  ]
}
Swagger 문서 확인:
http://localhost:5000/apidocs

requirements.txt 예시

flask
flask-cors
flasgger
google-cloud-vision
python-dotenv
openai
