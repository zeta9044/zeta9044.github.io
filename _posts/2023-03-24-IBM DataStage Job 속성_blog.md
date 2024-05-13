---
layout: post
title: "IBM DataStage Job 속성"
date: 2023-03-24
tags: [DataStage, IBM, Job, 속성]
categories: blog
---

## 사용자 입력:
> 다음 코드는 IBM DataStage .pjb파일의 일부이다. 이 내용을 자세히 해석해줘. 
<com.ibm.datastage.ai.dtm.ds:DSJobDefSDO xmi:version="2.0" xmlns:xmi="http://www.omg.org/XMI" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:com.ibm.datastage.ai.dtm.ds="http:///com/ibm/datastage/ai/dtm/ds.ecore" xmi:id="86" sDOVersion="2.0.0" objectID="c2e76d84.43058877.qqpo9uat8.6lemca9.sr3gev.hub38uffjqtdhjhipmha7" lastModificationTimestamp="2023-02-27T22:05:12.132+0900" optimisticLockID="10" createdByUser="migdev" creationTimestamp="2022-12-10T09:09:09.291+0900" modifiedByUser="migdev" name="DPNI_L0NUCA_TB_CMCA_CUST_INFO_INQ_L" isSystem="false" jobType="Parallel" version="56.0.0" act2ActBufferSize="0" act2ActEnableRowBuffer="false" act2ActIPCTimeout="0" act2ActOverideDefaults="false" act2ActUseIPC="false" allowMultipleInvocations="false" centuryBreakYear="30" container_="V0" controlAfterSubr="false" copyLibPrefix="ARDT" enableCacheSharing="false" expressionSemanticCheckFlag="0" iMSProgType="0" isTemplate="false" jobReportFlag="false" mFProcessMetaData="0" mFProcessMetaDataXMLFileExchangeMethod="0" nextAliasID="2" nextID="10" nLSLocale=",,,," nULLIndicatorPosition="0" parameterFileDDName="DD00001" pgmCustomizationFlag="false" relStagesInJobStatus="-1" reservedWordCheck="1" runtimeColumnPropagation="false" traceOption="0" transactionSize="0" uploadable="false" validationStatus="0" webServiceEnabled="false" category="\\Jobs\\20. NBIS\\NUCA" dSNameSpace="PNBISETL01V:MIG" recordPerformanceResults="0" internalID="ROOT" dSJobType="3">


## ChatGPT 응답:
> 이 코드는 IBM DataStage의 .pjb 파일에서 DSJobDefSDO를 정의하는 XML 문서의 일부입니다. 이 섹션에서는 Job의 다양한 속성을 정의합니다.

각 속성은 이름과 값 쌍으로 구성되어 있으며, 속성 값은 쌍따옴표(" ")로 묶여 있습니다. 몇 가지 중요한 속성은 다음과 같습니다.

- xmi:version: XMI 버전 정보
- xmlns:xmi, xmlns:xsi, xmlns:com.ibm.datastage.ai.dtm.ds: 네임스페이스 정보
- xmi:id: DSJobDefSDO의 ID
- name: Job의 이름
- isSystem: 시스템 Job 여부
- jobType: Job의 유형 (Parallel 또는 Server)
- version: Job의 버전 정보
- createdByUser, creationTimestamp, modifiedByUser, lastModificationTimestamp: Job의 생성 및 수정 정보
- container_: Job이 속한 Container의 이름
- allowMultipleInvocations: Job이 다중 실행을 허용하는지 여부
- dSNameSpace: Job이 실행될 DataStage의 네임스페이스 정보
- validationStatus: Job의 유효성 검사 결과
- category: Job이 속한 카테고리 정보
- recordPerformanceResults: Job의 성능 결과 기록 여부

이외에도 다른 속성들이 정의되어 있습니다.

