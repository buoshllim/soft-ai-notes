# 텔레그램으로 Claude Code 쓰기

Claude Code를 텔레그램과 연결하면 **외출 중에도 스마트폰으로 Claude한테 말 걸 수 있음.**

"이 파일 요약해줘", "코드 리뷰해줘" — 카톡 보내듯이.

Anthropic이 2025년에 공식 출시한 **Claude Code Channels** 기능임.

---

## STEP 1. 텔레그램 봇 만들기

텔레그램 앱을 열고 검색창에 `@BotFather` 검색.

<img src="/images/botfather-search.png" alt="BotFather 검색 화면" style="width: 320px; border: 1px solid #ddd; border-radius: 8px; margin-bottom: 24px;" />

> 파란 체크(인증 마크) 붙은 **BotFather**만 맞음. 비슷한 이름의 다른 봇에 들어가면 안 됨.


BotFather 채팅방 들어가서 입력:

```
/newbot
```

① 봇 이름 입력 (예: `My Claude`)
② 봇 아이디 입력 (예: `myClaude_bot` — 반드시 `bot`으로 끝나야 함)


그러면 이런 메시지가 옴:

```
Done! Congratulations on your new bot.
Token: 1234567890:ABCdefGHIjklMNOpqrSTUvwxyz
```

➡ **이 Token을 숫자부터 모두 복사해놔야 함.** 다음 단계에서 필요함.

---

## STEP 2. 텔레그램 플러그인 설치

맥 터미널에서 **일반 모드**로 Claude Code 실행 (`claude` 입력) 후, 아래 두 명령어를 **순서대로 하나씩** 입력:

```
/plugin install telegram@claude-plugins-official
```

> 플러그인을 찾을 수 없다는 오류가 나면 마켓플레이스를 먼저 추가해야 함:
> ```
> /plugin marketplace add anthropics/claude-plugins-official
> ```
> 그 다음 다시 설치 시도.

설치가 완료되면 이어서:

```
/reload-plugins
```

---

## STEP 3. 봇 토큰 등록

같은 세션에서 (아직 일반 모드), 아래 형식으로 입력 — 토큰을 바로 뒤에 붙여서 한 줄로:

```
/telegram:configure 여기에토큰붙여넣기
```

예시:
```
/telegram:configure 1234567890:ABCdefGHIjklMNOpqrSTUvwxyz
```

`~/.claude/channels/telegram/.env`에 자동 저장됨.

---

## STEP 4. 텔레그램 채널로 Claude 재시작

지금까지 쓰던 Claude를 종료(`/exit` 또는 Ctrl+C)하고, 아래 명령어로 다시 시작:

```bash
claude --channels plugin:telegram@claude-plugins-official
```

> ⚠️ **이 창을 닫지 말고 열어둬야 함.** 이 상태에서 바로 아래 STEP 5로 이어서 진행.

---

## STEP 5. 내 계정 페어링

① 텔레그램에서 내가 만든 봇 검색 (예: `@myClaude_bot`)
② 봇한테 아무 메시지나 보내기 (예: `안녕`)
③ 봇이 **6자리 숫자 코드**를 답장해줌

④ 다시 맥 터미널로 돌아와서 입력:

```
/telegram:access pair <받은 6자리 코드>
```

---

## STEP 6. 보안 설정 (이거 꼭 하기를 추천!)

페어링 직후 맥에서 바로 입력:

```
/telegram:access policy allowlist
```

> ⚠️ 이걸 안 하면 **아무나 내 봇에 메시지를 보내서 Claude Code에 접근**할 수 있음.
> 봇은 기본적으로 공개 상태이므로. 무조건 이 설정 해주는 것이 좋음.

---

## 완료! 테스트

텔레그램에서 봇한테 메시지 보내 보기!
Claude가 답하면 성공! 🎉

---

## 주의사항

| 조건 | 이유 |
|------|------|
| 맥/PC 켜져 있어야 함 | Claude가 거기서 돌아가기 때문 |
| `claude --channels ...` 실행 중이어야 함 | 꺼져 있으면 응답 안 됨 |
| 절전모드 해제 권장 | 자다가 연결 끊기면 답장 못함 |
