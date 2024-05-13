---
layout: post
title: "QR Code Generator GUI"
date: 2023-04-03
tags: [Generator, QR, GUI, Code]
categories: blog
---

## 사용자 입력:
> from PIL import Image
import qrcode
import os
from pyzbar.pyzbar import decode, ZBarSymbol

# QR 코드 이미지 저장 디렉토리 경로
qr_dir = "qr_codes"
if not os.path.exists(qr_dir):
    os.makedirs(qr_dir)

# QR 코드 이미지와 텍스트 파일이 이미 존재한다면 삭제
if os.path.exists(qr_dir):
    for file_name in os.listdir(qr_dir):
        os.remove(os.path.join(qr_dir, file_name))
else:
    os.makedirs(qr_dir)

if os.path.exists("restored.txt"):
    os.remove("restored.txt")

# 텍스트 파일 열기
with open('datastage_sample.xml', 'r', encoding='utf-8') as f:
    text = f.read()

# # QR 코드 생성 및 이미지 저장
split_size = 2200 # 한 장당 저장될 문자열 길이
qr_dir = "qr_codes"
if not os.path.exists(qr_dir):
    os.makedirs(qr_dir)
idx = 0 # 이미지 파일 이름에 사용될 인덱스
while text:
    # 텍스트에서 한 번에 저장될 문자열 분리
    data = text[:split_size]
    text = text[split_size:]
    
    # QR 코드 이미지 생성
    img = qrcode.make(data)
    img.save(os.path.join(qr_dir, f"{idx}_qr_code.png"))

    # 인덱스 증가
    idx += 1

위 코드는 CMD로 작동하는데, 사용자편의를 위해서 GUI프로그램으로 만드는 것이 필요하다. QR이미지로 만들 파일과 qr코드 저장할 위치를  윈도우선택창에서 선택하고 처리하는 GUI프로그램 만들어줘.


## ChatGPT 응답:
> 

아래는 Python tkinter 라이브러리를 사용하여 구현한 예시 코드입니다. 사용자가 파일과 저장 위치를 선택하면, 선택한 파일에서 데이터를 읽어와 QR 코드 이미지를 생성하고 지정된 위치에 저장합니다.

```python
import tkinter as tk
from tkinter import filedialog
from PIL import Image
import qrcode
import os

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            img = qrcode.make(data)
            img.save(os.path.join(self.qr_dir, f"{idx}_qr_code.png"))

            # 인덱스 증가
            idx += 1
            
        tk.messagebox.showinfo("QR Codes Generated", f"{idx} QR codes generated and saved to {self.qr_dir}")
        
    def start(self):
        self.window.mainloop()

if __name__ == "__main__":
    app = QRCodeGenerator()
    app.start()
```

위 코드를 실행하면 파일 선택 버튼과 저장 위치 선택 버튼이 표시됩니다. 각 버튼을 클릭하면 파일 선택 창과 폴더 선택 창이 열리고, 사용자가 파일과 저장 위치를 선택할 수 있습니다. 그 다음 Generate QR Codes 버튼을 클릭하면 선택한 파일에서 데이터를 읽어와 QR 코드 이미지를 생성하고 지정된 위치에 저장합니다. 마지막으로, QR 코드가 몇 개 생성되었는지와 저장 위치가 메시지 박스로

## 사용자 입력:
> 네가 만든 위의 코드에서 저장할 QR코드디렉토리 아래, 파일명으로 디렉토리를 만들고 그 안에 QR코드를 만들고, 처리내용을 보여주는 로그창을 추가해줘.

## ChatGPT 응답:
> 아래는 파일명으로 디렉토리를 만들고, 생성된 QR 코드를 해당 디렉토리에 저장하고, 처리 내용을 보여주는 로그창을 추가한 코드입니다. 로그창에는 QR 코드 생성 과정에서 발생한 오류 메시지도 표시됩니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR

## 사용자 입력:
> 위 코드 완성이 중단됐어. 완성해줘.

## ChatGPT 응답:
> 죄송합니다. 코드 작성 중간에 누락된 부분이 있었습니다. 아래는 해당 부분을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
    def start(self):
        self.window.mainloop()

