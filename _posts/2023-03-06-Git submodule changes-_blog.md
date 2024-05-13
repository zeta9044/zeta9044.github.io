---
layout: post
title: "Git submodule changes."
date: 2023-03-06
tags: [Git, changes, submodule]
categories: blog
---

## 사용자 입력:
> Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
  (commit or discard the untracked or modified content in submodules)
        modified:   jgraphx (untracked content)

## ChatGPT 응답:
> This message indicates that there are changes made to the files in the "jgraphx" submodule that have not been staged for commit. Git is suggesting that you use the "git add" command to update the changes and stage them for the next commit.

To stage the changes, you can use the following command:

```
git add jgraphx
```

This will stage all changes made to files in the "jgraphx" submodule. Once the changes have been staged, you can commit them using the "git commit" command.

Alternatively, if you do not want to keep the changes made to the files in the "jgraphx" submodule, you can discard them using the "git restore" command. For example:

```
git restore jgraphx
```

This will discard any changes made to files in the "jgraphx" submodule and revert them back to their previous state.

