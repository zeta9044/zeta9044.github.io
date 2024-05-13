---
layout: post
title: "Oracle KST Timezone Change"
date: 2023-04-12
tags: [Timezone, Change, KST, Oracle]
categories: blog
---

## 사용자 입력:
> oracle에서 TZABBREV = KST 인 시간대로 변경하려면?

## ChatGPT 응답:
> Oracle에서 시간대를 변경하려면 `ALTER SESSION` 명령을 사용하여 세션의 `TIME_ZONE` 설정을 조정할 수 있습니다. KST(Korea Standard Time)로 시간대를 변경하려면 다음 명령을 사용하세요:

```sql
ALTER SESSION SET TIME_ZONE = 'Asia/Seoul';
```

이렇게하면 Oracle 세션의 시간대가 KST로 설정됩니다. 그러나 이 설정은 현재 Oracle 세션에만 적용됩니다. 다른 세션에도 동일한 설정을 적용하려면 각 세션에 이 명령을 실행해야 합니다.

또한 Oracle 데이터베이스 서버 전체의 기본 시간대를 변경하려면 서버의 운영 체제 수준에서 시간대를 변경해야 합니다. 이 작업은 데이터베이스 관리자 또는 시스템 관리자가 수행해야 할 수 있습니다.

## 사용자 입력:
> ALTER DATABASE SET TIME_ZONE='Asia/Seoul';
이렇게 하면 안되나?

## ChatGPT 응답:
> 죄송합니다. 제가 이전의 답변에서 `ALTER DATABASE` 명령을 놓쳤습니다. Oracle 데이터베이스의 시간대를 변경하려면, `ALTER DATABASE` 명령을 사용할 수 있습니다. 이 명령은 데이터베이스 전체의 기본 시간대를 변경합니다.

```sql
ALTER DATABASE SET TIME_ZONE = 'Asia/Seoul';
```

이렇게 설정하면, 데이터베이스 전체의 기본 시간대가 KST로 변경됩니다. 그러나 이 명령을 실행하기 전에 몇 가지 주의 사항이 있습니다:

1. 이 작업은 DBA 권한을 가진 사용자만 수행할 수 있습니다.
2. 이 명령을 실행하기 전에 모든 데이터베이스 연결을 종료해야 합니다.
3. 시간대를 변경하면, 시간대 종속 데이터가 영향을 받을 수 있으므로, 변경 전에 데이터베이스를 백업하고 테스트하는 것이 좋습니다.

시간대 변경이 완료된 후에는, 새 연결 및 세션에서 변경된 시간대가 적용됩니다. 이전에 설정한 세션 수준의 시간대 설정은 변경되지 않습니다. 세션 수준의 설정을 변경하려면, 앞서 언급한 `ALTER SESSION` 명령을 사용해야 합니다.

## 사용자 입력:
> IBM DataStage 11이상의 .pjb파일에서 stageType="CTransformerStage"  이고 name="TRNAS" 인 contains_JobObject태그에서 inpin과 outpin의 관계는 어떻게 알 수 있나?

## ChatGPT 응답:
> IBM DataStage의 .pjb 파일은 XML 형식으로 되어 있으며, 각 스테이지 및 관련 요소를 정의합니다. stageType="CTransformerStage"인 스테이지에서 name="TRANS"로 정의된 contains_JobObject 태그에서 inpin과 outpin 사이의 관계를 확인하려면, XML 파일을 열어 적절한 스테이지를 찾아야 합니다.

1. 먼저, 스테이지를 찾기 위해 다음과 같은 XML 요소를 찾으세요:
```xml
<contains_JobObject stageType="CTransformerStage" name="TRANS">
```

2. 그런 다음, inpin 및 outpin의 관계를 확인하기 위해 해당 스테이지에서 `<links>` 태그를 찾으세요. `<links>` 태그는 스테이지의 입력 및 출력 연결을 정의합니다.

예를 들어, 이러한 링크 구성이 있다고 가정해 봅시다:

```xml
<links>
  <link identifier="1" type="input" name="in0" />
  <link identifier="2" type="output" name="out0" />
</links>
```

이 경우, `identifier` 속성은 각 링크를 식별하는 데 사용되며, `type` 속성은 링크가 입력인지 출력인지를 나타냅니다. `name` 속성은 링크의 이름을 나타냅니다.

