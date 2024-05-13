---
layout: post
title: "Skip Dot Files功能"
date: 2023-10-12
tags: [Dot, Skip]
categories: blog
---

## 사용자 입력:
> def get_extension_list(dir:str, extension:str = None) -> list:
    copy_dir = dir
    copy_extension = extension
    return_list = list()

    for (root, dirs, files) in os.walk(copy_dir):
        # Skip .git directory
        if '.git' in dirs:
            dirs.remove('.git')  # Remove .git from the list of directories to prevent os.walk() from visiting it
        # Skip .git directory
        if 'TeraStream' in dirs:
            dirs.remove('TeraStream')  # Remove .git from the list of directories to prevent os.walk() from visiting it            
        
        if copy_extension:  # 확장자가 주어진 경우
            row = [os.path.join(root, file) for file in files if file.endswith(copy_extension)]
        else:  # 확장자가 주어지지 않았거나 빈 문자열인 경우 (확장자가 없는 파일)
            row = [os.path.join(root, file) for file in files if not os.path.splitext(file)[1]]

        if row:
            return_list.extend(row)

    return return_list 이 함수에서 .gitignore , .project 같이 .으로 시작하는 파일은 skip하도록 기능 추가.

## ChatGPT 응답:
> To skip files that start with a period (like `.gitignore` or `.project`), you can add an additional condition in the list comprehension. 

Here's the modified `get_extension_list` function with this feature:

```python
import os

def get_extension_list(dir:str, extension:str = None) -> list:
    copy_dir = dir
    copy_extension = extension
    return_list = list()

    for (root, dirs, files) in os.walk(copy_dir):
        # Skip .git and TeraStream directories
        if '.git' in dirs:
            dirs.remove('.git')
        if 'TeraStream' in dirs:
            dirs.remove('TeraStream') 
        
        if copy_extension:  # 확장자가 주어진 경우
            row = [os.path.join(root, file) for file in files if file.endswith(copy_extension) and not file.startswith('.')]
        else:  # 확장자가 주어지지 않았거나 빈 문자열인 경우 (확장자가 없는 파일)
            row = [os.path.join(root, file) for file in files if not os.path.splitext(file)[1] and not file.startswith('.')]

        if row:
            return_list.extend(row)

    return return_list
```

I've added the condition `and not file.startswith('.')` in the list comprehensions to ensure files starting with a dot are excluded.

## 사용자 입력:
> 위에서 바이너리 파일은 제외하도록 해줘.

## ChatGPT 응답:
> To skip binary files, we need to read the content of each file and check if it contains any null bytes (`\x00`), which is a common indication of a binary file. 

Let's modify the `get_extension_list` function to skip binary files:

```python
import os

def is_binary(file_path: str) -> bool:
    """
    Check if a file is binary by looking for null bytes.
    """
    with open(file_path, 'rb') as f:
        CHUNKSIZE = 8192
        while True:
            chunk = f.read(CHUNKSIZE)
            if b'\x00' in chunk:  # Check for null bytes
                return True
            if len(chunk) < CHUNKSIZE:
                break
    return False

def get_extension_list(dir:str, extension:str = None) -> list:
    copy_dir = dir
    copy_extension = extension
    return_list = list()

    for (root, dirs, files) in os.walk(copy_dir):
        # Skip .git and TeraStream directories
        if '.git' in dirs:
            dirs.remove('.git')
        if 'TeraStream' in dirs:
            dirs.remove('TeraStream') 
        
        for file in files:
            file_path = os.path.join(root, file)

            # Skip binary files
            if is_binary(file_path):
                continue

            # Check for desired extensions or lack thereof
            if copy_extension:
                if file.endswith(copy_extension) and not file.startswith('.'):
                    return_list.append(file_path)
            else:
                if not os.path.splitext(file)[1] and not file.startswith('.'):
                    return_list.append(file_path)

    return return_list
```

I added a helper function `is_binary` to check if a file is binary. Then, in the main function, I loop through files, check if they're binary using our helper function, and skip them accordingly.