if __name__ == "__main__":
    app = QRCodeGenerator()
    app.start()
``` 

위 코드

## 사용자 입력:
> 위에 코드에 생성된 파일명의 qr코드 디렉토리 안에  qr코드내용을 선택한 디렉토리아래로 파일명으로 된 qr코드디렉토리명의 파일로 만들어주는 기능을 추가해줘.

## ChatGPT 응답:
> 아래는 생성된 QR 코드 이미지를 선택한 디렉토리에 파일명으로 된 디렉토리를 만들고, 해당 디렉토리에 QR 코드 이미지를 저장하는 기능을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드

## 사용자 입력:
> 코드를 만들다가 중단됐어. 코드를 완성해줘.

## ChatGPT 응답:
> 죄송합니다. 다시 코드를 작성 중간에 누락된 부분이 있었습니다. 아래는 해당 부분을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir

## 사용자 입력:
> 자꾸 중단되네...코드를 완성해줘.

## ChatGPT 응답:
> 죄송합니다. 코드 작성 중간에 누락된 부분이 있었습니다. 아래는 해당 부분을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{

## 사용자 입력:
> 자꾸 중단되네....완성해줘.

## ChatGPT 응답:
> 죄송합니다. 코드 작성 중간에 누락된 부분이 있었습니다. 아래는 해당 부분을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{

## 사용자 입력:
> 이어서 계속 완성해줘.

## ChatGPT 응답:
> 이어서 완성된 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
        for file_name in os.listdir(dir_name):


## 사용자 입력:
> 이어서 계속 완성해줘.

## ChatGPT 응답:
> 이어서 완성된 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
        for file_name in os.listdir(dir_name):


## 사용자 입력:
> #QR 코드 디렉토리 이동에서 계속 끊기는데 그 부분부터 이어서 완성해줘.

## ChatGPT 응답:
> 죄송합니다. 아래는 해당 부분을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
       

## ChatGPT 응답:
> 죄송합니다. 코드 작성 중간에 누락된 부분이 있었습니다. 아래는 해당 부분을 추가한 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{

## ChatGPT 응답:
> 이어서 완성된 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
        for file_name in os.listdir(dir_name):


## 사용자 입력:
>         # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
        for file_name in os.listdir(dir_name):  이 부분부터 나머지 코드 완성해 줄래?

## ChatGPT 응답:
> 네, 이어서 완성된 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("Text files", "*.txt")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_without_ext = os.path.splitext(os.path.basename(self.file_path))[0]
        dir_name = os.path.join(self.qr_dir, filename_without_ext)
        os.makedirs(dir_name, exist_ok=True)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")

            # 인덱스 증가
            idx += 1
        
        self.log_text.insert(tk.END, f"{idx} QR codes generated and saved to {dir_name}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.qr_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
        for file_name in os.listdir

## 사용자 입력:
> # 디렉토리에 있는 모든 QR 코드 이미지 파일 읽어들이기
for file_name in sorted(os.listdir(qr_dir)):
    if file_name.endswith(".png") or file_name.endswith(".jpg"):
        # QR 코드 이미지 파일 불러오기
        img = Image.open(os.path.join(qr_dir, file_name))

        # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
        qr_decoded = decode(img, symbols=[ZBarSymbol.QRCODE])
        qr_text_list.append(qr_decoded[0].data.decode("utf-8"))

# 추출된 텍스트 리스트를 하나의 텍스트 파일로 합치기
with open("restored.txt", "w", encoding="utf-8") as f:
    f.write("".join(qr_text_list))  이 코드는 특정디렉토리에 있는 qr코드를 하나의 파일로 만들어주는 코드이다. 사용자 편의를 위해서, 텍스트로 변환할 qr코드가 있는 특정디렉토리를 윈도우창으로 선택하고, 변환뒤에 저장할 디렉토리를 윈도우창으로 선택하고, 변환을 완료하면, 선택한 qr코드 디렉토리명으로 파일을 저장하는 GUI프로그램 만들어줘. 

## ChatGPT 응답:
> 이어서 완성된 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil
from pyzbar.pyzbar import decode, ZBarSymbol


class QRCodeDecoder:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Decoder")
        
        # QR 코드 디렉토리 선택 버튼
        self.select_qr_dir_button = tk.Button(
            self.window, text="Select QR Code Directory", command=self.select_qr_directory)
        self.select_qr_dir_button.pack(padx=10, pady=10)
        
        # 텍스트 파일 저장 위치 선택 버튼
        self.select_save_dir_button = tk.Button(
            self.window, text="Select Save Directory", command=self.select_save_directory)
        self.select_save_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 디코딩 버튼
        self.decode_button = tk.Button(
            self.window, text="Decode QR Codes", command=self.decode_qr_codes)
        self.decode_button.pack(padx=10, pady=10)
        
        self.qr_dir = None
        self.save_dir = None
        
    def select_qr_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select QR Code Directory")
        
    def select_save_directory(self):
        self.save_dir = filedialog.askdirectory(
            title="Select Directory to Save Decoded Text")
        
    def decode_qr_codes(self):
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a QR code directory.")
            return
        if not self.save_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save decoded text.")
            return
        
        # QR 코드 디렉토리 내 파일 목록 가져오기
        qr_code_files = []
        for file_name in os.listdir(self.qr_dir):
            file_path = os.path.join(self.qr_dir, file_name)
            if os.path.isfile(file_path) and (file_name.endswith(".png") or file_name.endswith(".jpg")):
                qr_code_files.append(file_path)
                
        if not qr_code_files:
            self.log_text.insert(tk.END, "No QR code images found in the directory.\n")
            return
        
        # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
        qr_text_list = []
        for file_path in sorted(qr_code_files):
            try:
                # QR 코드 이미지 파일 불러오기
                img = Image.open(file_path)

                # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
                qr_decoded = decode(img, symbols=[ZBarSymbol.QRCODE])
                qr_text_list.append(qr_decoded[0].data.decode("utf-8"))
                
                self.log_text.insert(tk.END, f"QR code decoded from {file_path}\n")
                
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while decoding {file_path}: {e}\n")
        
        # 추출된 텍스트 리스트를 하나의 텍스트 파일로 합치기
        filename_without_ext = os.path.basename(self.qr_dir)
        file_path = os.path.join(self.save_dir, f

## 사용자 입력:
> 코드가 중간에 자꾸 잘린다.

## ChatGPT 응답:
> 죄송합니다. 코드가 중간에 자동으로 잘리는 문제가 있어, 한 번에 긴 코드를 보내지 못했습니다. 이어서 코드를 보내드리겠습니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil
from pyzbar.pyzbar import decode, ZBarSymbol


class QRCodeDecoder:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Decoder")
        
        # QR 코드 디렉토리 선택 버튼
        self.select_qr_dir_button = tk.Button(
            self.window, text="Select QR Code Directory", command=self.select_qr_directory)
        self.select_qr_dir_button.pack(padx=10, pady=10)
        
        # 텍스트 파일 저장 위치 선택 버튼
        self.select_save_dir_button = tk.Button(
            self.window, text="Select Save Directory", command=self.select_save_directory)
        self.select_save_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 디코딩 버튼
        self.decode_button = tk.Button(
            self.window, text="Decode QR Codes", command=self.decode_qr_codes)
        self.decode_button.pack(padx=10, pady=10)
        
        self.qr_dir = None
        self.save_dir = None
        
    def select_qr_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select QR Code Directory")
        
    def select_save_directory(self):
        self.save_dir = filedialog.askdirectory(
            title="Select Directory to Save Decoded Text")
        
    def decode_qr_codes(self):
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a QR code directory.")
            return
        if not self.save_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save decoded text.")
            return
        
        # QR 코드 디렉토리 내 파일 목록 가져오기
        qr_code_files = []
        for file_name in os.listdir(self.qr_dir):
            file_path = os.path.join(self.qr_dir, file_name)
            if os.path.isfile(file_path) and (file_name.endswith(".png") or file_name.endswith(".jpg")):
                qr_code_files.append(file_path)
                
        if not qr_code_files:
            self.log_text.insert(tk.END, "No QR code images found in the directory.\n")
            return
        
        # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
        qr_text_list = []
        for file_path in sorted(qr_code_files):
            try:
                # QR 코드 이미지 파일 불러오기
                img = Image.open(file_path)

                # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
                qr_decoded = decode(img, symbols=[ZBarSymbol.QRCODE])
                qr_text_list.append(qr_decoded[0].data.decode("utf-8"))
                
                self.log_text.insert(tk.END, f"QR code decoded from {file_path}\n")
                
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while decoding {file_path}: {e}\n")
        
        # 추출

## 사용자 입력:
> 완성해줘....

## ChatGPT 응답:
> 이어서 코드를 보내드리겠습니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
from PIL import Image
import qrcode
import os
import shutil
from pyzbar.pyzbar import decode, ZBarSymbol


class QRCodeDecoder:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Decoder")
        
        # QR 코드 디렉토리 선택 버튼
        self.select_qr_dir_button = tk.Button(
            self.window, text="Select QR Code Directory", command=self.select_qr_directory)
        self.select_qr_dir_button.pack(padx=10, pady=10)
        
        # 텍스트 파일 저장 위치 선택 버튼
        self.select_save_dir_button = tk.Button(
            self.window, text="Select Save Directory", command=self.select_save_directory)
        self.select_save_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 디코딩 버튼
        self.decode_button = tk.Button(
            self.window, text="Decode QR Codes", command=self.decode_qr_codes)
        self.decode_button.pack(padx=10, pady=10)
        
        self.qr_dir = None
        self.save_dir = None
        
    def select_qr_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select QR Code Directory")
        
    def select_save_directory(self):
        self.save_dir = filedialog.askdirectory(
            title="Select Directory to Save Decoded Text")
        
    def decode_qr_codes(self):
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a QR code directory.")
            return
        if not self.save_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save decoded text.")
            return
        
        # QR 코드 디렉토리 내 파일 목록 가져오기
        qr_code_files = []
        for file_name in os.listdir(self.qr_dir):
            file_path = os.path.join(self.qr_dir, file_name)
            if os.path.isfile(file_path) and (file_name.endswith(".png") or file_name.endswith(".jpg")):
                qr_code_files.append(file_path)
                
        if not qr_code_files:
            self.log_text.insert(tk.END, "No QR code images found in the directory.\n")
            return
        
        # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
        qr_text_list = []
        for file_path in sorted(qr_code_files):
            try:
                # QR 코드 이미지 파일 불러오기
                img = Image.open(file_path)

                # QR 코드 디코딩하여 추출된 텍스트 리스트에 추가
                qr_decoded = decode(img, symbols=[ZBarSymbol.QRCODE])
                qr_text_list.append(qr_decoded[0].data.decode("utf-8"))
                
                self.log_text.insert(tk.END, f"QR code decoded from {file_path}\n")
                
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while decoding {file_path}: {e}\n")
        
        # 추출된 텍스트 리스트를 하나의 텍스트 파일로 합치기
        filename_without_ext = os.path.basename(self.qr_dir)
        file_path = os.path.join(self.save

## 사용자 입력:
> 완성해줘...자꾸 끊긴다.

## ChatGPT 응답:
> 죄송합니다. 코드가 중간에 잘린 것 같습니다. 이어서 코드를 보내드리겠습니다.

```python
        # 추출된 텍스트 리스트를 하나의 텍스트 파일로 합치기
        filename_without_ext = os.path.basename(self.qr_dir)
        file_path = os.path.join(self.save_dir, f"{filename_without_ext}.txt")
        with open(file_path, "w", encoding="utf-8") as f:
            f.write("".join(qr_text_list))
            
        self.log_text.insert(tk.END, f"Decoded text saved to {file_path}\n")
        
        # QR 코드 디렉토리 이동
        qr_dir_name = os.path.join(self.save_dir, f"{filename_without_ext}_qr_codes")
        os.makedirs(qr_dir_name, exist_ok=True)
        for file_name in os.listdir(self.qr_dir):
            if file_name.endswith(".png") or file_name.endswith(".jpg"):
                shutil.copyfile(os.path.join(self.qr_dir, file_name), os.path.join(qr_dir_name, file_name))
        
        self.log_text.insert(tk.END, f"QR code images copied to {qr_dir_name}\n")
        
        tk.messagebox.showinfo("Decoding Complete", "QR code decoding completed successfully.")
        
    def run(self):
        self.window.mainloop()

