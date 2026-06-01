# 텔레그램으로 Claude Code 쓰기

Claude Code는 원래 맥/PC 터미널에서만 써야 해.
그런데 텔레그램을 연결하면? **외출 중에도 스마트폰으로 Claude한테 말 걸 수 있어.**

"이 파일 요약해줘", "코드 리뷰해줘" — 카톡 보내듯이.

Anthropic이 2025년에 공식 출시한 **Claude Code Channels** 기능이야. 총 6단계, 15분이면 끝.

---

## STEP 1. 텔레그램 봇 만들기

텔레그램 앱을 열고 검색창에 `@BotFather` 검색.
BotFather 채팅방 들어가서 입력:

```
/newbot
```

① 봇 이름 입력 (예: `My Claude`)
② 봇 아이디 입력 (예: `myClaude_bot` — 반드시 `bot`으로 끝나야 해)

그러면 이런 메시지가 와:

```
Done! Congratulations on your new bot.
Token: 1234567890:ABCdefGHIjklMNOpqrSTUvwxyz
```

➡ **저 Token을 꼭 복사해놔.** 다음 단계에서 필요해.

---

## STEP 2. 텔레그램 플러그인 설치

맥미니 터미널에서 Claude Code 실행 후 입력:

```
/plugin install telegram@claude-plugins-official
/reload-plugins
```

첫 번째 줄이 설치, 두 번째 줄이 적용이야. 둘 다 입력해야 해.

---

## STEP 3. 봇 토큰 등록

Claude Code 안에서 입력:

```
/telegram:configure
```

아까 복사한 토큰 붙여넣기 → 엔터.
`~/.claude/channels/telegram/.env`에 자동 저장돼.

---

## STEP 4. 텔레그램 채널로 Claude 실행

이제 Claude를 실행할 때 텔레그램을 "듣는" 모드로 켜야 해.
기존 `claude` 대신 이렇게 실행해:

```bash
claude --channels plugin:telegram@claude-plugins-official
```

이 명령어로 켤 때마다 텔레그램 메시지를 받을 수 있어.

---

## STEP 5. 내 계정 페어링

① 텔레그램에서 내가 만든 봇 검색 (예: `@myClaude_bot`)
② 봇한테 아무 메시지나 보내 (예: `안녕`)
③ 봇이 **6자리 숫자 코드**를 답장해줘

④ 터미널에서 입력:

```
/telegram:access pair <받은 6자리 코드>
```

---

## STEP 6. 보안 설정 (이거 꼭 해야 해!)

페어링 직후 바로 입력:

```
/telegram:access policy allowlist
```

> ⚠️ 이걸 안 하면 **아무나 내 봇에 메시지를 보내서 Claude Code에 접근**할 수 있어.
> 봇은 기본적으로 공개 상태야. 무조건 이 설정 해줘.

---

## 완료! 테스트

텔레그램에서 봇한테 메시지 보내봐.
Claude가 답하면 성공! 🎉

---

## 주의사항

| 조건 | 이유 |
|------|------|
| 맥미니/PC 켜져 있어야 함 | Claude가 거기서 돌아가니까 |
| `claude --channels ...` 실행 중이어야 함 | 꺼져 있으면 응답 안 됨 |
| 절전모드 해제 권장 | 자다가 연결 끊기면 답장 못 해 |
