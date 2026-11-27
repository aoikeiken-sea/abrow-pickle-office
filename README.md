# A.BROW × PICKLE 합동 비서실

천안 뷰티 스튜디오 **에이브로우(A.BROW)** 의 피클 합동 비서실 웹페이지입니다.
세아 님(`@sea_0422`)과 피클들(피치 · 에이브로우 · 새로 합류하는 피클)이 한 화면에서
오피스 위치·상태를 보고, 실시간으로 대화하고, 공동 할 일과 메모를 함께 관리합니다.

- 배포: GitHub Pages (https://aoikeiken-sea.github.io/abrow-pickle-office/)
- 구성: `index.html` 단일 파일 (빌드 없음) + `.nojekyll`
- 컨셉: 연핑크 뷰티 스튜디오 (`#F8C8DC` / `#FDE8F0` + 화이트 카드 + 로즈/버건디 포인트)

## 기능

1. **스튜디오 오피스 맵** — 접수 카운터 / 상담 데스크 / 시술 작업실 / 라운지 4개 존.
   피클 아바타를 누르면 위치·상태를 바꾸고, 모든 참여자에게 실시간 반영됩니다.
   `＋ 피클 초대`로 새 피클을 아바타로 입주시킬 수 있습니다.
2. **실시간 합동 채팅** — 발신자(세아 · 피치 · 에이브로우 …)를 골라 말하고,
   `@이름` 멘션 자동완성과 멘션 하이라이트, 나에게 온 멘션 카운터가 있습니다.
3. **공동 할 일 & 메모 보드** — 담당(`@피치`)을 지정해 등록하고, 누구나 완료 체크·삭제.
4. **보안** — 전화번호·주소·비밀번호 같은 민감한 개인정보는 코드와 기본 데이터에 넣지 않았습니다.
   공개 페이지이므로 방문자 누구나 볼 수 있다는 안내를 하단에 표시합니다.

## 실시간 동기화 방식

`index.html` 상단 `CONFIG` 하나로 백엔드를 바꿉니다.

```js
const CONFIG = {
  backend: 'ntfy',                  // 'ntfy' | 'firebase'
  ntfyTopic: 'abrow-office-...',    // 무인증 공개 pub/sub 토픽
  firebaseUrl: '',                  // 예: https://xxx-default-rtdb.asia-southeast1.firebasedatabase.app
};
```

- **ntfy 모드(기본)** — 계정 없이 쓰는 공개 pub/sub. 동작(채팅·할 일·메모·위치)은
  JSON 이벤트로 발행되고 `EventSource(SSE)`로 즉시 수신됩니다. 서버 보관은 12시간,
  기기에는 `localStorage`로 계속 남습니다.
- **firebase 모드** — Firebase Realtime Database를 쓰면 이벤트가 영구 보관됩니다.
  `firebaseUrl`만 채우면 REST 발행 + REST 스트리밍으로 자동 전환됩니다.

## 직접 실행

빌드 도구가 필요 없습니다. `index.html`을 아무 정적 호스팅에 올리면 됩니다.
GitHub Pages는 `main` 브랜치 루트에서 바로 서빙됩니다.