if __name__ == "__main__":
    decoder = QRCodeDecoder()
    decoder.run()
```

위 코드는 특정 디렉토리 내에 있는 QR 코드 이미지 파일들을 읽어들여, 디코딩하여 추출된 텍스트들을 하나의 텍스트 파일로 저장하고, QR 코드 이미지 파일들을 선택한 위치에 디렉토리 이름으로 저장하는 GUI 프로그램입니다. 코드를 실행하면, "Select QR Code Directory" 버튼과 "Select Save Directory" 버튼이 있는 창이 나타납니다. 각각의 버튼을 눌러서 QR 코드 이미지 파일들이 있는 디렉토리와 추출된 텍스트 파일을 저장할 디렉토리를 선택해줍니다. "Decode QR Codes" 버튼을 누르면, QR 코드 디코딩과 추출된 텍스트 파일 저장, 그리고 QR 코드 이미지 파일 디렉토리 저장이 시작됩니다. 각각의 작업이 완료되면 로그 창에 결과가 표시됩니다.

## 사용자 입력:
> 파이썬에서 1을 001 3자리의 문자열로 만들려면  f"{idx}_qr_code.png" 어떻게 코딩

## ChatGPT 응답:
> Python에서 1을 001로 표현하기 위해서는 f-string에서 format specification mini-language를 사용하여 문자열 포맷을 지정할 수 있습니다. 

아래는 예시 코드입니다.

```python
idx = 1
file_name = f"{idx:03d}_qr_code.png"
print(file_name) # 출력 결과: "001_qr_code.png"
```

위 코드에서 `:03d`는 문자열 포맷을 지정하기 위한 구문입니다. 여기서 `0`은 숫자를 0으로 채우고, `3`은 총 3자리 숫자를 표시하도록 하며, `d`는 정수형 숫자를 나타냅니다. 따라서 `1`을 `001`로 표시하기 위해서는 `:03d`을 사용하면 됩니다. 이를 활용하여 `f"{idx:03d}_qr_code.png"`와 같이 코딩하면 됩니다.

## 사용자 입력:
> 파이썬에서 스크롤이 되는 log창을 만들고 싶어... 아래와 같은 코드를 어떻게 바꿔야 돼?         self.log_text = tk.Text(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)

## ChatGPT 응답:
> Tkinter에서 스크롤이 가능한 로그창을 만들기 위해서는 `tkinter.scrolledtext` 모듈을 사용할 수 있습니다. 이 모듈을 사용하면 일반적인 텍스트 위젯과 동일한 방식으로 사용할 수 있지만 스크롤바가 자동으로 추가됩니다.

아래는 예시 코드입니다.

```python
import tkinter as tk
from tkinter import scrolledtext

