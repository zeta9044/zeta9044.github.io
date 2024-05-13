---
layout: post
title: "Bamboo - Create Deployment Plan Variables"
date: 2023-03-22
tags: [Plan, Deployment, Create, Variables, Bamboo]
categories: blog
---

## 사용자 입력:
> bamboo에서 deployment plan을 생성할 때, variable만드법 알려줘

## ChatGPT 응답:
> Bamboo에서 Deployment Plan을 생성할 때, 변수(Variables)를 만들려면 다음 단계를 따르면 됩니다:

1. Deployment Plan을 생성하거나 편집하는 페이지에서 왼쪽 메뉴에서 "Variables"를 선택합니다.
2. "Add variable" 버튼을 클릭합니다.
3. "Name" 필드에 변수 이름을 입력합니다.
4. "Value" 필드에 변수의 값을 입력합니다.
5. "Encrypted?" 옵션을 선택하면 값이 암호화됩니다.
6. "Environment-specific value" 옵션을 선택하면 변수의 값을 각각 다른 환경에 맞게 지정할 수 있습니다.
7. 필요한 경우 "Description" 필드에 변수에 대한 설명을 추가합니다.
8. "Add" 버튼을 클릭하여 변수를 저장합니다.

이제 변수가 Deployment Plan에서 사용할 수 있습니다. 변수를 사용하려면 Deployment Plan에서 변수를 참조하는 곳에 ${variable_name}과 같은 구문을 사용하면 됩니다. 변수를 참조하는 위치는 Bamboo의 다양한 설정, 스크립트 또는 템플릿 파일에서 사용될 수 있습니다.

## 사용자 입력:
> 예제로 알려줘

## ChatGPT 응답:
> 예를 들어, Deployment Plan에서 AWS Lambda 함수를 배포하는 경우, AWS Lambda 함수 이름이 다른 환경에서 다를 수 있습니다. 이 경우, 변수를 사용하여 각각의 환경에서 AWS Lambda 함수 이름을 지정할 수 있습니다.

1. Deployment Plan을 생성하거나 편집하는 페이지에서 왼쪽 메뉴에서 "Variables"를 선택합니다.
2. "Add variable" 버튼을 클릭합니다.
3. "Name" 필드에 "lambda_function_name"을 입력합니다.
4. "Value" 필드에 각각의 환경에 맞는 AWS Lambda 함수 이름을 입력합니다. 예를 들어, 테스트 환경에서는 "my-function-test", 스테이징 환경에서는 "my-function-staging", 프로덕션 환경에서는 "my-function-prod"와 같이 입력합니다.
5. "Environment-specific value" 옵션을 선택하고, "Environment name" 필드에 각각의 환경 이름을 입력합니다. 그리고 "Value" 필드에 각각의 환경에 맞는 AWS Lambda 함수 이름을 입력합니다.
6. 필요한 경우 "Description" 필드에 변수에 대한 설명을 추가합니다.
7. "Add" 버튼을 클릭하여 변수를 저장합니다.

이제 변수를 사용하여 AWS Lambda 함수 이름을 참조할 수 있습니다. 예를 들어, AWS Lambda 함수 배포 스크립트에서 다음과 같이 변수를 사용할 수 있습니다:

```
aws lambda update-function-code --function-name ${lambda_function_name} --zip-file fileb://function.zip
```

위와 같이 스크립트에서 변수를 사용하면 각각의 환경에서 올바른 AWS Lambda 함수 이름을 참조할 수 있습니다.

