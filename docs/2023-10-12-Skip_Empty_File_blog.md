---
layout: post
title: "Skip Empty File"
date: 2023-10-12
tags: [Empty, File, Skip]
categories: blog
---

## 사용자 입력:
>     with Path(file_path).open('rb') as f:  파일길이가 0이면 skip하는 거 추가

## ChatGPT 응답:
> If you want to skip processing a file if its size (or length) is zero using Python's `pathlib`, you can do something like this:

```python
from pathlib import Path

file_path = 'your/file/path/here'

# Check if the file size is non-zero before opening
if Path(file_path).stat().st_size > 0:
    with Path(file_path).open('rb') as f:
        # Your code to process the file here
        pass
else:
    print(f"File {file_path} is empty. Skipping...")
```

Here, `Path(file_path).stat().st_size` gets the file size in bytes. The code checks if this size is greater than 0 before proceeding to open and process the file. If the file size is zero, it simply prints a skip message.

## 사용자 입력:
> f로 처리하는 걸로 알려줘.

## ChatGPT 응답:
> Got it. If you want to use the file object `f` to determine if the file is empty and skip if it is, you can do the following:

```python
from pathlib import Path

file_path = 'your/file/path/here'

with Path(file_path).open('rb') as f:
    # Check if the file has content
    if f.read(1):  # Try to read a byte
        # If we're here, it means the file has content.
        # Reset file pointer to the start if you read from it.
        f.seek(0)
        # Your code to process the file here
        pass
    else:
        print(f"File {file_path} is empty. Skipping...")
```

Here, `f.read(1)` attempts to read a single byte from the file. If the file is empty, it will return an empty bytes object which will evaluate to `False` in a boolean context, leading to the print statement. If there's content, it resets the file pointer back to the beginning with `f.seek(0)` and proceeds with processing.