class MyApplication:
    def __init__(self):
        self.window = tk.Tk()

        # 스크롤이 가능한 로그창
        self.log_text = scrolledtext.ScrolledText(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)

        # 버튼
        self.button = tk.Button(self.window, text="Click me", command=self.log_message)
        self.button.pack(padx=10, pady=10)

    def log_message(self):
        self.log_text.insert(tk.END, "Button clicked\n")
        self.log_text.see(tk.END)  # 자동으로 스크롤을 맨 아래로 이동

    def run(self):
        self.window.mainloop()

if __name__ == "__main__":
    app = MyApplication()
    app.run()
```

위 코드에서 `scrolledtext.ScrolledText`를 이용하여 스크롤이 가능한 로그창을 만들고, `self.log_text.see(tk.END)`를 이용하여 로그창이 자동으로 스크롤 되도록 했습니다.

## 사용자 입력:
> 진행되는 로그를 스크롤창에서 보고 싶으면?

## ChatGPT 응답:
> 스크롤이 가능한 로그창에서는 스크롤바를 사용하여 로그를 스크롤해서 볼 수 있습니다. 스크롤바가 자동으로 추가되기 때문에 별도의 구현이 필요하지 않습니다.

스크롤바를 사용하여 로그를 스크롤해서 보려면 스크롤바가 포함된 프레임 위젯과 그 프레임 위젯에 스크롤이 가능한 로그창을 생성하면 됩니다. 아래는 예시 코드입니다.

```python
import tkinter as tk
from tkinter import ttk