3. 이제 이 정보를 바탕으로 스테이지의 매핑 및 변환 규칙을 찾을 수 있습니다. `CTransformerStage`에서는 `<mapping>` 태그에서 이러한 규칙을 찾을 수 있습니다.

예를 들어, 다음과 같은 매핑이 있다고 가정해 봅시다:

```xml
<mapping identifier="1">
  <inputLinks>in0</inputLinks>
  <outputLinks>out0</outputLinks>
  <expression>some_expression</expression>
</mapping>
```

이 경우, 매핑은 입력 링크 `in0`와 출력 링크 `out0`를 연결하고 있으며, `some_expression`을 사용하여 데이터를 변환합니다.

이렇게 .pjb 파일에서 inpin과 outpin의 관계를 확인할 수 있습니다. 필요한 경우 이 정보를 사용하여 스테이지의 변환 규칙을 업데이트하거나 다른 스테이지로 데이터를 전달할 수 있습니다.

## 사용자 입력:
> IBM DataStage 11이상의 .pjb파일에서     <contains_JobObject xsi:type="com.ibm.datastage.ai.dtm.ds:DSStageSDO" xmi:id="114" sDOVersion="2.0.0" objectID="c2e76d84.9f91cbaf.qqpo9uat8.6lenia9.ka1fub.p6g2pulbki8e2br502pu0" lastModificationTimestamp="2022-12-10T09:09:09.291+0900" optimisticLockID="1" createdByUser="migdev" creationTimestamp="2022-12-10T09:09:09.291+0900" modifiedByUser="migdev" name="TRANS" isSystem="false" internalID="V0S2" nextID="3" stageType="CTransformerStage" stageTypeClassName="TransformerStage" inputPins="V0S2P1" outputPins="V0S2P2" stageTypeIsActive="true" xmetaLockingRoot="c2e76d84.43058877.qqpo9uat8.6lemca9.sr3gev.hub38uffjqtdhjhipmha7"> 이런 태그가 있는데, 여기서 inputpin과 outpin과의 관계는 어떻게 아나?


## ChatGPT 응답:
> IBM DataStage에서 .pjb 파일은 프로젝트 내의 작업(job)을 정의한 XML 파일입니다. 여기서 inputPin과 outputPin은 각각 DataStage 작업의 스테이지에 대한 입력 및 출력 연결을 나타냅니다.

이 경우의 XML 코드는 DataStage 작업의 `TRANS`라는 이름의 Transformer 스테이지를 정의하고 있습니다.

```
<contains_JobObject xsi:type="com.ibm.datastage.ai.dtm.ds:DSStageSDO" ... name="TRANS" ... stageType="CTransformerStage" ... inputPins="V0S2P1" outputPins="V0S2P2" ... >
```

이 Transformer 스테이지의 입력 핀(inputPins)과 출력 핀(outputPins)은 다음과 같습니다:

- inputPins: "V0S2P1"
- outputPins: "V0S2P2"

이 핀들은 스테이지 간의 데이터 흐름을 나타내며, 각 핀은 데이터의 출발지와 도착지 역할을 합니다.

입력 핀(inputPin)은 이전 스테이지에서 데이터를 가져오는 연결을 나타내며, 출력 핀(outputPin)은 다음 스테이지로 데이터를 보내는 연결을 나타냅니다.

이 경우, 이 Transformer 스테이지는 하나의 입력 핀과 하나의 출력 핀이 있으므로, 한 개의 입력 스트림과 한 개의 출력 스트림이 있음을 알 수 있습니다.

XML 파일의 다른 부분에서 해당 핀들과 관련된 정보를 찾을 수 있습니다. 예를 들어, 각각의 입력 및 출력 연결에 대한 링크 정의와 데이터 매핑을 찾을 수 있습니다. 이 정보를 통해 스테이지 간의 데이터 흐름을 이해하고 분석할 수 있습니다.

## 사용자 입력:
> 만약 inputPins와 outpins가 여러개면, 매핑은 어떻게 알 수 있나?

## ChatGPT 응답:
> 만약 inputPins와 outputPins가 여러 개 있을 경우, XML 파일의 다른 부분에서 각각의 매핑을 찾을 수 있습니다. 매핑 정보는 일반적으로 `<contains_JobObject>` 태그의 다른 하위 태그에서 찾을 수 있으며, 이들 태그는 각 입력 및 출력 핀에 대한 상세 정보를 제공합니다.

