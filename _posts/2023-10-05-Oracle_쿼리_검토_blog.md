---
layout: post
title: "Oracle 쿼리 검토"
tags: [쿼리, Oracle, 검토]
---

## 사용자 입력:
> 다음은 오라클 쿼리인데, 잘못된 부분이 있으면 찾아줘. MERGE /* MTEProcessData.updateTblAndColSeq#6 */ 
 INTO AIS0081 M    
USING ( SELECT /*+ LEADING(X1 X2 A X3) USE_HASH(X2) USE_HASH(A) USE_HASH(X3) */ 
               A.SEQ_ID, A.SRC_MTE_COL_ID AS COL_ORDER                            
             , X3.COL_LOC AS NEW_COL_ORDER                                        
          FROM AIS0081 A, AIS8000 X1, AIS0010 X2, TS_GLOBAL_FFD_DTL X3            
         WHERE 1=1                                                                
           AND A.SRC_TABLE_TYPE    = 'fil'                                        
           AND A.SRC_PRJ_ID        = X1.PRJ_ID                                    
           AND A.SRC_MTE_TABLE_ID  = X1.FILE_ID                                   
           AND X2.PRJ_ID           = X1.PRJ_ID                                    
           AND X2.FILE_ID          = X1.FILE_ID                                   
           AND X2.END_DATE         IS NULL                                        
           AND X2.FILE_ANLY_YN     IN ( 'Y', 'R' )                                
           AND X1.BLOCK_GUBUN      IN ( 'CONV', 'SGMB', 'JOIN', 'SGMF' )          
           AND X1.PRJ_ID           = ##{{prj_id}}                                            
           AND X3.IMPCT_PRJ_ID     =  X1.PRJ_ID                                   
           AND X3.PRJ_ID           =  X1.TS_PRJ_ID
           AND X3.PRJ_VER_ID       =  X1.TS_PRJ_VER_ID
           AND X3.FFD_ID           =  X1.IN_TS_FFD_ID                             
           AND X3.NM               =  A.SRC_CAPS_COL_NAME                         
           AND X3.COL_LOC          != A.SRC_MTE_COL_ID                            
      ) Y                                                                         
   ON ( M.SEQ_ID = Y.SEQ_ID )
WHEN MATCHED THEN                                       
 UPDATE SET SRC_MTE_COL_ID = Y.NEW_COL_ORDER

## ChatGPT 응답:
> 처음 보기에 쿼리는 문법적으로 잘못된 부분이 없어 보입니다. 그러나 주요 부분들을 확인하여 검토해보겠습니다:

1. **JOIN 조건**: 여러 테이블과 조인하고 있으므로 JOIN 조건이 올바른지 확인해야 합니다. 각 조인 조건을 검토하여 연관성이 있고, 필요한 필드를 사용했는지 확인해야 합니다.

2. **WHERE 조건**: {% raw %}`WHERE 1=1`{% endraw %}은 항상 참이므로 실제로 필터링하는 역할을 하지 않습니다. 이는 쿼리를 동적으로 생성할 때 유용하게 사용됩니다. 나머지 조건들이 원하는 데이터를 정확하게 필터링하는지 확인해야 합니다.

3. **Hint**: {% raw %}`/*+ LEADING(X1 X2 A X3) USE_HASH(X2) USE_HASH(A) USE_HASH(X3) */`{% endraw %} 부분은 Oracle의 실행 계획을 최적화하는 데 사용되는 힌트입니다. 힌트의 순서와 사용 방법이 적절한지, 또는 필요한지 확인해야 합니다.

4. **Placeholder**: {% raw %}`##{{prj_id}}`{% endraw %} 이 부분은 Placeholder처럼 보입니다. 이 부분에 값을 제대로 삽입(바인드)하려면 해당 프로그램 또는 프레임워크의 문서를 참고해야 합니다.

5. **MERGE 조건**: {% raw %}`ON ( M.SEQ_ID = Y.SEQ_ID )`{% endraw %} 이 부분은 MERGE 할 때 매칭되는 조건입니다. 이 조건이 올바르게 설정되었는지 확인해야 합니다.

6. **UPDATE SET**: 업데이트하려는 필드와 값을 확인합니다. 이 부분도 원하는 방식대로 동작하는지 확인해야 합니다.

위의 사항들을 검토한 후, 실제 데이터베이스에서 실행하여 예상대로 동작하는지 테스트해야 합니다.

