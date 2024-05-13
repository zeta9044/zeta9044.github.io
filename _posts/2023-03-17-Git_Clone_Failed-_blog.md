---
layout: post
title: "Git Clone Failed."
tags: [Clone, Failed, Git]
---

## 사용자 입력:
> 7-3월-2023 18:01:11	Warning: Permanently added '[192.168.0.119]:7999' (RSA) to the list of known hosts.
17-3월-2023 18:01:11	Repository not found
17-3월-2023 18:01:11	The requested repository does not exist, or you do not have permission to access it.
17-3월-2023 18:01:11	fatal: 리모트 저장소에서 읽을 수 없습니다
17-3월-2023 18:01:11	올바른 접근 권한이 있는지, 그리고 저장소가 있는지
17-3월-2023 18:01:11	확인하십시오.
17-3월-2023 18:01:11	fatal: 'ssh://git@192.168.0.119:7999/qtss/jgraphx.git'에서 하위 모듈 경로 '/data1/bamboo-home/xml-data/build-dir/QTRAC-DEV-CHKO/jgraphx'에 복제하는데 실패했습니다
17-3월-2023 18:01:11	'jgraphx' 복제하는데 실패했습니다. 재시도 예정
17-3월-2023 18:01:11	'/data1/bamboo-home/xml-data/build-dir/QTRAC-DEV-CHKO/jgraphx'에 복제합니다...
17-3월-2023 18:01:11	Warning: Permanently added '[192.168.0.119]:7999' (RSA) to the list of known hosts.
17-3월-2023 18:01:11	Repository not found
17-3월-2023 18:01:11	The requested repository does not exist, or you do not have permission to access it.
17-3월-2023 18:01:11	fatal: 리모트 저장소에서 읽을 수 없습니다
17-3월-2023 18:01:11	올바른 접근 권한이 있는지, 그리고 저장소가 있는지
17-3월-2023 18:01:11	확인하십시오.
17-3월-2023 18:01:11	fatal: 'ssh://git@192.168.0.119:7999/qtss/jgraphx.git'에서 하위 모듈 경로 '/data1/bamboo-home/xml-data/build-dir/QTRAC-DEV-CHKO/jgraphx'에 복제하는데 실패했습니다
17-3월-2023 18:01:11	두번째 'jgraphx' 복제하는데 실패했습니다. 중지합니다
17-3월-2023 18:01:11	        at com.atlassian.bamboo.plugins.git.GitCommandProcessor.runCommand(GitCommandProcessor.java:704)
17-3월-2023 18:01:11	        at com.atlassian.bamboo.plugins.git.GitCommandProcessor.runCommand(GitCommandProcessor.java:633)
17-3월-2023 18:01:11	        at com.atlassian.bamboo.plugins.git.GitCommandProcessor.runCommand(GitCommandProcessor.java:624)
17-3월-2023 18:01:11	        at com.atlassian.bamboo.plugins.git.GitCommandProcessor.runSubmoduleUpdateCommand(GitCommandProcessor.java:392)
17-3월-2023 18:01:11	        at com.atlassian.bamboo.plugins.git.NativeGitOperationHelper.checkout(NativeGitOperationHelper.java:479)
17-3월-2023 18:01:11	        ... 30 more
17-3월-2023 18:01:11	Failed to prepare the build 'Q-Track WAS - develop - 소스 코드 체크아웃 #2 (QTRAC-DEV-CHKO-2)'

## ChatGPT 응답:
> It appears that there was an error with a git repository while attempting to perform a code checkout. The error message states that the requested repository was not found or the user did not have permission to access it. The error occurred when attempting to clone the 'jgraphx' submodule from the repository located at 'ssh://git@192.168.0.119:7999/qtss/jgraphx.git'. The error message also indicates that the checkout process was retried but failed a second time. The error occurred during the preparation of the build 'Q-Track WAS - develop - 소스 코드 체크아웃 #2 (QTRAC-DEV-CHKO-2)'.

## 사용자 입력:
> 한글로

## ChatGPT 응답:
> 코드 체크아웃을 수행하는 중 git 저장소에서 오류가 발생한 것으로 보입니다. 오류 메시지에는 요청한 저장소를 찾을 수 없거나 사용자가 액세스 권한이 없는 것으로 나와 있습니다. 'jgraphx' 하위 모듈을 'ssh://git@192.168.0.119:7999/qtss/jgraphx.git'에 위치한 저장소에서 복제하려고 할 때 오류가 발생했습니다. 오류 메시지에는 체크아웃 프로세스가 재시도되었지만 두 번째 실패했다는 것도 나와 있습니다. 이 오류는 'Q-Track WAS - develop - 소스 코드 체크아웃 #2 (QTRAC-DEV-CHKO-2)' 빌드 준비 과정에서 발생한 것입니다.

