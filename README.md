# ai-pipeline-practice1

키워드 기반 스팸 분류기를 FastAPI로 서빙하는 웹 애플리케이션입니다.

## 프로젝트 구조

```
ai-pipeline-practice1/
├── app/
│   ├── main.py       # FastAPI 앱 진입점 (라우터 정의)
│   └── spam.py       # 스팸 분류 로직
├── static/
│   └── index.html    # 웹 UI (textarea + 분류 버튼)
├── requirements.txt  # 의존 패키지 목록
└── README.md
```

## 동작 방식

### 스팸 분류 로직 (`app/spam.py`)

`check_spam(text)` 함수는 입력 텍스트에 스팸 키워드가 몇 개 포함되어 있는지 세어 분류합니다.

- **스팸 키워드**: `free`, `win`, `winner`, `prize`, `click`, `buy now`, `urgent`, `cash`, `money`, `offer`, `deal`
- **판정 기준**: 키워드 2개 이상 일치 시 `spam`, 미만이면 `ham`
- **반환값**: `(label, score)` — label은 `"spam"` 또는 `"ham"`, score는 일치한 키워드 수

### API 엔드포인트 (`app/main.py`)

| 메서드 | 경로        | 설명                          |
|--------|-------------|-------------------------------|
| GET    | `/`         | 웹 UI 페이지 반환             |
| POST   | `/classify` | 텍스트 분류 요청 처리         |

**POST `/classify` 요청 예시**

```json
{ "text": "Win a FREE prize now" }
```

**응답 예시**

```json
{ "label": "spam", "score": 3 }
```

### 웹 UI (`static/index.html`)

브라우저에서 텍스트를 입력하고 **분류** 버튼을 누르면 `/classify` 엔드포인트를 호출하여 결과(`label`, `score`)를 화면에 출력합니다.

## 설치 및 실행

### 1. 의존 패키지 설치

```bash
pip install -r requirements.txt
```

### 2. 서버 실행

```bash
uvicorn app.main:app --reload
```

### 3. 접속

- 웹 UI: [http://127.0.0.1:8000](http://127.0.0.1:8000)
- Swagger UI (API 문서): [http://127.0.0.1:8000/docs](http://127.0.0.1:8000/docs)

## 의존 패키지

| 패키지             | 역할                          |
|--------------------|-------------------------------|
| `fastapi`          | 웹 프레임워크                 |
| `uvicorn`          | ASGI 서버                     |
| `python-multipart` | 폼 데이터 파싱 지원           |