이런 매핑 정보를 찾으려면, 다음 단계를 따르세요:

1. 각 입력 핀(inputPins) 및 출력 핀(outputPins)의 이름을 확인합니다. 이러한 이름은 콤마로 구분된 문자열로 나열됩니다. 예를 들어, `inputPins="V0S2P1, V0S2P2"`와 같은 형식입니다.
2. XML 파일에서 각 핀의 정의를 찾습니다. 이는 일반적으로 `<contains_JobObject>`의 하위 태그로 표시됩니다. 이러한 태그는 각 핀에 대한 추가 정보를 포함하며, 데이터 흐름에 대한 매핑 정보를 찾을 수 있는 위치입니다.
3. 매핑 정보를 찾습니다. 일반적으로 `<contains_JobObject>`의 다른 하위 태그에서 찾을 수 있으며, 이들 태그는 각 입력 및 출력 핀에 대한 매핑 정보를 제공합니다. 데이터 매핑은 스테이지 간의 데이터 흐름을 나타내는 링크 객체를 참조할 수 있는데, 이는 일반적으로 `inputLink` 또는 `outputLink`와 같은 속성을 사용하여 나타납니다.

예를 들어, 다음과 같은 입력 핀 및 출력 핀이 있는 Transformer 스테이지가 있다고 가정합니다.

```
<contains_JobObject ... name="TRANS" ... inputPins="V0S2P1, V0S2P2" outputPins="V0S2P3, V0S2P4" ... >
```

이 경우 입력 핀은 V0S2P1, V0S2P2이고 출력 핀은 V0S2P3, V0S2P4입니다. 이제 이러한 핀에 대한 매핑을 찾아야 합니다. XML 파일에서 이러한 핀에 대한 상세 정보를 찾을 수 있는 하위 태그를 찾으세요.

마지막으로, 이러한 매핑 정보를 분석하여 스테이지 간의 데이터 흐름을 파악할 수 있습니다. 이 정보를 사용하여 입력 및 출력 핀 간의 관계를 이해하고 작업의 전체 데이터 흐름을 분석할 수 있습니다.

## 사용자 입력:
> 그러면,         <has_InputPin xsi:type="com.ibm.datastage.ai.dtm.ds:DSInputPinSDO" xmi:id="30" sDOVersion="2.0.0" objectID="c2e76d84.159ac31a.qqpo9uat8.6les3nl.ligjl0.fq23v2o30tlm3opohlbl1" lastModificationTimestamp="2022-12-29T14:39:59.770+0900" optimisticLockID="2" createdByUser="migdev" creationTimestamp="2022-12-10T09:09:09.291+0900" modifiedByUser="migdev" isTargetOf_Link="8" internalID="V0S2P1" pinType="TrxInput" partner="V8S0|V8S0P2" linkType="1" xmetaLockingRoot="c2e76d84.43058877.qqpo9uat8.6lemca9.sr3gev.hub38uffjqtdhjhipmha7" /> 이런 태그가 있을 때, 이와 연결된 outpin 정보를 보여주는 속성이 빠진건가?

## ChatGPT 응답:
> 이 XML 코드에서 `<has_InputPin>` 태그는 입력 핀에 대한 정보를 포함하고 있습니다. 이 태그의 `partner` 속성은 해당 입력 핀과 연결된 출력 핀에 대한 정보를 제공합니다.

```
<has_InputPin ... isTargetOf_Link="8" internalID="V0S2P1" ... partner="V8S0|V8S0P2" ... />
```

이 경우, `partner` 속성은 "V8S0|V8S0P2"라는 값을 가집니다. 이 값은 두 부분으로 나뉩니다:

1. V8S0: 이 부분은 연결된 출력 핀이 속한 스테이지를 나타냅니다.
2. V8S0P2: 이 부분은 해당 스테이지에서 연결된 출력 핀을 나타냅니다.

따라서 이 입력 핀(V0S2P1)은 V8S0 스테이지의 V8S0P2 출력 핀과 연결되어 있음을 알 수 있습니다.

