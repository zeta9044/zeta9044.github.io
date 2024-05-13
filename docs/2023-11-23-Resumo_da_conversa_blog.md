---
layout: post
title: "Resumo da conversa"
date: 2023-11-23
tags: [Resumo, conversa, da]
categories: blog
---

## 사용자 입력:
> public class XMLParser extends ChannelWriter{
/*******************************************************************************
 *  All Parser Common Unit
 *  모든 파서가 기본적인 구조는 같게 해야 한다.
 ******************************************************************************/
	private XMLParser parser;

	private boolean debug = false;

	public static FileInfo parseFile = null;
/*******************************************************************************/
	public List<AIXmlTag> XmlTagList = new ArrayList<AIXmlTag>();
	public  static String prjId      = ""; // 프로젝트 ID
	public  static int    fileId     = 0;  // 파일 ID
	public  static int    objectId   = 0;  // 오브젝트 ID
	public  static int    functionId = 0;  // 함수 ID
	private        int    sqlId      = 0;  // SQL ID		// 2019.01.28. LeeYH. Modify... -- public static -> private 으로 변경.
	public  static long   totalLine  = 0;  // 총라인수
	private int    depth 	 = 0;  // 뎁스
	private int sqlVarNo			 = 0;
	private int predepth[] = new int[200];
	private static String sdepth  	 = "";
	public  int variableId			 = 0;
	public  int GrpId				 = 0;
	public  int fseqid				 = 0;
	public  int funcCallId      	 = 0;	// 2010.10.01. LeeYH. Add...
	public  int headerFileId 		 = 0;	// 2010.10.01. LeeYH. Add...
	public  int tagSeq               = 0;	// 2011.11.25. LeeYH. Add...
	private int tagSeqDSXMLPro       = 900000000;	// 2023.05.24. LeeYH. Add... -- LGU. DataStage의 XMLProperties 의 문장내 tagSeq는 따로 부여한다.
	private int tagAttrSeq           = 0;	// 2012.05.09. LeeYH. Add...
	private int pregrpId[] = new int[200];

	public  boolean isSqlmapCheck		     = false;
	public  boolean isFunctionCheck		     = false;
	public  boolean isFunctionCheck2		 = false;
	public  boolean isFunctionCheckINGLIFE   = false;	// 2011.06.28. LeeYH. Add... -- ING생명에서 함수 추출시 사용. "name" 을 가지고 함수명을 추출하는데 함수내에서 "name"을  사용하는 attribute가 존재하여 새롭게 만듬.
	public  boolean isFunctionCheckSQLTEXT   = false;	// 2011.09.02. LeeYH. Add... -- OLAP 파일에서 sqlText부분 SQL추출시 사용
	public  boolean isComponentCheck		 = false;
	public  boolean isPrependCheck		     = false;
	public  boolean isDynamicCheck		     = false;
	public  boolean isPackageNameCheck	     = false;
	public  boolean isBizunitCheck		     = false;
	public  boolean isMethodCheck		     = false;
	public  boolean isTransactionIdCheck     = false;
	public  boolean isComponentNameCheck     = false;	// 2010.09.20. LeeYH. Add... -- 한글명을 추출하기 위해서...
	public  boolean isBizUnitNameCheck       = false;	// 2010.09.20. LeeYH. Add... -- 한글명을 추출하기 위해서...
	public  boolean isMethodNameCheck        = false;	// 2010.09.20. LeeYH. Add... -- 한글명을 추출하기 위해서...
	public  boolean isScreenProgCheck        = false;	// 2010.10.01. LeeYH. Add... -- 솔로몬의 단말 프로그램 여부를 체크하기 위해서..
	public  boolean isActivityCheck          = false;	// 2011.06.27. LeeYH. Add... -- INGLIFE에서 사용...
	public  boolean isTransitionCheck        = false;	// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용. 연관관계를 맺는 부분을 추출하기 위해서...
	public  boolean isFromCheck              = false;	// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용. 연관관계를 맺는 부분을 추출하기 위해서...
	public  boolean isProcessDefinitionCheck = false;	// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용.
	public  boolean isObjectCheck            = false;	// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용.
	public  boolean isIterateCheck           = false;   // 2011.09.29. LeeYH. Add... -- isIterate 메소드 처리하기 위해서...
	private int     isBeansEntry             = 0    ;	// 2012.05.09. LeeYH. Add... -- 0: beans가 아님, 1 : <beans> , 2 : <map>으로 tag 데이터 생성했고 entry값을 생성.
	private int     isBizServiceEntry        = 0    ;	// 2016.09.12. LeeYH. Add... -- 0: bizservice가 아님, 1 : <preference> , 2 : <service> , 3 : <entry>으로 tag 데이터 생성했고 entry값을 생성.
	private int     isTypeAlias              = 0	;	// 2016.09.12. LeeYH. Add... -- 0: typeAlias가 아님, 1 : 추출.
	private int     isStrutsConfig           = 0    ;	// 2017.09.26. LeeYH. Add... -- 0: struts-config가 아님, 1: struts-confg tag, 2: action-mapping tag, 3: action tag
	private boolean isSaveSQL                = false;	// 2022.03.02. LeeYH. Add... -- Informatica내에서 Post SQL인 경우 SQL문 추출하기 위해서.
	private boolean isUnloadSQL              = false;	// 2023.06.15. LeeYH. Add... -- DataStage내에서 unload 용 SQL문을 추출하기 위해서.

	public String  groupName                = "";		// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용. 연관관계를 맺는 부분을 추출하기 위해서...
	public String  fromFuncName             = "";		// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용. 연관관계를 맺는 부분을 추출하기 위해서...
	public String  toFuncName               = "";		// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용. 연관관계를 맺는 부분을 추출하기 위해서...
	public int     fromLine                 = 0;		// 2011.06.28. LeeYH. Add... -- INGLIFE에서 사용. 연관관계를 맺는 부분을 추출하기 위해서...

	private int     startLineENTRY	= 0;	// 2016.09.12. LeeYH. Add...
	private int     startColumnENTRY= 0;	// 2016.09.12. LeeYH. Add...
	private int     startLineOBJ	= 0;	// 2016.09.12. LeeYH. Add...
	public  int     startLine		= 0;
	private int     startColumn		= 0;	// 2012.04.23. LeeYH. Add...
	public String  namespaceName	= "";
	public String  functionName		= "";
	public String  functionType		= "";
	public String  prependValue		= "";
	public String  prependValueClose= "";	// 2011.11.03. LeeYH. Add...
	public String  compareValue     = "";	// 2011.11.03. LeeYH. Add...
	public String  propertyName		= "";
	public String  prePropertyName	= "";
	public String  componentName	= "";
	public String  bizunitName		= "";
	public String  methodName		= "";
	public String  packageName		= "";
	public String  transactionId	= "";
	public String  componentNameKor	= "";	// 2010.09.20. LeeYH. Add... -- 한글명을 추출하기 위해서...
	public String  bizUnitNameKor	= "";	// 2010.09.20. LeeYH. Add... -- 한글명을 추출하기 위해서...
	public String  methodNameKor	= "";	// 2010.09.20. LeeYH. Add... -- 한글명을 추출하기 위해서...
	public int     preLine          = -1;	// 2011.06.22. LeeYH, Add... -- 이전 라인번호 처리.
	public int	   preColumn		= 0 ;	// 2011.07.14 SEJ
	public String  ioGubun          = "";	// 2011.11.25. LeeYH. Add... -- "IN" / "OUT"
	public String  tagName          = "";	// 2011.11.25. LeeYH. Add...
	private String entryName        = "";	// 2016.09.12. LeeYH. Add...
	private String entryValue       = "";	// 2016.09.12. LeeYH. Add...

	public Expression exprWHERE     = null;	// 2011.08.04. LeeYH. Add... -- Dynamic SQL중 "WHERE" 을 처리하기 위한 로직.

	public String analysisYn = "";
	// 2011.06.28. LeeYH. Add...
	// 정의된 function 리스트를 저장하는 함수.
	private	Hashtable<String, String> functionList = new Hashtable<String, String>();
	// 2011.10.07. LeeYH. Add...
	// isEmpty, isEqual, isNotEmpty를 SQL문 내에서 바로 사용할 경우 체크하기 위해서 사용.
	public boolean isIbatisNodeIS = false;
	// 2011.10.10. LeeYH. Add...
	// Dynamic SQL문을 처리하기 위한 변수선언.
	private	TableXML funcDynamicSql = null;
	// 2016.09.12. ShinDM. Add...
	// Dynamic SQL문을 처리하기위한 로직 - 라이브러리 사용
	//private MakeiBatisSQL autoDynamicSql = null;
	private TableXMLManager autoDynamicSql = null;

	// 2011.11.25. LeeYH. Add...
	// solomon에서 bizunit 파일을 처리하기 위해서.
	private AIXmlTag          xmlTag          = null;
	// 2012.05.09. LeeYH. Add...
	private AIXmlTagAttribute xmlTagAttribute = null;
	// 2012.04.23. LeeYH. Add...
	// resultMap을 하나의 Insert문으로 생성한다.
	private String sInsert    = "";
	private String sValues    = "";
	// 2012.04.24. LeeYH. Add...
	// SQL문과 resultMap의 연관 정보를 저장하는 변수.
	private int    resultMapFuncId = -1;
	public String moduleID     = "";	// 2013.12.24. LeeYH. Add..
	private	Hashtable<String, String> typeAliasList = new Hashtable<String, String>();		// 2016.09.12. LeeYH. Add... -- XML의 typeAlias 리스트를 저장한다. 추후 resultClass의 매핑 클래스를 찾기 위해서.
	private String typeAliasName                    = ""  ;	// 2016.09.12. LeeYH. Add... typeAlias명을 저장한다.
	private String selectID                         = ""  ;	// 2016.09.12. LeeYH. Add...
	private String resultClass                      = ""  ;	// 2016.09.12. LeeYH. Add...
	private String parameterClass                   = ""  ;	// 2016.09.12. LeeYH. Add...
	private String resultType                       = ""  ;	// 2016.12.13. LeeYH. Add...
	private String[] resultMapInfo                  = new String[3] ;	// 2016.12.13. LeeYH. Add... -- values = resultMap여부(Y/null) / id명 / class명
	// 2016.09.12. LeeYH. Add...
	// SQL별 property와 compareValue 을 저장한다.
	private	Hashtable<String, HashSet<XMLPropertyBean>> sqlPropertyList = new Hashtable<String, HashSet<XMLPropertyBean>>();
	private String[] propertyInfo = new String[3] ;	// 2016.09.12. LeeYH. Add... -- property와 compareValue 정보를 임시저장. (형식) select id / property 값 / compareValue 값
	private String   typeXML      = "";			// 2016.09.12. LeeYH. Add... -- values = ibatis  / mybatis / ""(=Default)		// 2019.04.16. LeeYH. Modify... -- XML파일인데 두형식이 아닌경우 처리하기 위해서.
	private String   dsSQLGubun   = "";			// 2023.05.17. LeeYH. Add... -- BeforeSQL/AfterSQL = DataStage CDATA내 SQL 추출 구분자.
	private String   dsLIASQL     = "";			// 2023.05.17. LeeYH. Add... -- DSN값('LIASQL_DSN') / Owner값('LIASQL_DSN')
	private String   dsLD_UNLD    = "";			// 2023.09.01. LeeYH. Add... -- DataStage의 Unload(UNLD) / Load(LD) 구분값.
	private boolean  isDSXMLProperties=false;	// 2023.05.17. LeeYH. Add... -- dsSQLGubun을 통해 SQL추출시작을 알리는 flag. 
	private int      _tagSeq = 0;		  		// 2023.06.15. LeeYH. Add... -- Unload용 SQL문 추출을 위해서 전역변수로 변경. // 2023.05.24. LeeYH. Add... -- LGU. DataStage을 위한 백업 변수.
/*******************************************************************************/

	// 2017.01.24. LeeYH. Modify... -- 데이터존재 체크조건으로 사용할 기능 추가.
	// 2016.12.28. LeeYH. Add...
	private String getTypeAliasValue(boolean isExistCheck, String aliasName)
	{
	    String sResult = aliasName;
	    if (isExistCheck) sResult = "";		// 2017.01.24. LeeYH. Add... -- 데이터존재 체크시에는 결과값을 "" 로 넘김.
	    if ( this.typeAliasList != null )
	    {
            if ( this.typeAliasList.containsKey(aliasName.toUpperCase()) )		// 2017.01.24. LeeYH. Modify... -- 대소문자가 틀린경우가 존재하여 대문자로 조회.
            {
				sResult = this.typeAliasList.get(aliasName.toUpperCase()).trim();		// 2017.01.24. LeeYH. Modify... -- 대소문자가 틀린경우가 존재하여 대문자로 조회.
            }
	    }
	    return sResult;
	}

	// 2011.06.28. LeeYH. Add... -- BEGIN
	public int getFunctionList(String aFunctionName) {
		int iFuncId = 0;
		String sFuncId = "";

		sFuncId = this.functionList.get(aFunctionName);
		if ( sFuncId != null && !sFuncId.equals("") )
		{
			iFuncId = Integer.parseInt(sFuncId);
		}

		return iFuncId;
	}

	public void setFunctionList(String aFunctionName, int aFunctionId) {
		this.functionList.put(aFunctionName, "" + aFunctionId);
	}

	// 표현식에서 값만 가져오도는 함수
	public String getExprValue(String aExpr)
	{
		String sReturn = aExpr.trim();
		if (!sReturn.equals("")) {
			int idx = sReturn.indexOf(":");
			if ( idx > -1 && sReturn.length() > idx) {
				sReturn = sReturn.substring(idx+1);
			}
		}
		return sReturn;
	}

	// 2011.06.28. LeeYH. Add... -- END

	public XMLParser(String prjIdPRMT, FileInfo filePRMT, FileInputStream streamPRMT, RuleSet ruleSetPRMT, MultiKeyMap jobSchemaMPPRMT)
	{
		try{
			Reqlog.token("DSLAB XMLParser() start...");

			parseFile 	= filePRMT;
			prjId		= prjIdPRMT;
			fileId    	= filePRMT.nIndex;
			objectId	= 0;
			functionId  = 0;
			this.sqlId		= 0;
			depth 		= 0;
			objectId	= 0;
			totalLine   = 0;
			this.typeAliasList = new Hashtable<String, String>();		// 2016.09.12. LeeYH. Add... -- XML의 typeAlias 리스트를 저장한다. 추후 resultClass의 매핑 클래스를 찾기 위해서.

			// 2011.10.10. LeeYH. Add...
			funcDynamicSql = new TableXML(prjId, fileId);
			// 2016.09.12. ShinDM. Add...
			autoDynamicSql = new TableXMLManager(typeXML,prjId, fileId, parseFile);  // 매개변수 수정해 SDM
			// 2011.11.25. LeeYH. Add...
			// solomon에서 bizunit 파일을 처리하기 위해서.
			xmlTag = new AIXmlTag(prjId, fileId);

			Reqlog.token("DSLAB XMLParser() JavaCharStream start...");
			try { jj_input_stream = new JavaCharStream(streamPRMT, null, 1, 1); } catch(java.io.UnsupportedEncodingException e)
		    {
				Reqlog.info("path :"+ parseFile.srcPath + ", Error : " + e.toString());
		    }
			Reqlog.token("DSLAB XMLParser() XMLParserTokenManager start...");
		    token_source = new XMLParserTokenManager(jj_input_stream);
			Reqlog.token("DSLAB XMLParser() new Token() start...");
		    token = new Token();
	
			Reqlog.token("DSLAB XMLParser()  token_source.getNextToken() start...");
		    token.next = jj_nt = token_source.getNextToken();
		    jj_gen = 0;
			Reqlog.token("DSLAB XMLParser()  jj_la1.length start...");
		    for (int i = 0; i < jj_la1.length; i++) jj_la1[i] = -1;
			Reqlog.token("DSLAB XMLParser()  jj_2_rtns.length start...");
	    	for (int i = 0; i < jj_2_rtns.length; i++) jj_2_rtns[i] = new JJCalls();
		} catch(Throwable e) {
			analysisYn = ConstDef.YN_N;
            parseFile.setErrMsg(e.toString());	// 2010.11.26. LeeYH. Add...
			Reqlog.error("path :"+ parseFile.srcPath + ", Lexer Error : ", e);
        }
	}

	public void parse()
	{
		String msg = "";

		try {
			if (!"KBBANK".equals(License.getSiteName()) && !"KB_CARD".equals(License.getSiteName())) License.runLicense(moduleID);
//	    	License.runLicense(this.moduleID);
	       	CompilationUnit();
	       	if ("".equals(analysisYn)) analysisYn = ConstDef.YN_Y;
		}catch(Throwable e){
			analysisYn = ConstDef.YN_N;
	        msg = e.toString();
			parseFile.setParserId("P0005");	// 2014.12.30. LeeYH. Add... -- ParserID 셋팅.
	    }finally {
			parseFile.setParserId("P0005");	// 2014.12.30. LeeYH. Add... -- ParserID 셋팅.
			// 2011.11.07. LeeYH. Modify...
			// 에러 메세지 : analysisYn : [N] , path :DTB_CB_CTI_CUBAS_M_00.xsql, Error : java.lang.OutOfMemoryError: Java heap space
			// 위 에러 메세지 처럼 너무 많은 SQL문이 생성되어 미분석 프로그램으로 떨어진경우 SQL문 자체가 불필요하므로 제외시킨다.
	    	if (analysisYn == ConstDef.YN_Y)
	    	{
				// 추출한 SQL문을 sqlsrc.dat 파일에 저장한다.
				funcDynamicSql.writeSQL(this.typeXML);
				// 2016.09.12. ShinDM. Add...
				try
				{
					autoDynamicSql.WriteDynamicSQL(namespaceName,sqlPropertyList, this.sqlId);
				} catch (Exception e) {
			        msg = e.toString();
					analysisYn = ConstDef.YN_N;
				}
	    	}
			funcDynamicSql.clear();
			// 2016.09.12. ShinDM. Add...
			autoDynamicSql.clear();
			// 파일정보 저장.
            if ("".equals(msg)) parseFile.setErrMsg(msg);	// 2010.11.26. LeeYH. Add...
	    	parseFile.setSrcPath(StringUtil.replace(parseFile.getSrcPath(), ".TPJB", ""));
	    	parseFile.setOriSrcPath(StringUtil.replace(parseFile.getOriSrcPath(), ".TPJB", ""));
			addFile( prjId, fileId, parseFile, analysisYn );
	    	if (analysisYn == ConstDef.YN_N)
	    	{
				Reqlog.error("analysisYn : [" + analysisYn + "] " + ", path :"+ parseFile.srcPath + ", Error : " + msg);
//				System.out.println("AAA analysisYn : [" + analysisYn + "] " + ", path :"+ parseFile.srcPath + ", Error : " + msg);
			}
		}
	}

	private void sqlCheck(String functionName, String propertyName, String gubun, String compareValue, int sqlline, int column, String value)
	{
	  	// 2019.08.12. LeeYH. Add...
	  	String paramClass = this.parameterClass;
	  	String rsltClass  = "";
	  	if ( !this.resultClass.equals("") || !this.resultType.equals("") )
	  	{
			if ( this.resultClass.length() > 0 && this.resultClass.indexOf("java.") > -1 )
			{
			    // 2020.01.31. LeeYH. Add...
				// resultType : java.lang.String / java.util.HashMap / java.lang.Int 인 경우 [this.resultClass + "_$L" + sqlline + "QP"+ prjId + "F"+ fileId] 형식을 취한다.
			    rsltClass  = this.resultClass + "_$L" + sqlline + "QP"+ prjId + "F"+ fileId + "::" + this.resultType ;
			}
			else
			{
			    // 기존 로직.
		  		rsltClass = this.resultClass + "::" + this.resultType;
			}
	  	}
	  	// parameterClass가 없는 경우 함수명으로 처리한다.
	  	if (paramClass.equals("") && !functionName.equals("")) paramClass = functionName + "_$L" + sqlline + "QP"+ prjId + "F"+ fileId +"_PRMT";
	  	// resultClass / resultType 이 없는 경우 함수명으로 처리한다.
	  	if (rsltClass.equals("")  && !functionName.equals("")) rsltClass  = functionName + "_$L" + sqlline + "QP"+ prjId + "F"+ fileId ;
		// 2020.01.31. LeeYH. Add...
		// parameterType : java.lang.String / java.util.HashMap / java.lang.Int 인 경우 [this.parameterClass + "_$L" + sqlline + "QP"+ prjId + "F"+ fileId +"_PRMT"] 형식을 취한다.
		if ( paramClass.length() > 0 && paramClass.indexOf("java.") > -1 )
		{
		    paramClass  = paramClass + "_$L" + sqlline + "QP"+ prjId + "F"+ fileId +"_PRMT" ;
		}

	    // 2016.09.12. LeeYH. Modify...
		// 2011.10.10. LeeYH. Add...
		// SQL문내에서 사용한 property문을 저장한다.
		funcDynamicSql.putFuncSqlUnit(functionName, propertyName, ""+sqlline, ""+column, gubun, compareValue, value, paramClass, rsltClass);
		// 2016.09.12. ShinDM. Add...
		autoDynamicSql.putFuncSqlUnit(functionName, propertyName, ""+sqlline, ""+column, gubun, compareValue, value, paramClass, rsltClass);
	}
	// 2016.12.13. LeeYH. Add...
	// ResultMap 처리를 위해서 만듬.
    private void sqlWriter(String gubun, String sqlSrc, int startLine, int startColumn)
    {
	    sqlWriter(gubun, sqlSrc, startLine, startColumn, "", "");
    }
    // 2023.06.15. LeeYH. Add...
    // DataStage내의 Unload용 SQL문 추출하기 위해서 파라메터를 추가한다.
    private void sqlWriter(String gubun, String sqlSrc, int startLine, int startColumn, String _SqlType, String _tagSeq)
    {
    	if ( !"".equals(sqlSrc.trim()) )
    	{
    	  	if ( !gubun.equals("SQL_INFA") )  this.sqlId = autoDynamicSql.getSqlId();	// 2017.01.02. LeeYH. Add... -- MAX(sqlId) 값을 구한다.
    		// SQL문장을 추출한다.
			AISql sql = new AISql();
			sql.setPrjId      (prjId  );					// 프로젝트번호
			sql.setFileId     (fileId );					// 파일번호
			sql.setObjId      (objectId );						// 오브젝트번호
			sql.setFuncId     (functionId );					// 함수번호
			sql.setSqlId      (++this.sqlId );							// SQL ID
			sql.setBeginLine  (startLine  );		// 시작 라인 번호
			sql.setBeginColumn(startColumn);		// 시작 컬럼 번호
			sql.setSqlSrc     (sqlSrc );						// SQL 소스
			sql.setParameterClass("");
			sql.setResultClass("");
			autoDynamicSql.setSqlId(this.sqlId);	// 2017.01.02. LeeYH. Add... -- MAX(sqlId) 값을 저장한다.

			if ("".equals(_SqlType))
			{
			 	// 기존 로직
			 	
				// 무조건 외부호출함수가 있는 경우 'dC' 로 넣음.
				int idx1 = sqlSrc.indexOf("*<");
				int idx2 = sqlSrc.indexOf(">*", idx1);
				if ( idx1 == -1 && idx2 == -1 )
				{
					if ( gubun.equals("RESULTMAP") )
					{
						sql.setSqlType    (AISql.RESULTMAP);		// SQL구문 - resultMap을 가지고 만든것을 의미
					}
					else if ( gubun.equals("SQL") || gubun.equals("SQL_INFA") )				// 2022.03.02. LeeYH. Modify...
					{
						sql.setSqlType    (AISql.EMBEDED );			// SQL구문 - embeded SQL
					}
					else
					{
						sql.setSqlType    (AISql.DYNAMIC );			// SQL구문 - Dynamic SQL
					}
				}
				else
				{
					sql.setSqlType    (AISql.CALLING );				// SQL구문 - Calling 함수를 포함한 Dynamic SQL
				}
			}
			else
			{
			  	// 2023.06.15. LeeYH. Add...
			  	sql.setSqlType    (_SqlType);
                sql.setShArgKeyCmd(_tagSeq);
			}

			// 완성된 SQL문 여부에 상관없이 생성된 모든 SQL문을 저장한다.
			// 외부함수 호출시 사용하기 위해서 저장한다.
			ChannelWriter.addSqlSrcALL("IMPACT", sql);

			// ---------------------------------------------------------------------
			// 다음은 SQL문인 것만 체크하여 sqlsrc.dat파일에 저장한다.
			// SQL Parser로 분석할 SQL문만을 따로 저장한다.
			// ---------------------------------------------------------------------

			// SQL문 체크하는 로직을 공통화 시킴.
    		SQLUnitCheck SQLUnitCheck = new SQLUnitCheck();
    		if (SQLUnitCheck.SQLCheck(sqlSrc))
    		{
    			// 반드시 "*[" 가 첫번째로 와야지 정상적인 sql문으로 간주한다.(항상 *[라인,컬럼]*이 오기 때문이다.)
    			if (sqlSrc.indexOf("*[",0) == 0 && sqlSrc.indexOf("]*",sqlSrc.indexOf("*[",0)) > -1)
    			{
        			ChannelWriter.addSqlSrc(sql);
        			sql = null;
    			}
    		}
    		// 2022.03.02. LeeYH. Add...
    		// Informatica 소스에서 추출한 SQL문은 무조건 추출한다.
    		if (gubun.equals("SQL_INFA"))
    		{
    			ChannelWriter.addSqlSrc(sql);
    			sql = null;
    		}
    	}
//		System.gc();	// 2017.03.16. LeeYH. Delete...  -- MetaPopulator.dat 처리시 속도 저하로 인해서 삭제.
    }

	// 2016.09.12. LeeYH. Modify...
	// functionId증가 여부와 funcMemType 의 파라메터를 추가.
	// 2011.06.28. LeeYH. Add...
	// addFunction에 저장하는 함수.
	public boolean setAddFunction(String aFunctionName, int aStartLine, int aEndLine, String resultMapYN, boolean isFuncId, String aFuncMemType)
	{
		boolean bReturn = true;
		// 2016.09.12. LeeYH. Add...
		if (isFuncId)	{	++functionId;	}
		addFunction(prjId,                  // 프로젝트 ID String prjId
        	 		fileId,                	// 파일ID int fileId
        	 		objectId,               // 오브젝트ID int objectId
        	 		functionId,        		// 함수ID  int functionId
        	 		aFunctionName,          // 함수명 String functionName
        	 		aStartLine,             // 시작라인 int start
        	 		aEndLine,				// 종료라인 int end
        	 		"",     				// String funcFullName
        	 		"",       				// 리턴타입 String retType
        	 		"",      				// 접근기술자 String accType
        	 		0,               		// 인자수 int ArgsCnt
       	 			"",        				// 예외처리 String exceptionName
       	 			"N",                   	// 생성자여부 String constYn
       	 			aFuncMemType,      		// 메모리유형 String funcMemType			// 2016.09.12. LeeYH. Modify...
       	 			"",          			// 인자타입 String funcArgsType
       	 			"N",            		// static 여부 String staticYn
       	 			"N");            		//

		// 함수정보를 저장한다.
		setFunctionList(aFunctionName, functionId);
		// 2011.10.11. LeeYH. Add...
		// SQL문을 만들기위해서 함수정보를 저장한다.
		funcDynamicSql.addFunctionInfo(aFunctionName, ""+objectId, ""+functionId, resultMapYN, ""+resultMapFuncId);
		// 2016.09.12. ShinDM. Add...
		autoDynamicSql.addFunctionInfo(aFunctionName, ""+objectId, ""+functionId, resultMapYN, ""+resultMapFuncId);

		isIbatisNodeIS = false;	// 2011.10.07. LeeYH. Add...

		return bReturn;
	}
}
이 자바코드에서 CRITICAL_PUBLIC_VARIABLE_WITHOUT_FINAL_MODIFIER  취약점 부분을 알려줘.