class MyApplication:
    def __init__(self):
        self.window = tk.Tk()

        # 스크롤이 가능한 로그창을 위한 프레임 생성
        log_frame = ttk.Frame(self.window)
        log_frame.pack(padx=10, pady=10)

        # 스크롤바를 포함한 스크롤이 가능한 로그창 생성
        self.log_scrollbar = ttk.Scrollbar(log_frame)
        self.log_text = tk.Text(log_frame, width=60, height=10, yscrollcommand=self.log_scrollbar.set)
        self.log_scrollbar.config(command=self.log_text.yview)
        self.log_scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
        self.log_text.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

        # 버튼
        self.button = tk.Button(self.window, text="Click me", command=self.log_message)
        self.button.pack(padx=10, pady=10)

    def log_message(self):
        self.log_text.insert(tk.END, "Button clicked\n")
        self.log_text.see(tk.END)

    def run(self):
        self.window.mainloop()

if __name__ == "__main__":
    app = MyApplication()
    app.run()
```

위 코드에서 `ttk.Scrollbar`와 `self.log_text` 위젯을 각각 생성하여 스크롤이 가능한 로그창을 만들었습니다. `yscrollcommand`와 `config`를 이용하여 스크롤바를 로그창과 연동시켜서 스크롤바를 사용하여 로그를 스크롤해서 볼 수 있도록 했습니다.

## 사용자 입력:
>             self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")
실시간으로 로그를 보려면

## ChatGPT 응답:
> 실시간으로 로그를 보려면, 로그 메시지를 추가할 때마다 `self.log_text.see(tk.END)`를 호출하면 됩니다. 이 함수는 로그창을 자동으로 스크롤하여 가장 마지막 로그를 보여주게 됩니다.

아래는 예시 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, messagebox
import qrcode
import os
from pyzbar.pyzbar import decode, ZBarSymbol

class QRCodeConverter:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Converter")

        # QR 코드 변환 버튼
        self.convert_button = tk.Button(self.window, text="Convert to QR Code", command=self.convert_to_qr)
        self.convert_button.pack(padx=10, pady=10)

        # QR 코드 변환 로그
        self.log_frame = tk.Frame(self.window)
        self.log_frame.pack(padx=10, pady=10)

        self.log_text = tk.Text(self.log_frame, width=60, height=10)
        self.log_text.pack(side=tk.LEFT, fill=tk.BOTH, expand=True)

        self.log_scrollbar = tk.Scrollbar(self.log_frame, command=self.log_text.yview)
        self.log_scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
        self.log_text.config(yscrollcommand=self.log_scrollbar.set)

        self.qr_dir = ""

    def convert_to_qr(self):
        # 파일 선택 대화상자 열기
        file_path = filedialog.askopenfilename()
        if not file_path:
            messagebox.showwarning("Error", "Please select a file.")
            return

        # QR 코드 저장 디렉토리 선택 대화상자 열기
        self.qr_dir = filedialog.askdirectory()
        if not self.qr_dir:
            messagebox.showwarning("Error", "Please select a directory to save QR codes.")
            return

        # QR 코드 이미지 생성
        with open(file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        qr_text_list = []
        idx = 0
        split_size = 2200
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            img = qrcode.make(data)
            file_name = f"{idx:03d}_qr_code.png"
            file_path = os.path.join(self.qr_dir, file_name)
            img.save(file_path)

            # 로그 메시지 추가 및 스크롤
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")
            self.log_text.see(tk.END)

            idx += 1

        messagebox.showinfo("Success", "QR codes were successfully created.")

    def run(self):
        self.window.mainloop()

if __name__ == "__main__":
    app = QRCodeConverter()
    app.run()
```

