# 🏠 가계부 - Firebase 실시간 공유 버전

남편과 함께 실시간으로 가계부를 공유할 수 있는 웹 애플리케이션입니다.
Firebase Realtime Database를 통해 데이터가 실시간으로 동기화됩니다.

## ✨ 주요 기능

- 📝 **가계부 기록**: 수입, 지출을 카테고리별로 기록
- 💰 **통계 분석**: 월별 수입/지출/잔액 통계
- 🔗 **실시간 공유**: Firebase를 통한 남편과의 실시간 데이터 동기화
- 👤 **계정 관리**: 이메일/비밀번호 인증
- 📊 **분류 정리**: 결제수단별, 카테고리별 자동 정렬

## 🚀 시작하기

### 1. Firebase 프로젝트 생성

1. [Firebase 콘솔](https://console.firebase.google.com/)에 접속
2. "새 프로젝트 만들기" 클릭
3. 프로젝트 이름 입력 (예: "household-ledger")
4. Google Analytics는 선택 해제 후 프로젝트 생성

### 2. Realtime Database 설정

1. Firebase 콘솔 → 좌측 메뉴 → **Realtime Database** 클릭
2. "데이터베이스 만들기" 클릭
3. 위치 선택 (asia-southeast1 권장)
4. 보안 규칙을 다음으로 설정:

```json
{
  "rules": {
    "ledgers": {
      "$uid": {
        ".read": "auth.uid === $uid || root.child('ledgers').child($uid).child('sharing').child(auth.token.email).exists()",
        ".write": "auth.uid === $uid",
        "records": {
          ".indexOn": ["date", "createdAt"]
        }
      }
    }
  }
}
```

### 3. 인증 설정

1. Firebase 콘솔 → **Authentication** 클릭
2. "시작하기" 클릭
3. **이메일/비밀번호** 방식 활성화

### 4. 웹 앱 연결

1. Firebase 콘솔 → 프로젝트 설정 (⚙️ 아이콘)
2. "내 앱" 탭 → **</> 웹 앱 추가**
3. 앱 별명 입력
4. 생성된 설정 코드 복사

### 5. 코드 수정

[가계부/index.html](가계부/index.html)의 아래 부분을 Firebase 설정으로 교체:

```javascript
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_AUTH_DOMAIN",
  databaseURL: "YOUR_DATABASE_URL",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_STORAGE_BUCKET",
  messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
  appId: "YOUR_APP_ID"
};
```

Firebase 콘솔에서 복사한 설정 값들을 대입하면 됩니다:
- `apiKey` → `Your API Key`
- `authDomain` → `projectId.firebaseapp.com`
- `databaseURL` → `https://projectId.firebaseio.com`
- `projectId` → `projectId`
- 나머지는 콘솔의 설정 값 복사

### 6. 사용

1. 가계부/index.html을 브라우저에서 열기
2. "회원가입" 클릭하여 계정 생성 (이메일/비밀번호)
3. 남편의 이메일로 계정 생성하도록 안내
4. 가계부 공유 섹션에서 남편의 이메일을 입력하여 "공유 요청" 클릭
5. 남편도 같은 이메일로 로그인하면 실시간 데이터 공유 시작!

## 📱 사용 방법

### 내역 추가
1. "➕ 내역 추가" 섹션에서 정보 입력
2. 날짜, 분류(지출/수입), 카테고리, 금액, 결제수단, 설명 입력
3. "저장하기" 클릭 (자동으로 Firebase에 저장되고 남편과 동기화)

### 공유
1. "📝 기록 목록" 섹션의 "🔗 남편과 공유하기"
2. 남편의 이메일 입력 후 "공유 요청" 클릭
3. 남편이 로그인하면 실시간으로 데이터 공유

### 통계
- 상단의 요약 카드에서 월별 수입/지출/잔액 확인
- 결제수단별, 카테고리별 통계 자동 생성

## 🔒 보안 주의사항

- **API Key 노출 금지**: 개인 프로젝트 또는 공개하지 않는 용도로만 사용
- **비밀번호 보관**: Firebase 콘솔의 사용자 인증정보는 외부에 공유 금지
- **데이터 백업**: 정기적으로 데이터 백업 권장

## 📋 커스터마이징

### 카테고리 추가
가계부/index.html의 다음 부분 수정:

```html
<select id="category">
  <option value="식비">식비</option>
  <option value="교통">교통</option>
  <!-- 여기에 추가 -->
  <option value="새로운카테고리">새로운카테고리</option>
</select>
```

### 결제수단 추가
```html
<select id="payment">
  <option value="현금">현금</option>
  <option value="카드">카드</option>
  <!-- 여기에 추가 -->
  <option value="새로운수단">새로운수단</option>
</select>
```

## 🐛 문제 해결

### "Firebase 설정이 필요합니다" 에러
- Firebase 설정이 정확하게 입력되었는지 확인
- 브라우저 개발자 도구 콘솔(F12)에서 오류 메시지 확인

### 데이터가 저장되지 않음
- 로그인 상태 확인
- Realtime Database의 보안 규칙 확인
- 브라우저 콘솔의 오류 메시지 확인

### 공유가 안 됨
- 남편의 이메일 정확성 확인
- 남편이 해당 이메일로 로그인했는지 확인

## 📞 지원

문제가 발생하면:
1. 브라우저 개발자 도구 콘솔(F12)에서 오류 확인
2. Firebase 콘솔에서 데이터베이스/인증 상태 확인
3. 보안 규칙이 올바르게 설정되었는지 재확인

## 📝 라이선스

개인 사용 목적의 오픈 프로젝트입니다.
