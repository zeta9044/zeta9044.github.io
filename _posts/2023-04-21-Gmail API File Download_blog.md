---
layout: post
title: "Gmail API File Download"
date: 2023-04-21
tags: [Gmail, API, Download, File]
categories: blog
---

## 사용자 입력:
> 파이썬으로 나의 gmail주소에 접속해서, 제목이 숫자로 시작되는 메일의 첨부파일이 있으면, 첨부파일명을 제목으로 만드는데, 왼쪽패딩에 3자리 0을 넣고, 확장자는 첨부파일명과 동일하게 저장하는 프로그램 만들어줘. 

## ChatGPT 응답:
> 

이 문제를 해결하기 위해서는, Gmail API를 사용하여 이메일 메시지를 가져오는 방법과, 첨부 파일을 다운로드하고 저장하는 방법을 알아야 합니다. 따라서 Gmail API를 사용하기 위해 Google Cloud Console에서 프로젝트를 만들고, 인증키를 생성해야 합니다.

인증키를 생성하고, 필요한 라이브러리를 설치한 후, 아래 코드를 실행해주세요. 이 코드는 Gmail API를 사용하여 이메일 메시지를 가져오고, 첨부 파일이 있는 메시지의 첨부 파일을 다운로드하고 저장합니다.

```python
import os
import base64
import re
from google.oauth2.credentials import Credentials
from googleapiclient.discovery import build
from googleapiclient.errors import HttpError

# 사용자 인증키 파일 경로
creds = Credentials.from_authorized_user_file('credentials.json', ['https://www.googleapis.com/auth/gmail.readonly'])

# Gmail API 클라이언트 생성
service = build('gmail', 'v1', credentials=creds)

# 제목이 숫자로 시작하는 이메일 검색
query = 'subject:[0-9]*'
result = service.users().messages().list(userId='me', q=query).execute()

if 'messages' in result:
    for msg in result['messages']:
        # 이메일 메시지 가져오기
        message = service.users().messages().get(userId='me', id=msg['id']).execute()
        headers = message['payload']['headers']
        subject = ''
        file_name = ''
        for header in headers:
            if header['name'] == 'Subject':
                subject = header['value']
            if header['name'] == 'Content-Disposition':
                match = re.search('filename="(.+)"', header['value'])
                if match:
                    file_name = match.group(1)
        # 첨부 파일이 있는 경우
        if file_name:
            # 첨부 파일 다운로드
            attachment = service.users().messages().attachments().get(userId='me', messageId=msg['id'], id=file_name).execute()
            # 파일 저장
            file_data = base64.urlsafe_b64decode(attachment['data'].encode('UTF-8'))
            file_ext = os.path.splitext(file_name)[1]
            new_file_name = '{:0>3d}{}'.format(int(subject), file_ext)
            with open(new_file_name, 'wb') as f:
                f.write(file_data)
``` 

위 코드를 실행하기 전에, `credentials.json` 파일을 만들어야 합니다. 이 파일은 Google Cloud Console에서 생성한 인증키 파일이어야 합니다. 

또한, 이 코드를 실행하기 위해서는 Google API 클라이언트 라이브러리가 필요합니다. 아래와 같이 라이브러리를 설치해주세요.

```
pip install --upgrade google-api-python-client google-auth google-auth-oauthlib google-auth-httplib2
```

이제 위 코드를 실행하면, 나의 Gmail 계정에 접속하여, 제목이 숫자로 시작하는 이메일을 검색합니다. 검색된 이메일 중 첨부 파일이 있는 경우, 첨부 파일을 다운로드하고, 파일 이름을 제목으로 변경하여 저장합니다

## 사용자 입력:
> gmail의 사용자 인증키(.json) 받는 법 알려줘.

## ChatGPT 응답:
> Gmail API를 사용하기 위해서는 Google Cloud Console에서 프로젝트를 만들고, 인증키를 생성해야 합니다. 아래는 사용자 인증키를 받는 방법입니다.

