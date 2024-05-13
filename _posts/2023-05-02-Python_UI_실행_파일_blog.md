---
layout: post
title: "Python UI 실행 파일"
date: 2023-05-02
tags: [파일, Python, 실행, UI]
categories: blog
---

## 사용자 입력:
> 윈도우에서 python ui프로그램 만들었는데, 실행파일로 배포할려고해. 어떻게 해야돼?

## ChatGPT 응답:
> 

윈도우에서 Python으로 만든 UI 프로그램을 실행 가능한 실행 파일로 변환하려면 PyInstaller, cx_Freeze, Py2Exe 등의 패키지를 사용할 수 있습니다. 

여기서는 PyInstaller를 사용하는 방법을 알아보겠습니다.

1. PyInstaller 설치

먼저 PyInstaller를 설치해야 합니다. 명령 프롬프트(Windows + R, "cmd" 입력 후 Enter)에서 다음 명령어를 실행하여 설치합니다.

```
pip install pyinstaller
```

2. 실행 파일 생성

다음으로 PyInstaller를 사용하여 실행 파일을 생성합니다. 명령 프롬프트에서 프로그램이 있는 디렉토리로 이동한 후 다음 명령어를 실행합니다.

```
pyinstaller --onefile yourprogram.py
```

위 명령어에서 "yourprogram.py"는 변환할 Python 파일의 이름으로 바꿔주세요.

위 명령어를 실행하면, "dist" 폴더 안에 실행 가능한 실행 파일이 생성됩니다.

3. 추가 파일 및 디렉토리 포함

만약 프로그램에서 사용하는 이미지, 데이터 파일, 기타 파일이 있다면 실행 파일과 함께 포함시켜야 합니다. 이 경우에는 다음과 같은 방법으로 실행 파일을 생성합니다.

```
pyinstaller --onefile --add-data "images;images" yourprogram.py
```

위 명령어에서 "--add-data" 옵션으로 추가할 파일이 있는 디렉토리와 실행 파일 안에 어떤 위치에 포함시킬 것인지를 지정합니다. 위 예제에서는 "images" 디렉토리의 파일을 "images" 폴더에 포함시켜 실행 파일 안에 포함시킵니다.

이렇게 생성된 실행 파일을 사용자에게 배포하면 됩니다. 실행 파일을 더블 클릭하여 프로그램을 실행할 수 있습니다.

## 사용자 입력:
> 위에 내용을 실행시키면 뒤에 윈도우 cmd창이 뜨고 ui프로그램 뜨는데, ui프로그램만 뜨게 안돼나?

## ChatGPT 응답:
> PyInstaller로 만든 실행 파일에서 뒤에 나오는 cmd 창을 없애는 방법은 두 가지가 있습니다.

1. "--noconsole" 옵션 사용

PyInstaller 명령어 실행 시 "--noconsole" 옵션을 추가하면 cmd 창이 나오지 않습니다. 따라서 다음과 같이 명령어를 수정합니다.

```
pyinstaller --onefile --noconsole yourprogram.py
```

2. spec 파일 수정

PyInstaller로 실행 파일을 만들 때 자동으로 생성되는 spec 파일에서 console 속성을 False로 설정하면 cmd 창을 없앨 수 있습니다. 다음과 같이 명령어를 입력하여 spec 파일을 생성합니다.

```
pyinstaller yourprogram.py --name yourprogramname --onedir
```

위 명령어를 실행하면, "yourprogram.spec" 파일이 생성됩니다. 이 파일을 에디터로 열어서 다음과 같이 수정합니다.

```
a = Analysis(['yourprogram.py'],
             pathex=['C:\\path\\to\\your\\program'],
             binaries=[],
             datas=[],
             hiddenimports=[],
             hookspath=[],
             runtime_hooks=[],
             excludes=[],
             win_no_prefer_redirects=False,
             win_private_assemblies=False,
             cipher=block_cipher,
             console=False)  # 이 부분을 False로 수정
```

위와 같이 console 속성을 False로 설정하면 cmd 창이 나오지 않습니다.

수정이 끝나면 다음과 같은 명령어로 실행 파일을 생성합니다.

```
pyinstaller yourprogram.spec
```

이렇게 수정하면 PyInstaller로 생성한 실행 파일에서 cmd 창이 나오지 않습니다.

