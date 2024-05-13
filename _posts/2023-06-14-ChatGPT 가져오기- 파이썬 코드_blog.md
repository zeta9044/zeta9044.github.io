---
layout: post
title: "ChatGPT 가져오기: 파이썬 코드"
date: 2023-06-14
tags: [ChatGPT, 파이썬, 코드]
categories: blog
---

## 사용자 입력:
> 웹에서 chatgpt와 대화한 결과가 history로 today,yesterday,previous 7days이렇게 나오는데, 이걸 가져오는 파이썬 프로그램 만들어줘.

## ChatGPT 응답:
> 물론이죠! 파이썬을 사용하여 웹에서 ChatGPT와 대화 기록을 가져오는 프로그램을 만들어드리겠습니다. 프로그램은 requests 라이브러리를 사용하여 웹 페이지에서 대화 기록을 가져올 것입니다.

먼저, 필요한 라이브러리를 설치해야 합니다. 파이썬에서 requests 라이브러리를 설치하려면 다음 명령을 실행하세요:

```
pip install requests
```

이제 다음은 ChatGPT 대화 기록을 가져오는 파이썬 프로그램의 예시입니다:

```python
import requests

def get_chat_history(date):
    url = f"https://example.com/chat_history?date={date}"  # ChatGPT 대화 기록이 있는 URL로 변경해야 합니다.
    response = requests.get(url)
    
    if response.status_code == 200:
        return response.text
    else:
        return None

# 오늘 대화 기록 가져오기
today_history = get_chat_history("today")
print("Today's Chat History:")
print(today_history)

# 어제 대화 기록 가져오기
yesterday_history = get_chat_history("yesterday")
print("Yesterday's Chat History:")
print(yesterday_history)

# 지난 7일간의 대화 기록 가져오기
previous_7days_history = get_chat_history("previous_7days")
print("Previous 7 Days' Chat History:")
print(previous_7days_history)
```

위 코드에서 `https://example.com/chat_history?date={date}` 부분을 실제 ChatGPT 대화 기록이 있는 URL로 바꾸어야 합니다. 또한, 해당 URL에서 올바른 응답을 받을 수 있도록 인증이나 세션 관리 등이 필요한 경우 해당 로직을 추가해야 합니다.

이 예시를 참고하여 웹에서 ChatGPT 대화 기록을 가져올 수 있는 파이썬 프로그램을 작성해보세요. 필요에 따라 코드를 수정하여 사용하시면 됩니다.