위 코드에서 `self.log_text.see(tk.END)`를 호출하여 로그창을 스크롤하게 만들었습니다. 각각의 로그 메시지가 추가될 때마다 자동으로 스크롤되어 마지막 로그를 보여줍니다.

## 사용자 입력:
> import tkinter as tk
from tkinter import filedialog,scrolledtext
import qrcode
import os

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = scrolledtext.ScrolledText(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("All files", "*.*")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
       
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_with_ext = os.path.basename(self.file_path)
        dir_name = os.path.join(self.qr_dir, filename_with_ext)
        
        # QR 코드 이미지와 텍스트 파일이 이미 존재한다면 삭제
        if os.path.exists(dir_name):
            for file_name in os.listdir(dir_name):
                os.remove(os.path.join(dir_name, file_name))
        else:
            os.makedirs(dir_name)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx:03d}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")
            self.log_text.see(tk.END)  # 자동으로 스크롤을 맨 아래로 이동

            # 인덱스 증가
            idx += 1
            

        tk.messagebox.showinfo("Encoding Complete", "QR code encoding completed successfully.")
        
    def start(self):
        self.window.mainloop()

if __name__ == "__main__":
    app = QRCodeGenerator()
    app.start()
이 프로그램은 로그창에 실시간으로 보여주는게 아니라 QR코드 생성이 다 끝나면 보여주게 되어 있어. QR코드 생성할때 마다 로그창에 실시간으로 보여주길 원해. 그리고 프로그램바도 추가해줘.