이렇게 연결 정보를 사용하여 입력 핀과 출력 핀 간의 관계를 이해할 수 있습니다. XML 파일에서 더 많은 정보를 찾으려면, 해당 출력 핀(V8S0P2)의 정의를 찾고 해당 스테이지(V8S0)의 정보를 검토하세요. 이를 통해 스테이지 간의 데이터 흐름을 분석하고 이해할 수 있습니다.

## 사용자 입력:
> <contains_JobObject ... name="TRANS" ... inputPins="V0S2P1, V0S2P2" outputPins="V0S2P3, V0S2P4" ... >
여기에서, V0S2P1, V0S2P2 과 V0S2P3, V0S2P4 관계는 알 수 없는 거네.

## ChatGPT 응답:
> 네, 그렇습니다. `<contains_JobObject>` 태그 내에서는 입력 핀(inputPins) 및 출력 핀(outputPins)의 목록만 제공되고, 이들 핀 간의 관계는 직접적으로 알 수 없습니다.

핀 간의 관계를 이해하려면, XML 파일의 다른 부분에서 각 핀에 대한 상세 정보와 연결 정보를 찾아야 합니다. 앞서 언급한 대로 `<has_InputPin>` 및 `<has_OutputPin>`과 같은 태그를 찾아 해당 핀과 연결된 파트너 핀 정보를 확인하십시오.

예를 들어, 앞서 언급한 예에서 입력 핀 V0S2P1이 V8S0 스테이지의 출력 핀 V8S0P2와 연결되어 있음을 확인했습니다. 이러한 연결 정보를 기반으로 입력 및 출력 핀 간의 관계를 이해하고, 작업의 전체 데이터 흐름을 분석할 수 있습니다.

따라서 입력 핀과 출력 핀의 관계를 알아내려면 XML 파일의 여러 부분을 살펴봐야 합니다. 이를 통해 각 스테이지 간의 데이터 흐름을 이해할 수 있습니다.

## 사용자 입력:
> 내말은 Transform 스테이지내에 있는 input과 outpin과의 연결을 표시하는 건 없는 거지?

## ChatGPT 응답:
> 아, 이해했습니다. 네, 맞습니다. `<contains_JobObject>` 태그 내에서 직접적으로 Transformer 스테이지 내부의 입력 핀(inputPins)과 출력 핀(outputPins) 간의 연결이나 매핑 정보를 제공하지는 않습니다.

그러나 Transformer 스테이지의 내부에서 데이터 변환 및 매핑은 스테이지의 속성 중 하나인 'Derivations' 속성에서 찾을 수 있습니다. 이 속성에는 각 출력 컬럼에 대한 연산식이 포함되어 있으며, 이를 통해 입력 핀에서 가져온 데이터를 어떻게 처리하고 출력 핀으로 보내는지를 확인할 수 있습니다.

XML 파일에서 이러한 Derivations 속성을 찾으려면 `<contains_JobObject>` 태그의 하위 태그 중에서 해당 Transformer 스테이지와 관련된 태그를 찾아야 합니다. 일반적으로 이러한 태그는 `xsi:type="com.ibm.datastage.ai.dtm.ds:DSStageProperty"` 등의 속성을 가진 `<contains_StageProperty>` 태그에 포함되어 있습니다.

예를 들어, 다음과 같은 Derivations 속성을 가진 `<contains_StageProperty>` 태그가 있다고 가정합니다.

```
<contains_StageProperty ... propertyName="Derivations" ...>
  ...
  <stagePropertyValuePair ... columnName="outputColumn1" expression="inputColumn1 + inputColumn2" />
  <stagePropertyValuePair ... columnName="outputColumn2" expression="inputColumn3 * 2" />
  ...
</contains_StageProperty>
```

이 경우, 입력 핀에서 가져온 데이터(inputColumn1, inputColumn2, inputColumn3)가 어떻게 처리되어 출력 핀의 데이터(outputColumn1, outputColumn2)로 변환되는지를 확인할 수 있습니다.

따라서 입력 핀과 출력 핀 간의 연결 및 매핑을 이해하려면, 이러한 Derivations 속성을 참조하여 Transformer 스테이지 내부의 데이터 처리 방식을 분석해야 합니다. 이를 통해 각 스테이지에서 어떻게 데이터가 처리되고 변환되는지 이해할 수 있습니다.

