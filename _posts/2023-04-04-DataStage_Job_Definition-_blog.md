---
layout: post
title: "DataStage Job Definition."
date: 2023-04-04
tags: [DataStage, Job, Definition]
categories: blog
---

## 사용자 입력:
> <com.ibm.datastage.ai.dtm.ds:DSJobDefSDO xmiversion="2.0"
    xmlns:xmi="http://www.omg.org/XMI" xmlhsixsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:com.ibm.datastage.ai.dtm.ds="http:///com/ibm/datastage/ai/dtm/ds.ecore" xmi:id="86" sDoVersion="2.0.0" objectID="c2e76084.43058877.qqpo9uat8.6lemca9.sr3gev.hub 38uffjqtdhjhipmha7" lastModificationTimestamp="2023-02-27T22:05:12.132+0900" optimisticLockID="10" createdByUser="migdev" creationTimestamp="2022-12-10T09:09:09,291 +0900" modifiedByuser="migdev" name="DPN_L0NUCA_TB_CMCA_CUST_JINFO INQ.L" isSystem="false" jobType="Parallel" version="56.0.0" act2ActBufferSize="0" act2ActEnableRowBuffer="false" actzActIPCTimeout="0" act2ActOverideDefaults="false" act2ActUselpC="false" allowMultiplelnvocations="false" centuryBreakYear="30" container_="VO" controlAfterSubr="false" copyLibPrefix="ARDT" enableCacheSharing="false" expressionSemanticCheckFlag="O" iMSProgType="0" isTemplate="false" jobReportFlag="false" mFProcessMetaData="0" mFProcessetaDataxMLFileExchangeMethod ="0" nextAliasiD="2" nextID="10" nLSLocale=",,,," nULLIndicatorPosition="0" parameterFileDDName="DD00001" pgmCustomizationFlag="false" relstagesInJobstatus="-1" reservedWordcheck="1" runtimeColumnPropagation="false" traceoption="0" transactionsize="0" uploadable="false" validationstatus="0" webServiceEnabled="false" category="wwJobs₩₩20. NBISNUCA" dSNameSpace="PNBISETLO1V:MIG" recordPerformanceResults="0" internallD="ROOT" dSlobType="3"> 이것은 IBM DataStage .pjb파일의 일부야.해석해줘.

## ChatGPT 응답:
> 이것은 IBM DataStage의 작업 정의(job definition)을 나타내는 XML 파일이며, 데이터 통합 작업을 구성하는 구성 요소를 정의합니다. 

파일의 첫 번째 줄은 DSJobDefSDO 요소를 정의하고 있습니다. 이 요소는 XML 파일이 IBM DataStage 작업 정의를 나타내는 것임을 나타내는 이름공간을 가지고 있습니다. 

이 파일은 job의 메타데이터를 포함하고 있습니다. 이 metadata는 job의 이름, 버전, 생성 및 수정 일자 및 시간, 작성자, job의 종류, 등 다양한 정보를 포함하고 있습니다. 

각 속성은 job의 특성을 설명하며, 예를 들어 "jobType" 속성은 job의 유형(Parallel 또는 Server)을 나타냅니다. "version" 속성은 job의 버전을 나타내며, "createdByUser" 및 "modifiedByuser" 속성은 각각 job을 생성하고 수정한 사용자를 나타냅니다.

또한 이 파일은 job의 성능, 오류 및 경고를 보고하기 위한 옵션도 포함하고 있습니다. 예를 들어 "jobReportFlag" 속성은 job 보고서를 생성하도록 지시하며, "traceoption" 속성은 job 실행 중 발생하는 문제를 진단하기 위해 추적(trace)을 활성화하는 데 사용됩니다.

마지막으로, 이 파일은 DataStage job의 로컬 및 공유 자원의 위치를 식별하는 데 사용되는 다양한 매개 변수도 포함합니다. 이러한 매개 변수는 데이터베이스 연결 정보, 파일 경로 및 스키마 이름 등을 포함할 수 있습니다.

## 사용자 입력:
> 이클립스 RCP 프로그램이 UTF-8을 기본으로 실행하게 하려면?

## ChatGPT 응답:
> 이클립스 RCP 프로그램이 UTF-8을 기본으로 실행하게 하려면, 다음과 같은 단계를 따르면 됩니다.

1. 이클립스 RCP 프로그램을 실행합니다.
2. Window → Preferences 메뉴를 선택합니다.
3. General → Workspace 메뉴로 이동합니다.
4. Text file encoding 섹션에서 Other: UTF-8을 선택합니다.
5. Apply 버튼을 클릭하고, OK 버튼을 클릭하여 변경 사항을 저장합니다.

위의 단계를 따르면, 이클립스 RCP 프로그램에서 새 파일을 만들거나 기존 파일을 열 때 UTF-8 인코딩이 사용됩니다. 또한, 이전에 생성된 파일의 인코딩도 UTF-8로 변경됩니다.

## 사용자 입력:
> 이클립스 INI파일로는 안돼나?

## ChatGPT 응답:
> 네, 이클립스 INI 파일을 사용하여 이클립스 RCP 프로그램이 UTF-8을 기본으로 실행되도록 설정할 수 있습니다.

INI 파일에서 UTF-8을 기본 인코딩으로 설정하려면, 다음과 같은 설정을 추가해야 합니다.

```
-vmargs
-Dfile.encoding=UTF-8
```

위 설정은 인코딩을 UTF-8로 설정하는 "-Dfile.encoding=UTF-8" JVM 인자를 추가합니다.

INI 파일을 수정하는 방법은 다음과 같습니다.

1. 이클립스 RCP 프로그램 디렉토리에서 "eclipse.ini" 파일을 찾습니다.
2. 파일을 열고, "-vmargs" 옵션 아래에 위의 설정을 추가합니다.
3. 파일을 저장하고, 이클립스 RCP 프로그램을 재시작합니다.

위의 설정은 이클립스 RCP 프로그램에서 새 파일을 만들거나 기존 파일을 열 때 UTF-8 인코딩이 사용되도록 합니다. 이전에 생성된 파일의 인코딩은 변경되지 않습니다.