## ChatGPT 응답:
> 로그창에 실시간으로 로그를 보여주기 위해서는 `self.log_text.see(tk.END)`를 호출하여 스크롤을 맨 아래로 이동시켜야 합니다. 이전에 제공한 코드에서 이 부분은 이미 구현되어 있습니다.

프로그레스바를 추가하기 위해서는 `tkinter.ttk.Progressbar` 위젯을 사용하면 됩니다. `maximum` 속성을 설정하여 프로그레스바의 전체 범위를 설정하고, `value` 속성을 조절하여 현재 진행 상황에 맞게 프로그레스바를 업데이트하면 됩니다.

아래는 수정된 코드입니다.

```python
import tkinter as tk
from tkinter import filedialog, scrolledtext, ttk
import qrcode
import os

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = scrolledtext.ScrolledText(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 프로그레스 바
        self.progress_bar = ttk.Progressbar(self.window, orient=tk.HORIZONTAL, mode='determinate')
        self.progress_bar.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("All files", "*.*")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_with_ext = os.path.basename(self.file_path)
        dir_name = os.path.join(self.qr_dir, filename_with_ext)
        
        # QR 코드 이미지와 텍스트 파일이 이미 존재한다면 삭제
        if os.path.exists(dir_name):
            for file_name in os.listdir(dir_name):
                os.remove(os.path.join(dir_name, file_name))
        else:
            os.makedirs(dir_name)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장

## 사용자 입력:
> 완성해줘. 자꾸 끊긴다.

## ChatGPT 응답:
> 코드를 완성해 드리겠습니다. 중간에 끊기는 것을 방지하기 위해 코드를 여러 개의 메서드로 나누어 작성하였습니다. 추가로 프로그레스바도 추가하였습니다.

```python
import tkinter as tk
from tkinter import filedialog, scrolledtext
import qrcode
import os


class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")

        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)

        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)

        # 로그 창
        self.log_text = scrolledtext.ScrolledText(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)

        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)

        # 프로그레스바
        self.progress_bar = tk.ttk.Progressbar(self.window, orient="horizontal", length=300, mode="determinate")
        self.progress_bar.pack(padx=10, pady=10)

        self.file_path = None
        self.qr_dir = None

    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("All files", "*.*")])

    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")

    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return

        self.log_text.delete("1.0", tk.END)  # 이전 로그 삭제
        self.progress_bar["value"] = 0  # 프로그레스바 초기화

        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_with_ext = os.path.basename(self.file_path)
        dir_name = os.path.join(self.qr_dir, filename_with_ext)

        if os.path.exists(dir_name):
            self.delete_existing_qr_codes(dir_name)

        os.makedirs(dir_name)

        # QR 코드 생성 및 이미지 저장
        split_size = 2200  # 한 장당 저장될 문자열 길이
        qr_text_list = []  # 추출된 QR 코드 텍스트 저장 리스트
        idx = 0  # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log