1. [Google Cloud Console](https://console.developers.google.com/)에 로그인합니다.
2. 프로젝트를 선택하거나 새 프로젝트를 만듭니다.
3. 왼쪽 상단의 네비게이션 메뉴에서 "API 및 서비스" > "대시보드"로 이동합니다.
4. "API 및 서비스 사용 설정" 버튼을 클릭합니다.
5. "Gmail API"를 검색하여 선택하고, "사용" 버튼을 클릭합니다.
6. "사용자 인증 정보" 탭으로 이동합니다.
7. "사용자 인증 정보 만들기" 버튼을 클릭합니다.
8. "어떤 API를 사용하시겠습니까?" 화면에서 "Gmail API"를 선택하고, "어떤 유형의 사용자 인증 정보가 필요하십니까?" 화면에서 "데스크톱 앱"을 선택합니다.
9. "OAuth 클라이언트 ID 만들기" 버튼을 클릭합니다.
10. "이름"을 입력하고, "만들기" 버튼을 클릭합니다.
11. "OAuth 클라이언트 ID" 페이지에서 "JSON 다운로드" 버튼을 클릭합니다.
12. 다운로드한 인증키 파일을 프로젝트 폴더에 저장합니다.

이제 다운로드한 인증키 파일을 사용하여 Gmail API에 접근할 수 있습니다. 다음과 같은 코드를 사용하여 인증키 파일을 로드할 수 있습니다.

```python
from google.oauth2.credentials import Credentials

creds = Credentials.from_authorized_user_file('credentials.json', ['https://www.googleapis.com/auth/gmail.readonly'])
```

여기서 `credentials.json`은 다운로드한 인증키 파일의 이름입니다. 이 파일은 프로젝트 폴더에 저장되어 있어야 합니다. 인증키 파일을 다른 이름으로 저장하거나 다른 경로에 저장하려면, `from_authorized_user_file()` 메서드의 첫 번째 인자를 적절히 수정하면 됩니다.

## 사용자 입력:
> 위 서비스 사용은 무료야?

## ChatGPT 응답:
> Gmail API를 사용하는 것은 무료가 아닙니다. Gmail API를 사용하려면 Google Cloud Console에서 프로젝트를 만들고, API를 사용하도록 설정해야 합니다. Google Cloud Console에서 사용 가능한 API에 대한 요금을 확인하려면 [Google Cloud Console의 요금 페이지](https://console.cloud.google.com/billing)를 방문해야 합니다.

Gmail API를 사용하려면 먼저 API를 활성화하고, API를 사용하도록 인증 및 권한 부여를 설정해야 합니다. 또한, API를 사용하여 데이터에 액세스하는 데 필요한 쿼터와 제한이 있을 수 있습니다. 일부 요청에 대한 비용이 발생할 수도 있습니다. 자세한 내용은 Google Cloud Console의 요금 페이지와 Gmail API의 [가격 책정 페이지](https://developers.google.com/gmail/api/v1/pricing)를 참조하시기 바랍니다.

## 사용자 입력:
> 네가 만든 코드에서, 사용자 email주소, 비밀번호 넣는 방법은 안돼?

## ChatGPT 응답:
> 맞습니다. 저는 Gmail API를 사용하여 Gmail 계정에 연결하는 코드를 제공했지만, 이 코드는 사용자의 이메일 주소와 비밀번호를 포함하지 않습니다. 

Gmail API를 사용하여 Gmail 계정에 연결하려면, Google 계정 로그인 정보를 사용하여 OAuth 2.0 인증을 구성해야 합니다. 이 방법은 Gmail API에서 권장되는 방법입니다. 

따라서 사용자 이메일 주소와 비밀번호를 직접 입력하지 않아도 됩니다. 대신, Google Cloud Console에서 OAuth 2.0 클라이언트 ID와 시크릿을 만들고, 인증 절차를 따르면 됩니다.

OAuth 2.0 인증을 구성하는 방법과 OAuth 2.0 클라이언트 ID와 시크릿을 만드는 방법은 매우 복잡하므로, 여기서 자세한 설명을 제공하지는 않겠습니다. Google Developers 사이트에서 Gmail API를 사용하는 방법과 OAuth 2.0 인증을 구성하는 방법에 대한 자세한 정보를 찾을 수 있습니다. 

다음은 Gmail API 공식 문서에서 OAuth 2.0 인증을 구성하는 방법에 대한 링크입니다.

- [Gmail API 인증 구성](https://developers.google.com/gmail/api/auth/about-auth)

## 사용자 입력:
> 네가 만든 코드에서, 사용자 email주소, 비밀번호 넣는 방법으로 알려줘

## ChatGPT 응답:
> 저는 이전에 말씀드린 대로, Gmail API를 사용하여 Gmail 계정에 연결하는 코드를 제공했습니다. 이 코드는 사용자의 이메일 주소와 비밀번호를 직접 입력하지 않습니다. 

그러나, Gmail API를 사용하지 않고, Gmail 계정에 직접 로그인하는 방법으로 이메일 메시지를 가져오려면, 사용자의 이메일 주소와 비밀번호를 사용하여 Gmail 계정에 로그인해야 합니다. 이를 위해서는 다음과 같은 코드를 사용할 수 있습니다.

```python
import imaplib
import email
from email.header import decode_header

# Gmail IMAP 서버에 연결
imap_server = imaplib.IMAP4_SSL('imap.gmail.com')

# 로그인 정보 입력
username = 'YOUR_EMAIL_ADDRESS'
password = 'YOUR_PASSWORD'

# Gmail 계정 로그인
imap_server.login(username, password)

# Inbox 폴더 선택
imap_server.select('Inbox')

# 모든 이메일 검색
status, messages = imap_server.search(None, 'ALL')
messages = messages[0].split(b' ')

# 각 이메일에 대해 처리
for msg in messages:
    # 이메일 본문 가져오기
    _, msg_data = imap_server.fetch(msg, '(RFC822)')
    msg_body = msg_data[0][1]
    email_msg = email.message_from_bytes(msg_body)
    # 이메일 제목 가져오기
    subject = decode_header(email_msg["Subject"])[0][0]
    if isinstance(subject, bytes):
        # 바이너리 문자열을 문자열로 변환
        subject = subject.decode()
    # 첨부 파일 확인
    for part in email_msg.walk():
        # multipart/* 내용 건너뛰기
        if part.get_content_maintype() == 'multipart':
            continue
        # 첨부 파일이 있는 경우
        if part.get('Content-Disposition') is not None:
            filename = decode_header(part.get_filename())[0][0]
            if isinstance(filename, bytes):
                # 바이너리 문자열을 문자열로 변환
                filename = filename.decode()
            # 파일 이름 출력
            print(f"첨부 파일: {filename}")
                
# IMAP 서버 종료
imap_server.close()
imap_server.logout()
```

이 코드에서 "YOUR_EMAIL_ADDRESS"와 "YOUR_PASSWORD"에 대한 값을 사용자의 Gmail 이메일 주소와 Gmail 비밀번호로 바꿔주어야 합니다. 

하지만, 이 코드에서 이메일을 가져오기 위해 Gmail IMAP 서버를 사용하고 있으므로, Google 보안 정책으로 인해 Gmail 계정에 로그인하려면 "보안 수준이 낮은 앱의 액세스"를 허용해야 합니다. 이 기능을 사용하도록 설정하려면, Gmail 계정 설정에서 "보안" 탭으로 이동하고, "보안 수준이 낮은 앱의 액세스"를 "사용"으로 설정하면 됩니다.

