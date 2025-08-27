# 🎓 Class Progress Dashboard

실시간 학습 진도 관리 시스템으로, 교사가 학생들의 학습 진행상황을 실시간으로 모니터링하고 관리할 수 있는 웹 애플리케이션입니다.

## ✨ 주요 기능

### 👨‍🏫 교사용 대시보드
- **실시간 진도 모니터링**: 학생들의 학습 진행률을 실시간으로 확인
- **개인/조별 모드**: 개별 학습과 조별 활동 모두 지원
- **미션 관리**: 학습 단계별 미션 생성 및 관리
- **메시지 시스템**: 학생이 보낸 질문/도움 요청 확인 및 읽음 처리
- **QR 코드 생성**: 학생 접속용 QR 코드 자동 생성
- **수업 관리**: 기존 수업 불러오기, 새 수업 생성, 수업 삭제

### 👨‍🎓 학생용 인터페이스
- **진행률 업데이트**: 미션 완료 시 자동 진행률 갱신
- **도움 요청**: 실시간 도움 요청 기능
- **메모/질문 전송**: 교사에게 메시지 전송
- **미션 확인**: 현재 및 전체 미션 목록 확인

## 🚀 시작하기

### 1. 프로젝트 클론
```bash
git clone https://github.com/your-username/class-progress.git
cd class-progress
```

### 2. Firebase 설정
1. [Firebase Console](https://console.firebase.google.com/)에서 새 프로젝트 생성
2. Firestore Database 및 Realtime Database 활성화
3. `firebase-config.example.js`를 복사하여 `firebase-config.js` 생성
4. Firebase 프로젝트 설정으로 구성 정보 교체

```javascript
// firebase-config.js
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
  databaseURL: "https://YOUR_PROJECT_ID-default-rtdb.region.firebasedatabase.app/",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT_ID.firebasestorage.app",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

### 3. 로컬 서버 실행
```bash
# HTTP 서버 실행 (예: Python)
python -m http.server 8000

# 또는 Node.js 사용
npx http-server -p 8080

# 또는 Live Server (VSCode 확장)
```

### 4. 접속
- **교사용 대시보드**: `http://localhost:8080/dashboard.html`
- **학생용 페이지**: `http://localhost:8080/student.html`

## 📁 파일 구조

```
class-progress/
├── dashboard.html          # 교사용 대시보드 (메인)
├── student.html           # 학생용 페이지
├── firebase-config.js     # Firebase 설정 (git에서 제외)
├── firebase-config.example.js  # Firebase 설정 예시
├── firebase-functions.js  # Firebase 관련 함수들
├── dashboard.css          # 대시보드 스타일 (분리된 버전)
├── .gitignore            # Git 무시 파일 목록
└── README.md             # 프로젝트 문서
```

## 🛠️ 기술 스택

- **Frontend**: HTML5, CSS3, Vanilla JavaScript
- **Backend**: Firebase Firestore + Realtime Database
- **Authentication**: Firebase Authentication (선택적)
- **Hosting**: 정적 웹 호스팅 (Firebase Hosting, GitHub Pages 등)

## 📖 사용 방법

### 새 수업 시작하기
1. 대시보드에서 **"새 수업"** 버튼 클릭
2. 수업 이름, 학생 수 또는 조 수 설정
3. 미션 목록 작성
4. **"수업 시작"** → QR 코드 생성
5. 학생들에게 QR 코드 공유

### 진행상황 모니터링
- 학생 카드의 색상과 진행률로 상태 확인
- 빨간 테두리: 도움 요청한 학생
- 숫자 배지: 읽지 않은 메시지 개수
- 학생 카드 클릭 → 상세 정보 및 메시지 확인

### 메시지 관리
- 학생 카드의 숫자 배지 확인
- 카드 클릭 → 메시지 목록 확인
- **"읽음"** 버튼으로 일괄 읽음 처리

## 🎨 커스터마이징

### 색상 테마 변경
`dashboard.html` 내 CSS 변수를 수정하여 색상 테마를 변경할 수 있습니다:

```css
:root {
  --primary-color: #3b82f6;
  --success-color: #10b981;
  --warning-color: #f59e0b;
  --danger-color: #ef4444;
}
```

### 미션 개수 조정
기본적으로 5단계 미션을 지원하며, 필요에 따라 `dashboard.js`에서 조정 가능합니다.

## 🔧 Firebase 설정 가이드

### Firestore 컬렉션 구조
```
classData/{classId}/
├── students/{studentId}    # 학생 정보
├── groups/{groupId}        # 조 정보 (조별 모드)
└── missions/              # 미션 목록

studentMemos/              # 학생 메시지/메모
└── {memoId}              # 개별 메시지
```

### 보안 규칙 예시
```javascript
// Firestore Rules
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // 수업 데이터 읽기/쓰기 허용
    match /classData/{classId}/{document=**} {
      allow read, write: if true;
    }
    
    // 학생 메모 읽기/쓰기 허용  
    match /studentMemos/{memoId} {
      allow read, write: if true;
    }
  }
}
```

## 🐛 문제 해결

### 일반적인 문제들

**Firebase 연결 실패**
- Firebase 설정이 올바른지 확인
- 프로젝트 ID와 지역 설정 확인

**학생 페이지 접속 안됨**
- QR 코드의 URL이 올바른지 확인
- 로컬 서버가 실행 중인지 확인

**실시간 업데이트 안됨**
- Realtime Database가 활성화되어 있는지 확인
- 네트워크 연결 상태 확인

## 🤝 기여하기

1. Fork 후 브랜치 생성: `git checkout -b feature/새기능`
2. 변경사항 커밋: `git commit -am '새 기능 추가'`
3. 브랜치에 Push: `git push origin feature/새기능`
4. Pull Request 생성

## 📄 라이선스

이 프로젝트는 MIT 라이선스 하에 배포됩니다. 자세한 내용은 [LICENSE](LICENSE) 파일을 참조하세요.

## 👥 개발팀

- **메인 개발**: Claude (Anthropic AI Assistant)
- **기획 및 테스트**: 프로젝트 사용자

## 📞 지원

문제가 발생하거나 질문이 있으시면 GitHub Issues를 통해 문의해주세요.

---

**🎯 이 도구로 더 효과적인 수업을 만들어보세요!**