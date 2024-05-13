---
layout: post
title: "Skip .git in os.walk"
date: 2023-10-12
tags: [walk, in, Skip, git, os]
categories: blog
---

## 사용자 입력:
> def get_extension_list(dir:str, extension:str = None) -> list:
    copy_dir = dir
    copy_extension = extension
    return_list = list()
    for (root, dirs, files) in os.walk(copy_dir):
        if copy_extension:  # 확장자가 주어진 경우
            row = [os.path.join(root, file) for file in files if file.endswith(copy_extension)]
        else:  # 확장자가 주어지지 않았거나 빈 문자열인 경우 (확장자가 없는 파일)
            row = [os.path.join(root, file) for file in files if not os.path.splitext(file)[1]]
        
        if row:
            return_list.extend(row)
    return return_list  이 함수에서 .git디렉토리는 skip하도록 처리

## ChatGPT 응답:
> To skip the `.git` directory when walking through the directories, you can check if `.git` is part of the current root during each iteration of the `os.walk()` and continue to the next iteration if it is. Here's how you can modify your function to skip `.git`:

```python
import os

def get_extension_list(dir:str, extension:str = None) -> list:
    copy_dir = dir
    copy_extension = extension
    return_list = list()

    for (root, dirs, files) in os.walk(copy_dir):
        # Skip .git directory
        if '.git' in dirs:
            dirs.remove('.git')  # Remove .git from the list of directories to prevent os.walk() from visiting it
        
        if copy_extension:  # 확장자가 주어진 경우
            row = [os.path.join(root, file) for file in files if file.endswith(copy_extension)]
        else:  # 확장자가 주어지지 않았거나 빈 문자열인 경우 (확장자가 없는 파일)
            row = [os.path.join(root, file) for file in files if not os.path.splitext(file)[1]]

        if row:
            return_list.extend(row)

    return return_list
```

Note: By modifying `dirs` directly during the loop, you're influencing the directories that `os.walk()` will visit next. By removing `.git` from `dirs`, you're effectively skipping it and all of its subdirectories.