## ChatGPT 응답:
> `CRITICAL_PUBLIC_VARIABLE_WITHOUT_FINAL_MODIFIER`는 Java에서 공개(public)으로 선언된 변수들이 `final`로 선언되지 않아 변경될 수 있는 상태임을 나타내는 취약점입니다. 이는 불변성을 보장하지 않기 때문에 코드의 예측 가능성과 안정성을 저하시킬 수 있습니다. 코드를 검토한 결과, 아래의 공개(public) 변수들이 `final`로 선언되지 않았습니다:

1. `public static FileInfo parseFile = null;`
2. `public List<AIXmlTag> XmlTagList = new ArrayList<AIXmlTag>();`
3. `public static String prjId = "";`
4. `public static int fileId = 0;`
5. `public static int objectId = 0;`
6. `public static int functionId = 0;`
7. `public static long totalLine = 0;`
8. `public int variableId = 0;`
9. `public int GrpId = 0;`
10. `public int fseqid = 0;`
11. `public int funcCallId = 0;`
12. `public int headerFileId = 0;`
13. `public int tagSeq = 0;`
14. `public String groupName = "";`
15. `public String fromFuncName = "";`
16. `public String toFuncName = "";`
17. `public int fromLine = 0;`
18. `public int startLine = 0;`
19. `public String namespaceName = "";`
20. `public String functionName = "";`
21. `public String functionType = "";`
22. `public String prependValue = "";`
23. `public String prependValueClose = "";`
24. `public String compareValue = "";`
25. `public String propertyName = "";`
26. `public String prePropertyName = "";`
27. `public String componentName = "";`
28. `public String bizunitName = "";`
29. `public String methodName = "";`
30. `public String packageName = "";`
31. `public String transactionId = "";`
32. `public String componentNameKor = "";`
33. `public String bizUnitNameKor = "";`
34. `public String methodNameKor = "";`
35. `public String ioGubun = "";`
36. `public String tagName = "";`
37. `public String analysisYn = "";`

이들 변수는 상수로 사용되거나 변경되지 않아야 할 값이라면 `final`로 선언되어야 합니다. 그렇지 않고 변경 가능한 상태가 필요하다면 현재 상태를 유지할 수 있습니다. 그러나 불변성을 요구하는 상황에서는 `final`을 추가하는 것이 좋습니다.

