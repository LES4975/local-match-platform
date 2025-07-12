# Django + Flutter 앱 개발 프로세스 가이드

## 프로젝트 개요
- **프론트엔드**: Flutter (모바일 앱)
- **백엔드**: Django + Django REST Framework
- **데이터베이스**: PostgreSQL
- **목적**: 공공 복지 혜택 및 주거 정보 추천 앱

## 기술 스택 선택 이유

### Django 선택 이유
- 웹 개발 경험 있음 (익숙함)
- Django Admin으로 복지 정보 관리 용이
- 강력한 ORM과 보안 기능
- AI/ML 라이브러리와 호환성 좋음

### PostgreSQL 선택 이유
- 복지 혜택 조건의 복잡한 관계 표현 적합
- 다양한 필터링 쿼리 성능 우수
- Django ORM과 완벽 호환
- 정형화된 데이터 관리 효율적

## 웹 개발 vs 앱 개발 차이점

| 구분 | 웹 개발 | 앱 개발 |
|------|---------|---------|
| 템플릿 | HTML 페이지 렌더링 | JSON 데이터만 제공 |
| UI 담당 | Django 템플릿 | Flutter가 모든 UI 담당 |
| 인증 방식 | 세션 기반 (쿠키) | JWT 토큰 기반 |
| 응답 형태 | HTML 반환 | JSON 응답 |
| 추가 설정 | - | CORS 설정 필수 |

## 개발 프로세스 단계별 가이드

### 1. 프로젝트 초기 설정
```bash
# Django 프로젝트 생성
django-admin startproject welfare_app
cd welfare_app

# 필수 패키지 설치
pip install django
pip install djangorestframework
pip install psycopg2-binary
pip install django-cors-headers
pip install djangorestframework-simplejwt
```

**settings.py 설정**
- PostgreSQL 데이터베이스 연결
- Django REST Framework 추가
- CORS 설정
- JWT 토큰 인증 설정

### 2. 데이터베이스 모델 설계

**주요 모델 구조**
- **User**: 사용자 정보 (확장된 사용자 모델)
- **Benefit**: 복지 혜택 정보
- **Category**: 혜택 카테고리
- **Region**: 지역 정보
- **Recommendation**: 추천 히스토리
- **UserProfile**: 사용자 상세 프로필

**모델 관계**
- User ↔ UserProfile (1:1)
- Benefit ↔ Category (M:N)
- Benefit ↔ Region (M:N)
- User ↔ Recommendation (1:M)

### 3. API 엔드포인트 설계

**인증 관련 API**
- POST /api/auth/register/ - 회원가입
- POST /api/auth/login/ - 로그인
- POST /api/auth/token/refresh/ - 토큰 갱신
- POST /api/auth/logout/ - 로그아웃

**복지 정보 API**
- GET /api/benefits/ - 복지 혜택 목록
- GET /api/benefits/{id}/ - 특정 혜택 상세
- GET /api/benefits/search/ - 혜택 검색
- GET /api/categories/ - 카테고리 목록
- GET /api/regions/ - 지역 목록

**추천 시스템 API**
- GET /api/recommendations/ - 개인 맞춤 추천
- POST /api/recommendations/ - 추천 요청
- GET /api/recommendations/history/ - 추천 히스토리

**사용자 관련 API**
- GET /api/users/profile/ - 사용자 프로필 조회
- PUT /api/users/profile/ - 사용자 프로필 수정
- GET /api/users/favorites/ - 즐겨찾기 목록
- POST /api/users/favorites/ - 즐겨찾기 추가

### 4. 시리얼라이저 구현

**핵심 시리얼라이저**
- UserSerializer: 사용자 정보 직렬화
- BenefitSerializer: 복지 혜택 직렬화
- RecommendationSerializer: 추천 데이터 직렬화
- CategorySerializer: 카테고리 직렬화

**중첩 관계 처리**
- 복지 혜택과 카테고리 관계
- 사용자와 추천 히스토리 관계
- 지역과 혜택 관계

### 5. 뷰 및 ViewSet 구현

**ViewSet 활용**
- BenefitViewSet: 복지 혜택 CRUD
- RecommendationViewSet: 추천 관련 로직
- UserProfileViewSet: 사용자 프로필 관리

**권한 및 인증 처리**
- JWT 토큰 인증
- 권한 클래스 설정
- 사용자별 데이터 접근 제어

**페이지네이션**
- 복지 혜택 목록 페이지네이션
- 추천 히스토리 페이지네이션

### 6. AI/추천 시스템 연동

**OpenAI API 연동**
- 사용자 질문 처리
- 복지 혜택 설명 생성
- 맞춤형 추천 로직

**추천 알고리즘**
- 사용자 프로필 기반 필터링
- 과거 추천 히스토리 활용
- 지역 기반 추천

**비동기 처리**
- Celery로 무거운 AI 작업 처리
- Redis를 큐로 활용
- 추천 결과 캐싱

### 7. 테스트 및 문서화

**API 테스트**
- Django Test Framework 활용
- 각 엔드포인트별 테스트 케이스
- 인증/권한 테스트

**문서화**
- drf-spectacular로 Swagger 문서 자동 생성
- API 엔드포인트 문서화
- 사용 예시 포함

### 8. 배포 준비

**환경 설정**
- 개발/운영 환경 분리
- 환경 변수 관리
- 시크릿 키 보안

**성능 최적화**
- 데이터베이스 쿼리 최적화
- 인덱스 설정
- 캐싱 전략

**로깅 및 모니터링**
- 로그 레벨 설정
- 에러 추적
- 성능 모니터링

## 필수 학습 항목

### 우선순위 높음
1. **Django REST Framework**
   - 시리얼라이저 사용법
   - ViewSet과 APIView 차이
   - 권한 및 인증 시스템

2. **JWT 토큰 인증**
   - 토큰 생성/검증 방식
   - 리프레시 토큰 처리
   - 보안 고려사항

### 우선순위 중간
3. **PostgreSQL과 Django ORM**
   - 복잡한 쿼리 최적화
   - 인덱스 설정
   - 마이그레이션 관리

4. **API 설계 원칙**
   - RESTful API 구조
   - HTTP 상태 코드 활용
   - 에러 처리 방식

### 우선순위 낮음
5. **배포 및 운영**
   - 환경 변수 관리
   - 로깅 설정
   - 성능 모니터링

## Flutter와의 연동 방식

### Django 측면
- Django REST Framework로 JSON API 제공
- CORS 설정으로 Flutter 앱과 통신
- JWT 토큰 인증 구현

### Flutter 측면
- http 패키지로 REST API 호출
- dio 패키지로 고급 HTTP 통신
- 상태 관리(Provider/Riverpod)로 데이터 관리

## 주의사항

1. **보안**
   - API 키 관리
   - 사용자 입력 데이터 검증
   - SQL 인젝션 방지

2. **성능**
   - 데이터베이스 쿼리 최적화
   - 적절한 캐싱 전략
   - API 응답 시간 관리

3. **확장성**
   - 모듈화된 코드 구조
   - 재사용 가능한 컴포넌트
   - 버전 관리 전략

## 참고 자료

- Django REST Framework 공식 문서
- PostgreSQL 최적화 가이드
- JWT 토큰 보안 가이드
- Flutter HTTP 통신 가이드