## 사용자 입력:
> 끊긴 부분부터 이어서 완성해줘.

## ChatGPT 응답:
> ```
import tkinter as tk
from tkinter import filedialog, messagebox, ttk, scrolledtext
import qrcode
import os

class QRCodeGenerator:
    def __init__(self):
        self.window = tk.Tk()
        self.window.title("QR Code Generator")
        
        # 파일 선택 버튼
        self.select_file_button = tk.Button(
            self.window, text="Select File", command=self.select_file)
        self.select_file_button.pack(padx=10, pady=10)
        
        # 저장 위치 선택 버튼
        self.select_dir_button = tk.Button(
            self.window, text="Select Directory", command=self.select_directory)
        self.select_dir_button.pack(padx=10, pady=10)
        
        # 로그 창
        self.log_text = scrolledtext.ScrolledText(self.window, width=60, height=10)
        self.log_text.pack(padx=10, pady=10)
        
        # 생성 버튼
        self.generate_button = tk.Button(
            self.window, text="Generate QR Codes", command=self.generate_qr_codes)
        self.generate_button.pack(padx=10, pady=10)
        
        # 프로그레스 바
        self.progress_bar = ttk.Progressbar(self.window, orient="horizontal", length=300, mode="determinate")
        self.progress_bar.pack(padx=10, pady=10)
        
        self.file_path = None
        self.qr_dir = None
        
    def select_file(self):
        self.file_path = filedialog.askopenfilename(
            title="Select File", filetypes=[("All files", "*.*")])
    
    def select_directory(self):
        self.qr_dir = filedialog.askdirectory(
            title="Select Directory to Save QR Codes")
        
    def generate_qr_codes(self):
        if not self.file_path:
            tk.messagebox.showerror("Error", "Please select a file.")
            return
        if not self.qr_dir:
            tk.messagebox.showerror("Error", "Please select a directory to save QR codes.")
            return
        
       
        # 텍스트 파일 열기
        with open(self.file_path, 'r', encoding='utf-8') as f:
            text = f.read()

        # 디렉토리 생성
        filename_with_ext = os.path.basename(self.file_path)
        dir_name = os.path.join(self.qr_dir, filename_with_ext)
        
        # QR 코드 이미지와 텍스트 파일이 이미 존재한다면 삭제
        if os.path.exists(dir_name):
            for file_name in os.listdir(dir_name):
                os.remove(os.path.join(dir_name, file_name))
        else:
            os.makedirs(dir_name)
        
        # QR 코드 생성 및 이미지 저장
        split_size = 2200 # 한 장당 저장될 문자열 길이
        idx = 0 # 이미지 파일 이름에 사용될 인덱스
        total_chunks = len(text) // split_size + 1
        self.progress_bar["maximum"] = total_chunks
        
        for i in range(total_chunks):
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[i*split_size:(i+1)*split_size]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue
            
            file_path = os.path.join(dir_name, f"{idx:03d}_qr_code.png")
            img.save(file_path)
            
            self.log_text.insert(tk

## 사용자 입력:
> # QR 코드 이미지 생성 부터 이어서 완성해줘.

## ChatGPT 응답:
>         # QR 코드 생성 및 이미지 저장
        split_size = 2200  # 한 장당 저장될 문자열 길이
        idx = 0  # 이미지 파일 이름에 사용될 인덱스
        while text:
            # 텍스트에서 한 번에 저장될 문자열 분리
            data = text[:split_size]
            text = text[split_size:]

            # QR 코드 이미지 생성
            try:
                img = qrcode.make(data)
            except Exception as e:
                self.log_text.insert(tk.END, f"Error occurred while creating QR code: {e}\n")
                continue

            # 이미지 파일 저장
            file_name = f"{idx:03d}_qr_code.png"
            file_path = os.path.join(self.qr_dir, file_name)
            img.save(file_path)

            self.log_text.insert(tk.END, f"QR code saved to {file_path}\n")
            self.log_text.see(tk.END)  # 자동으로 스크롤을 맨 아래로 이동

            # 인덱스 증가
            idx += 1

        tk.messagebox.showinfo("Encoding Complete", "QR code encoding completed successfully.")

