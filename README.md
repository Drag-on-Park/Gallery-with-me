# Gallery With Me

## 🖼️ 프로젝트 소개

**Gallery With Me**는 졸업 직전 미대생의 졸업 작품이 졸업전시회 이후 방치 · 폐기되는 문제와 미술작가로서 나아가기 어려운 미대생 청년작가를 위한 프로젝트로 작품을 판매하거나 다른 사용자의 작품을 구매할 수 있습니다. 
AI 이미지 인식 기능 기능을 통해 전시 작품뿐 아니라 기존에 가지고 있던 작품 이미지를 첨부하여 어느시대 어떤 작가의 어떤 화풍인지 결과를 받을 수 있습니다.

## ✨ 주요 기능

*   **👨‍🎨 사용자 및 인증**: 회원가입 및 선호 작품 선택, 로그인, 프로필 관리 기능
    *   찜한 작품 & 전시회 북마크
    *   판매 내역 : 상품이 결제 완료 되면 주문 상태가 ‘주문 완료’ 상태로 업데이트(구매 취소 가능)
    *   celery를 이용하여 ‘주문 완료’로 업데이트 된 후 1주일이 지나면 ‘구매 확정’ 상태로 업데이트(구매 취소 불가능, 환불 신청 가능)
    *   ‘구매 확정’ 상태로 업데이트하며 정산 테이블 정보 생성
    *   정산 : 구매 확정이 된 후 계좌 정보가 있는 상태라면, celery를 이용하여 매일 오전 12시 ‘정산 전’ 상태인 정산 정보 ‘정산 완료’로 변경
    *   실제 정산을 위해서는 추가 로직 구현 필요
    *   정산 금액은 수수료와 부가세를 계산한 금액
    *   구매 취소 / 환불요청


*   **🎨 작품 관리**: 작품 등록, 수정, 삭제 및 상세 정보 조회
    *   전국 미술 대학의 과별 전시 정보
    *   태그 검색 : 각 작품에 해당하는 태그 및 재료, 가격 및 크기 정보들을 통해서 작품을 필터링
    *   무한페이지 : AJAX를 활용한 무한 페이지 기능 구현
    *   작품검색: FASTAPI, chatGPT API를 활용해 모든 작품의 제목, 설명의 내용을 임베딩한 후, 입력값과의 유사도를 계산
    
   
*   **🏛️ 전시회**:
    *   전국 미술 대학의 과별 전시 정보
    *   전시명/학과명/학교명 중 원하는 항목으로 검색 가능
    *   지도와 맵핑
    *   북마크 필터링

*   **🛒 주문 및 결제**:
    *   작품 구매를 위한 장바구니 기능
    *   안전한 결제 시스템 연동
    *   배송지: 배송지 변경 버튼 클릭 시 배송지 관리 창 생성
    *   배송지 카드 클릭 시 AJAX를 통해 받는 사람 정보에 정보 자동 입력
      

*   **🗣️ 커뮤니티**: 사용자 간의 소통을 위한 게시판 기능
*   *   **🤖 AI 이미지 인식**:
    *   AI 이미지 검색: 이미지 첨부로 어느시대 어떤 작가의 어떤 화풍인지 결과를 받을 수 있는 일반고객 편의 기능
  

## 🛠️ 기술 스택

### Backend
- **Framework**: Django, Django REST Framework, FastAPI
- **Async Tasks**: Celery, Redis
- **Database**: PostgreSQL
- **AI/ML**: Langchain, Hugging Face Transformers, OpenAI, ONNX
- **Search**: ChromaDB

### DevOps
- **Containerization**: Docker, Docker Compose

## 🚀 설치 및 실행 방법

본 프로젝트는 Docker Compose를 사용하여 간편하게 설치하고 실행할 수 있습니다.

### 사전 요구 사항
- [Docker](https://www.docker.com/get-started)
- [Docker Compose](https://docs.docker.com/compose/install/)

### 설치 과정
1.  **프로젝트 클론**
    ```bash
    git clone https://github.com/your-username/Gallery-with-me.git
    cd Gallery-with-me
    ```

2.  **.env 파일 설정**
    `.env.example` 파일이 있다면 해당 파일을 복사하여 `.env` 파일을 생성하고, 환경에 맞게 변수들을 수정합니다. (만약 없다면, `docker-compose.yml` 파일을 참고하여 필요한 환경변수를 직접 설정해야 합니다.)

    ```bash
    cp .env.example .env
    ```

3.  **Docker Compose 실행**
    다음 명령어를 실행하여 Docker 컨테이너를 빌드하고 실행합니다.

    ```bash
    docker-compose up --build -d
    ```
    
4.  **데이터베이스 마이그레이션**
    웹 서버 컨테이너가 실행된 후, 데이터베이스 스키마를 생성하기 위해 마이그레이션을 진행합니다.

    ```bash
    docker-compose exec web python manage.py migrate
    ```

5.  **관리자 계정 생성**
    필요한 경우, Django 관리자 페이지에 접근하기 위한 슈퍼유저를 생성합니다.

    ```bash
    docker-compose exec web python manage.py createsuperuser
    ```

이제 웹 브라우저에서 `http://localhost:8000` (또는 `.env` 파일에 설정된 포트)으로 접속하여 애플리케이션을 확인할 수 있습니다.

## 📁 프로젝트 구조

```
.
├── art_classifier/   # AI 작품 분류 모델 및 관련 API
├── artwork/          # 작품(Artwork) 모델 및 관련 비즈니스 로직
├── board/            # 커뮤니티 게시판 앱
├── config/           # 프로젝트 전반의 설정 (Django settings, urls)
├── exhibit/          # 전시회(Exhibit) 앱
├── order/            # 주문(Order) 앱
├── payment/          # 결제(Payment) 앱
├── seller/           # 판매자(Seller) 정보 관리 앱
├── user/             # 사용자(User) 모델 및 인증 관련 로직
├── image_api.py      # FastAPI 기반 이미지 검색 API (추정)
├── search_api.py     # FastAPI 기반 텍스트 검색 API (추정)
├── manage.py         # Django 관리 스크립트
├── Dockerfile        # Backend 서버 Docker 이미지 설정
├── docker-compose.yml # 프로젝트 서비스(web, db, redis 등) 정의
└── requirements.txt  # Python 의존성 목록
```
