---
layout: post
title: "JUnit 테스트 실행 전체 빌드"
tags: [전체, 테스트, JUnit, 빌드, 실행]
---

## 사용자 입력:
> intellj에서 junit test를 실행하는데, 왜 전체빌드를 하지?

## ChatGPT 응답:
> IntelliJ에서 JUnit 테스트를 실행할 때 전체 빌드를 하는 이유는 다음과 같습니다.

1. 의존성 관리: JUnit 테스트를 실행할 때, 테스트 대상 코드와 테스트 코드 사이에 의존성이 존재할 수 있습니다. 이러한 의존성은 코드 변경 시 테스트 결과에 영향을 줄 수 있습니다. 전체 빌드를 실행하면 변경된 코드와 테스트 코드 간의 일관성을 유지할 수 있습니다.

2. 최신 코드 사용: 전체 빌드를 실행하면 변경된 코드를 컴파일하고 적용할 수 있습니다. 테스트를 실행하기 전에 최신 코드를 사용하여 테스트를 보다 정확하게 수행할 수 있습니다.

3. 이전 테스트 결과의 영향 제거: 이전에 실행된 테스트의 결과가 빌드 결과에 영향을 미칠 수 있습니다. 전체 빌드를 실행하면 이전 테스트의 결과를 제거하고 새로운 테스트를 깨끗한 상태에서 실행할 수 있습니다.

따라서 IntelliJ는 JUnit 테스트를 실행할 때 전체 빌드를 실행하여 코드 간의 일관성을 유지하고 최신 코드를 사용하여 정확한 테스트 결과를 제공합니다.

## 사용자 입력:
> 내말은 테스트코드에서 사용하지 않는 코드까지 빌드되니까 물어보는거야.

## ChatGPT 응답:
> 죄송합니다, 이해를 잘못하고 답변드렸습니다. 테스트 코드에서 사용하지 않는 코드까지 빌드하는 이유에 대해 설명해 드리겠습니다.

IntelliJ에서 JUnit 테스트를 실행할 때 전체 빌드를 하는 이유는 테스트 대상 코드와 테스트 코드 사이에 존재하는 의존성을 해결하기 위해서입니다. 테스트 코드는 테스트 대상 코드와 상호작용하므로, 테스트를 실행하기 위해서는 테스트 대상 코드와 해당 코드가 의존하는 다른 클래스 및 리소스들을 빌드해야 합니다.

또한, IntelliJ는 변경된 코드의 일관성을 유지하기 위해 전체 빌드를 실행합니다. 테스트 코드를 실행하기 전에 최신 코드를 컴파일하고 적용하여 테스트를 보다 정확하게 수행할 수 있습니다. 이는 변경된 코드로 인해 테스트 결과에 영향을 줄 수 있는 잠재적인 문제를 방지하기 위함입니다.

하지만 만약에 불필요한 코드나 의존성이 없는 코드가 빌드되고 있다면, IntelliJ 설정 또는 빌드 설정을 확인하여 해당 코드를 제외하도록 구성할 수 있습니다. 이렇게 설정을 조정하면 테스트 실행 시 필요한 코드만 빌드하여 불필요한 시간과 자원 낭비를 줄일 수 있습니다.

## 사용자 입력:
> 어떻게 설정하는지 알려줘

## ChatGPT 응답:
> IntelliJ에서 JUnit 테스트 실행 시 전체 빌드를 하지 않고 필요한 코드만 빌드하도록 설정하는 방법은 다음과 같습니다.

1. 프로젝트 설정 확인: IntelliJ의 "File" 메뉴에서 "Project Structure"를 선택합니다. 또는 키보드 단축키인 "Ctrl + Shift + Alt + S"를 눌러도 설정으로 이동할 수 있습니다.

2. 모듈 설정: "Project Structure" 대화 상자에서 왼쪽 패널에서 "Modules"을 선택합니다. 여기에서 프로젝트에 대한 모듈 설정을 확인할 수 있습니다.

3. 테스트 모듈 확인: "Modules" 패널에서 테스트에 사용되는 모듈을 선택합니다. 일반적으로 "test"라는 이름의 모듈이 있을 것입니다.

4. 의존성 제거: 선택한 테스트 모듈의 의존성을 확인합니다. 불필요한 의존성이 있다면 이를 제거합니다. 의존성 제거는 다음과 같은 방법으로 수행할 수 있습니다.

   - "Dependencies" 탭에서 사용하지 않는 의존성을 선택하고 마우스 오른쪽 클릭한 다음 "Remove"를 선택합니다.
   - 또는 "Dependencies" 탭에서 사용하지 않는 의존성을 선택하고 "-" 버튼을 클릭하여 제거합니다.

5. 필요한 의존성 추가: 필요한 의존성이 누락된 경우, "Dependencies" 탭에서 "+" 버튼을 클릭하여 필요한 의존성을 추가합니다. 이는 테스트에 필요한 외부 라이브러리 또는 모듈 등을 추가하는 것입니다.

6. 저장 및 적용: 변경 사항을 저장하고 적용합니다.

이렇게 설정을 조정하면 IntelliJ에서 JUnit 테스트를 실행할 때 전체 빌드 대신 필요한 코드만 빌드하여 테스트를 실행할 수 있습니다.

## 사용자 입력:
> package com.ids.parser.test;

import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.util.*;

import com.ids.core.ConstDef;
import com.ids.core.Context;
import com.ids.core.cache.CachedObject;
import com.ids.core.data.AISql;
import com.ids.file.FileChannelManager;
import com.ids.license.License;
import com.ids.parser.DB2Mgr;
import com.ids.parser.OracleMgr;
import com.ids.parser.SybaseIQMgr;
import com.ids.parser.XmlMgr;
import com.ids.util.file.FileFilter;
import com.ids.util.file.FileFinder;
import com.ids.util.file.FileInfo;
import com.ids.util.file.LineReader;
import com.ids.util.string.StringUtil;

import junit.framework.TestCase;

public class xmlParserTest extends TestCase {
	private Context ctx = null;

	/**
	 * 2010.01.21. LeeYH. Add...
	 */
	private Vector<Object> addFileListFilter( Vector<Object> files, Vector<FileInfo> addFiles, Vector<String> aFilter ){
		for ( Object o : addFiles ){
			FileInfo fFile = (FileInfo)o;
			for (int i=0; i<aFilter.size(); i++) {
				if (aFilter.get(i).toString().trim().equalsIgnoreCase(fFile.sFileExt.trim())) {
					files.add( o );
					break;
				}
			}
		}
		return files;
	}
	public String setLineColumn(int line, String str)
	{
		String result ="";
		int column = str.indexOf(str.trim())+1;
		result = " *["+line+","+column+"]*"+str.trim();
		return result;
	}
	public void testDumpVisitor(){
  	String prjId = "90";
  	CachedObject cachedObj = null;
	Vector<Object>    fullFiles   = new Vector<Object>();

  	try {
			System.out.println("[---------------xmlParserTest start2----------------]");
  		ctx = Context.getInstance();
		ctx.put(Context.CTX_HOMEDIR, "D:/LIA-Engine/aiworks");
		ctx.put(Context.CTX_REPOSITORYS, "D:/LIAEngine/repositorys");

    	cachedObj = CachedObject.getInstance();

    	if ( !cachedObj.isCache(prjId) )
    			cachedObj.register(prjId, new FileChannelManager( prjId ));

// 			String virtual_path = "D:/LeeYHWork/DataSrc/mybatis";
//    	String virtual_path = "D:/LeeYHWork/DataSrc/xml_srcs";
//	    	String virtual_path = "D:/LeeYHWork/DataSrc/xml_srcs";
	    	String virtual_path = "E:/DataSrc/xml_src";
//    	String virtual_path = "D:/LeeYHWork/DataSrc/NPASsrc/cseries";
//    	String virtual_path = "D:/LeeYHWork/DataSrc/NPASsrc/npas";

			Vector<String> vFilter = new Vector<String>();
			vFilter.add( "PJB" );
//			vFilter.add( "XML" );
	    	FileFinder finder = new FileFinder( new FileFilter(vFilter) );

	    	Vector files = finder.getList(virtual_path);
	    	FileInfo fo = null;
	    	String fullPath = "";

	    	System.out.println("Text Parser Test Start... -------> PrjId [" + prjId + "]");

	    	fullFiles = addFileListFilter( fullFiles, files, vFilter );
	    	int iFileId = 0;
			for ( Object o : fullFiles )
			{
				fo = (FileInfo)o;
	    		if ( fo.isFile > 0 ) {
	    			fo.nIndex = ++iFileId;
	    			fullPath = fo.sFullPath;
	    			fullPath = fullPath.substring(0, fullPath.lastIndexOf(File.separator));
					fo.srcPath = fo.sFullPath;

	    			try {
						System.out.println("File Path : " + fo.sFullPath );
						if (fo.sFileName.equalsIgnoreCase(".DS_Store") == false)
						{
							XmlMgr parser = new XmlMgr();
							parser.parse( prjId, fo, null, null, null );
//							preProcess(fo);
//							XMLParser parser = new XMLParser(prjId,fo, new FileInputStream( fo.sFullPath ));
//							parser.parse();
						}
					}catch(Exception e){
						e.printStackTrace();
					}
	    		}
	    		Thread.currentThread().sleep(10);
	    	}

	    	if ( true ) {
//				TeradataMgr manager = new TeradataMgr();
//				DB2Mgr manager = new DB2Mgr();
				OracleMgr manager = new OracleMgr();
//				SybaseIQMgr manager = new SybaseIQMgr();
					System.out.println("SQL Parser [" + manager.getExtention() + "] START...");

					String parserClass = "";
					BufferedReader br  = null;

					try {
	//						String prjId     = "";
						String fileId    = "";
						String objId     = "";
						String funcId    = "";
						String sqlId     = "";
						String startLine = "";
						String startColumn = "";
						String sqlSrc    = "";
						String sqlType   = "";
						String sqlSrcOrg    = "";
						String cursorName   = "";
						String layoutFieldId= "";
						String relSqlId     = "";
						String sqlName       = "";
						String parameterClass= "";
						String resultClass   = "";
						String resultType    = "";
						String dbType        = "";	// 2016.09.12. LeeYH. Add... -- DB Type을 저장한다. UI에서 SQL Structure 표시시 필요.
						String dbmsTypeCd       = "-1";	// 2018.02.13. LeeYH. Add...
						String sJobId         = "-1";	// 2021.09.03. LeeYH. Add...
						AISql  sql           = new AISql();		// 2022.04.17. LeeYH. Add...

						String buffer    = "";
						String delimeter = "";
						byte [] delim =  { 0x07 };

						String sqlsrcName = ctx.get(Context.CTX_REPOSITORYS) + "/" +  prjId + "/" + ConstDef.SQLSRC;
						BufferedInputStream bis = new BufferedInputStream( new FileInputStream(sqlsrcName) );
						LineReader lr = new LineReader(bis);
						delimeter = new String( delim );
						while ( (buffer = lr.readLine()) != null && !"".equals(buffer.trim()) ){
							try {
								String [] array = StringUtil.strSplit(buffer, delimeter);

	//								prjId         = array[0].trim();
								fileId        = array[ 1].trim();
								objId					= array[ 2].trim();
								funcId        = array[ 3].trim();
								sqlId         = array[ 4].trim();
								startLine     = array[ 5].trim();
								startColumn   = array[ 6].trim();
								sqlSrc        = array[ 7].trim();
								sqlSrcOrg     = array[ 8].trim();
								cursorName    = array[ 9].trim();
	//							sqlGubun      = array[10].trim();
								sqlType       = array[11].trim();	// 2011.01.26. LeeYH. Modify...
								layoutFieldId = array[12].trim();	// 2011.05.13. LeeYH. Add... --
								relSqlId      = array[13].trim();	// 2012.04.23. LeeYH. Add... --
//								defineOwnerName = array[14].trim();	// 2013.05.15. LeeYH. Add... -- defineOwnerName.
								sqlName         = array[15].trim();	// 2013.05.15. LeeYH. Add... -- SQL명.
								// 2016.09.12. LeeYH. Add... -- BEGIN
								if ( License.getExecVersion("1.6") )
								{
									dbType         = array[16].trim();
									parameterClass = array[17].trim();
									resultClass    = array[18].trim();
									resultType     = array[19].trim();	// 2016.12.13. LeeYH. Add...
									dbmsTypeCd     = array[20].trim();	// 2018.02.13. LeeYH. Add...
									if (dbType.equals("") == false)
									{
										// (중요)dbType 이 존재하는 경우 해당 파서로 분석하도록 한다.
									}
									sJobId          = array[21].trim();	// 2021.09.03. LeeYH. Add...
									sql.setShArgKeyCmd  (array[22].trim());		// 2022.04.17. LeeYH. Add...
									sql.setShArgJobId   (array[23].trim());		// 2022.04.17. LeeYH. Add...
									sql.setShArgRsrvName(array[24].trim());		// 2022.04.17. LeeYH. Add...
								}

								System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId +
							  			"] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine + "]");

								// SQL Parser 호占쏙옙占싹울옙 占식쏙옙.
							  	manager.parse(prjId, fileId, funcId, objId, sqlId, startLine, startColumn, sqlSrc, sqlType, layoutFieldId, relSqlId, "DEFINE_OWNER", sqlName, null, parameterClass, resultClass, resultType, dbmsTypeCd, sJobId, null, sql);

//								System.gc();
							}catch(Exception e) {
								System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId +
										  			"] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine + "] \n" +
										  			"====>" + e + "\n");
							}
						}	// while
					}catch(Exception e) {
						System.out.println( "SQL Parser Exception===>" + e);
					}
					finally {
						if ( cachedObj != null ) cachedObj.close( prjId );
						if (br != null) br.close();
						System.out.println("SQL Parser END...");
					}
	    	}
		}catch(Exception e){
			e.printStackTrace();
		}
	}

}
이 코드를 Junit 4.x.x를 이용하는 코드로 코딩해봐.

## ChatGPT 응답:
> 아래는 주어진 코드를 JUnit 4.x.x를 사용하여 수정한 예시입니다.

{% raw %}```java
import org.junit.Test;
import static org.junit.Assert.*;

import java.io.File;
import java.util.Vector;

public class XmlParserTest {

    @Test
    public void testDumpVisitor() {
        String prjId = "90";
        Vector<Object> fullFiles = new Vector<Object>();

        try {
            System.out.println("[---------------XmlParserTest start2----------------]");
            Context ctx = Context.getInstance();
            ctx.put(Context.CTX_HOMEDIR, "D:/LIA-Engine/aiworks");
            ctx.put(Context.CTX_REPOSITORYS, "D:/LIAEngine/repositorys");

            CachedObject cachedObj = CachedObject.getInstance();

            if (!cachedObj.isCache(prjId))
                cachedObj.register(prjId, new FileChannelManager(prjId));

            String virtual_path = "E:/DataSrc/xml_src";
            Vector<String> vFilter = new Vector<String>();
            vFilter.add("PJB");

            FileFinder finder = new FileFinder(new FileFilter(vFilter));

            Vector<File> files = finder.getList(virtual_path);
            FileInfo fo = null;
            String fullPath = "";

            System.out.println("Text Parser Test Start... -------> PrjId [" + prjId + "]");

            for (File file : files) {
                fo = new FileInfo(file);
                if (fo.isFile > 0) {
                    fullPath = fo.sFullPath;
                    fullPath = fullPath.substring(0, fullPath.lastIndexOf(File.separator));
                    fo.srcPath = fo.sFullPath;

                    try {
                        System.out.println("File Path : " + fo.sFullPath);
                        if (!fo.sFileName.equalsIgnoreCase(".DS_Store")) {
                            XmlMgr parser = new XmlMgr();
                            parser.parse(prjId, fo, null, null, null);
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
                Thread.currentThread().sleep(10);
            }

            if (true) {
                OracleMgr manager = new OracleMgr();
                System.out.println("SQL Parser [" + manager.getExtention() + "] START...");

                String parserClass = "";

                try {
                    String buffer = "";
                    String delimeter = "";
                    byte[] delim = { 0x07 };

                    String sqlsrcName = ctx.get(Context.CTX_REPOSITORYS) + "/" + prjId + "/" + ConstDef.SQLSRC;
                    BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sqlsrcName));
                    LineReader lr = new LineReader(bis);
                    delimeter = new String(delim);
                    while ((buffer = lr.readLine()) != null && !"".equals(buffer.trim())) {
                        try {
                            String[] array = StringUtil.strSplit(buffer, delimeter);

                            String fileId = array[1].trim();
                            String objId = array[2].trim();
                            String funcId = array[3].trim();
                            String sqlId = array[4].trim();
                            String startLine = array[5].trim();
                            String startColumn = array[6].trim();
                            String sqlSrc = array[7].trim();
                            String sqlSrcOrg = array[8].trim();
                            String cursorName = array[9].trim();
                            String sqlType = array[11].trim();
                            String layoutFieldId = array[12].trim();
                            String relSqlId = array[13].trim();
                            String sqlName = array[15].trim();
                            String parameterClass = array[17].trim();
                            String resultClass = array[18].trim();
                            String resultType = array[19].trim();
                            String dbmsTypeCd = array[20].trim();
                           

## ChatGPT 응답:
>  String sJobId = array[21].trim();
                            AISql sql = new AISql();

                            System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId
                                    + "] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine + "]");

                            // SQL Parser 호출
                            manager.parse(prjId, fileId, funcId, objId, sqlId, startLine, startColumn, sqlSrc,
                                    sqlType, layoutFieldId, relSqlId, "DEFINE_OWNER", sqlName, null, parameterClass,
                                    resultClass, resultType, dbmsTypeCd, sJobId, null, sql);

                        } catch (Exception e) {
                            System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId
                                    + "] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine
                                    + "] \n" + "====>" + e + "\n");
                        }
                    }
                } catch (Exception e) {
                    System.out.println("SQL Parser Exception===>" + e);
                } finally {
                    if (cachedObj != null)
                        cachedObj.close(prjId);
                    System.out.println("SQL Parser END...");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```{% endraw %}

이렇게 수정된 코드는 {% raw %}`XmlParserTest`{% endraw %} 클래스를 생성하고, 해당 클래스에 {% raw %}`testDumpVisitor`{% endraw %} 메서드를 추가하여 테스트할 수 있습니다. JUnit의 {% raw %}`@Test`{% endraw %} 애너테이션을 사용하여 테스트 메서드로 지정하고, {% raw %}`assertEquals`{% endraw %} 등의 JUnit 단언문(assertion)을 사용하여 테스트 결과를 검증할 수 있습니다.

## 사용자 입력:
>  String prjId = "90"; ctx.put(Context.CTX_HOMEDIR, "D:/LIA-Engine/aiworks");
            ctx.put(Context.CTX_REPOSITORYS, "D:/LIAEngine/repositorys"); String virtual_path = "E:/DataSrc/xml_src"; 이 코드는 사실 테스트할 때 마다 바뀔 수 있는데, 위 코드를 다시 한번 잘 코딩된 것으로 만들어봐.

## ChatGPT 응답:
> 주어진 코드를 테스트할 때마다 변경 가능한 값들을 변수로 추출하여 유연성을 높일 수 있습니다. 아래는 해당 코드를 수정한 예시입니다.

{% raw %}```java
import org.junit.Test;
import static org.junit.Assert.*;

import java.io.File;
import java.util.Vector;

public class XmlParserTest {

    @Test
    public void testDumpVisitor() {
        String prjId = "90";
        String homeDir = "D:/LIA-Engine/aiworks";
        String repositoryDir = "D:/LIAEngine/repositorys";
        String virtualPath = "E:/DataSrc/xml_src";

        try {
            System.out.println("[---------------XmlParserTest start2----------------]");
            Context ctx = Context.getInstance();
            ctx.put(Context.CTX_HOMEDIR, homeDir);
            ctx.put(Context.CTX_REPOSITORYS, repositoryDir);

            CachedObject cachedObj = CachedObject.getInstance();

            if (!cachedObj.isCache(prjId))
                cachedObj.register(prjId, new FileChannelManager(prjId));

            Vector<String> vFilter = new Vector<String>();
            vFilter.add("PJB");

            FileFinder finder = new FileFinder(new FileFilter(vFilter));

            Vector<File> files = finder.getList(virtualPath);
            FileInfo fo = null;
            String fullPath = "";

            System.out.println("Text Parser Test Start... -------> PrjId [" + prjId + "]");

            for (File file : files) {
                fo = new FileInfo(file);
                if (fo.isFile > 0) {
                    fullPath = fo.sFullPath;
                    fullPath = fullPath.substring(0, fullPath.lastIndexOf(File.separator));
                    fo.srcPath = fo.sFullPath;

                    try {
                        System.out.println("File Path : " + fo.sFullPath);
                        if (!fo.sFileName.equalsIgnoreCase(".DS_Store")) {
                            XmlMgr parser = new XmlMgr();
                            parser.parse(prjId, fo, null, null, null);
                        }
                    } catch (Exception e) {
                        e.printStackTrace();
                    }
                }
                Thread.currentThread().sleep(10);
            }

            if (true) {
                OracleMgr manager = new OracleMgr();
                System.out.println("SQL Parser [" + manager.getExtention() + "] START...");

                String parserClass = "";

                try {
                    String buffer = "";
                    String delimeter = "";
                    byte[] delim = { 0x07 };

                    String sqlsrcName = ctx.get(Context.CTX_REPOSITORYS) + "/" + prjId + "/" + ConstDef.SQLSRC;
                    BufferedInputStream bis = new BufferedInputStream(new FileInputStream(sqlsrcName));
                    LineReader lr = new LineReader(bis);
                    delimeter = new String(delim);
                    while ((buffer = lr.readLine()) != null && !"".equals(buffer.trim())) {
                        try {
                            String[] array = StringUtil.strSplit(buffer, delimeter);

                            String fileId = array[1].trim();
                            String objId = array[2].trim();
                            String funcId = array[3].trim();
                            String sqlId = array[4].trim();
                            String startLine = array[5].trim();
                            String startColumn = array[6].trim();
                            String sqlSrc = array[7].trim();
                            String sqlSrcOrg = array[8].trim();
                            String cursorName = array[9].trim();
                            String sqlType = array[11].trim();
                            String layoutFieldId = array[12].trim();
                            String relSqlId = array[13].trim();
                            String sqlName = array[15].trim();
                            String parameterClass = array[17].trim();
                            String resultClass =

## ChatGPT 응답:
>  array[18].trim();
                            String resultType = array[19].trim();
                            String dbmsTypeCd = array[20].trim();
                            String sJobId = array[21].trim();
                            AISql sql = new AISql();

                            System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId
                                    + "] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine + "]");

                            // SQL Parser 호출
                            manager.parse(prjId, fileId, funcId, objId, sqlId, startLine, startColumn, sqlSrc,
                                    sqlType, layoutFieldId, relSqlId, "DEFINE_OWNER", sqlName, null, parameterClass,
                                    resultClass, resultType, dbmsTypeCd, sJobId, null, sql);

                        } catch (Exception e) {
                            System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId
                                    + "] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine
                                    + "] \n" + "====>" + e + "\n");
                        }
                    }
                } catch (Exception e) {
                    System.out.println("SQL Parser Exception===>" + e);
                } finally {
                    if (cachedObj != null)
                        cachedObj.close(prjId);
                    System.out.println("SQL Parser END...");
                }
            }
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```{% endraw %}

위 코드에서 변경 가능한 값들은 {% raw %}`prjId`{% endraw %}, {% raw %}`homeDir`{% endraw %}, {% raw %}`repositoryDir`{% endraw %}, {% raw %}`virtualPath`{% endraw %}입니다. 이렇게 변수로 추출하여 필요에 따라 변경하실 수 있습니다.

## 사용자 입력:
> package com.ids.parser.test;

import java.io.BufferedInputStream;
import java.io.BufferedReader;
import java.io.File;
import java.io.FileInputStream;
import java.util.*;

import com.ids.core.ConstDef;
import com.ids.core.Context;
import com.ids.core.cache.CachedObject;
import com.ids.core.data.AISql;
import com.ids.file.FileChannelManager;
import com.ids.license.License;
import com.ids.parser.DB2Mgr;
import com.ids.parser.OracleMgr;
import com.ids.parser.SybaseIQMgr;
import com.ids.parser.XmlMgr;
import com.ids.util.file.FileFilter;
import com.ids.util.file.FileFinder;
import com.ids.util.file.FileInfo;
import com.ids.util.file.LineReader;
import com.ids.util.string.StringUtil;

import junit.framework.TestCase;

public class xmlParserTest extends TestCase {
	private Context ctx = null;

	/**
	 * 2010.01.21. LeeYH. Add...
	 */
	private Vector<Object> addFileListFilter( Vector<Object> files, Vector<FileInfo> addFiles, Vector<String> aFilter ){
		for ( Object o : addFiles ){
			FileInfo fFile = (FileInfo)o;
			for (int i=0; i<aFilter.size(); i++) {
				if (aFilter.get(i).toString().trim().equalsIgnoreCase(fFile.sFileExt.trim())) {
					files.add( o );
					break;
				}
			}
		}
		return files;
	}
	public String setLineColumn(int line, String str)
	{
		String result ="";
		int column = str.indexOf(str.trim())+1;
		result = " *["+line+","+column+"]*"+str.trim();
		return result;
	}
	public void testDumpVisitor(){
  	String prjId = "90";
  	CachedObject cachedObj = null;
	Vector<Object>    fullFiles   = new Vector<Object>();

  	try {
			System.out.println("[---------------xmlParserTest start2----------------]");
  		ctx = Context.getInstance();
		ctx.put(Context.CTX_HOMEDIR, "D:/LIA-Engine/aiworks");
		ctx.put(Context.CTX_REPOSITORYS, "D:/LIAEngine/repositorys");

    	cachedObj = CachedObject.getInstance();

    	if ( !cachedObj.isCache(prjId) )
    			cachedObj.register(prjId, new FileChannelManager( prjId ));

// 			String virtual_path = "D:/LeeYHWork/DataSrc/mybatis";
//    	String virtual_path = "D:/LeeYHWork/DataSrc/xml_srcs";
//	    	String virtual_path = "D:/LeeYHWork/DataSrc/xml_srcs";
	    	String virtual_path = "E:/DataSrc/xml_src";
//    	String virtual_path = "D:/LeeYHWork/DataSrc/NPASsrc/cseries";
//    	String virtual_path = "D:/LeeYHWork/DataSrc/NPASsrc/npas";

			Vector<String> vFilter = new Vector<String>();
			vFilter.add( "PJB" );
//			vFilter.add( "XML" );
	    	FileFinder finder = new FileFinder( new FileFilter(vFilter) );

	    	Vector files = finder.getList(virtual_path);
	    	FileInfo fo = null;
	    	String fullPath = "";

	    	System.out.println("Text Parser Test Start... -------> PrjId [" + prjId + "]");

	    	fullFiles = addFileListFilter( fullFiles, files, vFilter );
	    	int iFileId = 0;
			for ( Object o : fullFiles )
			{
				fo = (FileInfo)o;
	    		if ( fo.isFile > 0 ) {
	    			fo.nIndex = ++iFileId;
	    			fullPath = fo.sFullPath;
	    			fullPath = fullPath.substring(0, fullPath.lastIndexOf(File.separator));
					fo.srcPath = fo.sFullPath;

	    			try {
						System.out.println("File Path : " + fo.sFullPath );
						if (fo.sFileName.equalsIgnoreCase(".DS_Store") == false)
						{
							XmlMgr parser = new XmlMgr();
							parser.parse( prjId, fo, null, null, null );
//							preProcess(fo);
//							XMLParser parser = new XMLParser(prjId,fo, new FileInputStream( fo.sFullPath ));
//							parser.parse();
						}
					}catch(Exception e){
						e.printStackTrace();
					}
	    		}
	    		Thread.currentThread().sleep(10);
	    	}

	    	if ( true ) {
//				TeradataMgr manager = new TeradataMgr();
//				DB2Mgr manager = new DB2Mgr();
				OracleMgr manager = new OracleMgr();
//				SybaseIQMgr manager = new SybaseIQMgr();
					System.out.println("SQL Parser [" + manager.getExtention() + "] START...");

					String parserClass = "";
					BufferedReader br  = null;

					try {
	//						String prjId     = "";
						String fileId    = "";
						String objId     = "";
						String funcId    = "";
						String sqlId     = "";
						String startLine = "";
						String startColumn = "";
						String sqlSrc    = "";
						String sqlType   = "";
						String sqlSrcOrg    = "";
						String cursorName   = "";
						String layoutFieldId= "";
						String relSqlId     = "";
						String sqlName       = "";
						String parameterClass= "";
						String resultClass   = "";
						String resultType    = "";
						String dbType        = "";	// 2016.09.12. LeeYH. Add... -- DB Type을 저장한다. UI에서 SQL Structure 표시시 필요.
						String dbmsTypeCd       = "-1";	// 2018.02.13. LeeYH. Add...
						String sJobId         = "-1";	// 2021.09.03. LeeYH. Add...
						AISql  sql           = new AISql();		// 2022.04.17. LeeYH. Add...

						String buffer    = "";
						String delimeter = "";
						byte [] delim =  { 0x07 };

						String sqlsrcName = ctx.get(Context.CTX_REPOSITORYS) + "/" +  prjId + "/" + ConstDef.SQLSRC;
						BufferedInputStream bis = new BufferedInputStream( new FileInputStream(sqlsrcName) );
						LineReader lr = new LineReader(bis);
						delimeter = new String( delim );
						while ( (buffer = lr.readLine()) != null && !"".equals(buffer.trim()) ){
							try {
								String [] array = StringUtil.strSplit(buffer, delimeter);

	//								prjId         = array[0].trim();
								fileId        = array[ 1].trim();
								objId					= array[ 2].trim();
								funcId        = array[ 3].trim();
								sqlId         = array[ 4].trim();
								startLine     = array[ 5].trim();
								startColumn   = array[ 6].trim();
								sqlSrc        = array[ 7].trim();
								sqlSrcOrg     = array[ 8].trim();
								cursorName    = array[ 9].trim();
	//							sqlGubun      = array[10].trim();
								sqlType       = array[11].trim();	// 2011.01.26. LeeYH. Modify...
								layoutFieldId = array[12].trim();	// 2011.05.13. LeeYH. Add... --
								relSqlId      = array[13].trim();	// 2012.04.23. LeeYH. Add... --
//								defineOwnerName = array[14].trim();	// 2013.05.15. LeeYH. Add... -- defineOwnerName.
								sqlName         = array[15].trim();	// 2013.05.15. LeeYH. Add... -- SQL명.
								// 2016.09.12. LeeYH. Add... -- BEGIN
								if ( License.getExecVersion("1.6") )
								{
									dbType         = array[16].trim();
									parameterClass = array[17].trim();
									resultClass    = array[18].trim();
									resultType     = array[19].trim();	// 2016.12.13. LeeYH. Add...
									dbmsTypeCd     = array[20].trim();	// 2018.02.13. LeeYH. Add...
									if (dbType.equals("") == false)
									{
										// (중요)dbType 이 존재하는 경우 해당 파서로 분석하도록 한다.
									}
									sJobId          = array[21].trim();	// 2021.09.03. LeeYH. Add...
									sql.setShArgKeyCmd  (array[22].trim());		// 2022.04.17. LeeYH. Add...
									sql.setShArgJobId   (array[23].trim());		// 2022.04.17. LeeYH. Add...
									sql.setShArgRsrvName(array[24].trim());		// 2022.04.17. LeeYH. Add...
								}

								System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId +
							  			"] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine + "]");

								// SQL Parser 호占쏙옙占싹울옙 占식쏙옙.
							  	manager.parse(prjId, fileId, funcId, objId, sqlId, startLine, startColumn, sqlSrc, sqlType, layoutFieldId, relSqlId, "DEFINE_OWNER", sqlName, null, parameterClass, resultClass, resultType, dbmsTypeCd, sJobId, null, sql);

//								System.gc();
							}catch(Exception e) {
								System.out.println("PrjID [" + prjId + "], FileId [" + fileId + "] ObjId [" + objId +
										  			"] FuncId [" + funcId + "] sqlId [" + sqlId + "] startLine [" + startLine + "] \n" +
										  			"====>" + e + "\n");
							}
						}	// while
					}catch(Exception e) {
						System.out.println( "SQL Parser Exception===>" + e);
					}
					finally {
						if ( cachedObj != null ) cachedObj.close( prjId );
						if (br != null) br.close();
						System.out.println("SQL Parser END...");
					}
	    	}
		}catch(Exception e){
			e.printStackTrace();
		}
	}

}
위 코드를 잘 모듈화한 Junit 테스트 코드로 바꿔줘.

## ChatGPT 응답:
> 아래는 모듈화된 JUnit 테스트 코드로 변환된 예시입니다. 테스트 코드를 작성할 때에는 실제 코드에서 사용되는 의존성을 가장 가까운 단위로 대체하거나 목(mock) 객체를 사용하여 테스트하도록 권장합니다.

{% raw %}```java
import org.junit.Before;
import org.junit.Test;

import java.util.Vector;

import static org.junit.Assert.assertEquals;
import static org.mockito.Mockito.*;

public class XmlParserTest {
    private XmlMgr xmlParser;
    private Context ctx;

    @Before
    public void setUp() {
        xmlParser = new XmlMgr();
        ctx = Context.getInstance();
        ctx.put(Context.CTX_HOMEDIR, "D:/LIA-Engine/aiworks");
        ctx.put(Context.CTX_REPOSITORYS, "D:/LIAEngine/repositorys");
    }

    @Test
    public void testXmlParsing() throws Exception {
        // Mock dependencies
        CachedObject cachedObj = mock(CachedObject.class);
        FileChannelManager fileChannelManager = mock(FileChannelManager.class);

        // Set up test data
        String prjId = "90";
        String virtualPath = "E:/DataSrc/xml_src";
        Vector<String> vFilter = new Vector<>();
        vFilter.add("PJB");
        Vector<Object> fullFiles = new Vector<>();
        FileInfo fileInfo = new FileInfo();
        fileInfo.isFile = 1;
        fileInfo.sFullPath = "E:/DataSrc/xml_src/sample.xml";
        fullFiles.add(fileInfo);

        // Stub method calls
        when(cachedObj.isCache(prjId)).thenReturn(false);
        when(cachedObj.register(prjId, fileChannelManager)).thenReturn(true);
        when(cachedObj.getInstance()).thenReturn(cachedObj);
        when(cachedObj.close(prjId)).thenReturn(true);

        FileFinder fileFinder = mock(FileFinder.class);
        when(fileFinder.getList(virtualPath)).thenReturn(fullFiles);

        // Test XML parsing
        xmlParser.setCachedObject(cachedObj);
        xmlParser.setFileChannelManager(fileChannelManager);
        xmlParser.setFileFinder(fileFinder);

        xmlParser.parse(prjId, fileInfo, null, null, null);

        // Verify method calls
        verify(cachedObj, times(1)).isCache(prjId);
        verify(cachedObj, times(1)).register(prjId, fileChannelManager);
        verify(fileChannelManager, times(1)).parse(prjId, fileInfo, null, null, null);
        verify(cachedObj, times(1)).close(prjId);
    }
}
```{% endraw %}

위의 코드는 {% raw %}`XmlMgr`{% endraw %} 클래스의 {% raw %}`parse`{% endraw %} 메서드를 테스트하는 JUnit 테스트 코드입니다. {% raw %}`CachedObject`{% endraw %}, {% raw %}`FileChannelManager`{% endraw %}, {% raw %}`FileFinder`{% endraw %} 등의 의존성을 Mockito 프레임워크를 사용하여 가짜(mock) 객체로 대체하였습니다. 테스트 코드에서는 {% raw %}`parse`{% endraw %} 메서드 호출 전후의 메서드 호출 여부와 횟수를 검증하는 것을 포함하여, 예상한 대로 동작하는지를 확인할 수 있습니다.

## 사용자 입력:
> Caused by: java.lang.ClassNotFoundException: org.hamcrest.SelfDescribing

## ChatGPT 응답:
> The {% raw %}`ClassNotFoundException`{% endraw %} for {% raw %}`org.hamcrest.SelfDescribing`{% endraw %} indicates that the Hamcrest library is not available in the classpath. Hamcrest is a commonly used library for creating assertions in JUnit tests.

To resolve this issue, you need to include the Hamcrest library as a dependency in your project. If you are using a build automation tool like Maven or Gradle, you can add the Hamcrest dependency to your project configuration file.

For Maven, add the following dependency to your {% raw %}`pom.xml`{% endraw %} file:

{% raw %}```xml
<dependency>
    <groupId>org.hamcrest</groupId>
    <artifactId>hamcrest-all</artifactId>
    <version>1.3</version>
    <scope>test</scope>
</dependency>
```{% endraw %}

For Gradle, add the following dependency to your {% raw %}`build.gradle`{% endraw %} file:

{% raw %}```groovy
dependencies {
    testCompile 'org.hamcrest:hamcrest-all:1.3'
}
```{% endraw %}

Make sure to synchronize or recompile your project to fetch the Hamcrest library and resolve the {% raw %}`ClassNotFoundException`{% endraw %}.

## 사용자 입력:
> 위 클래스없이 junit만 가지고 안되나?

## ChatGPT 응답:
> Yes, you can use JUnit without the Hamcrest library. Hamcrest is an additional library that provides more flexible and expressive assertions, but it is not mandatory for writing JUnit tests.

If you prefer to use JUnit without Hamcrest, you can modify the test code by removing the assertions that rely on Hamcrest matchers and use JUnit's built-in assertions instead.

For example, instead of using {% raw %}`assertThat`{% endraw %} with Hamcrest matchers like {% raw %}`is`{% endraw %}, {% raw %}`equalTo`{% endraw %}, or {% raw %}`containsString`{% endraw %}, you can use JUnit's {% raw %}`assertEquals`{% endraw %}, {% raw %}`assertTrue`{% endraw %}, or {% raw %}`assertContains`{% endraw %} methods.

Here's an example of how you can modify the test code to use only JUnit assertions:

{% raw %}```java
import junit.framework.TestCase;

public class XmlParserTest extends TestCase {
    // ...

    public void testDumpVisitor() {
        String prjId = "90";
        // ...

        try {
            // ...

            for (Object o : fullFiles) {
                // ...

                try {
                    System.out.println("File Path : " + fo.sFullPath);
                    if (!fo.sFileName.equalsIgnoreCase(".DS_Store")) {
                        XmlMgr parser = new XmlMgr();
                        parser.parse(prjId, fo, null, null, null);
                        // Perform assertions using JUnit's built-in methods
                        assertNotNull(parser);
                        // ...
                    }
                } catch (Exception e) {
                    e.printStackTrace();
                }
                
                // ...
            }

            // ...
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```{% endraw %}

By making these modifications, you can use JUnit without relying on the Hamcrest library.

## 사용자 입력:
> java에서 System property 설정하는 프로그램 예제 알려줘

## ChatGPT 응답:
> Sure! Here's an example of a Java program that sets a system property:

{% raw %}```java
public class SystemPropertyExample {
    public static void main(String[] args) {
        // Set a system property
        System.setProperty("my.property", "Hello, World!");

        // Get the value of the system property
        String propertyValue = System.getProperty("my.property");
        System.out.println("System Property: " + propertyValue);
    }
}
```{% endraw %}

In this example, we use the {% raw %}`System.setProperty(key, value)`{% endraw %} method to set a system property with the key "my.property" and the value "Hello, World!". Then, we retrieve the value of the system property using {% raw %}`System.getProperty(key)`{% endraw %} and print it to the console.

You can run this program, and it will output:

{% raw %}```
System Property: Hello, World!
```{% endraw %}

Note that setting a system property affects the entire Java virtual machine (JVM) for the duration of its execution. Other parts of your program or other programs running in the same JVM can access the same system property using the same key.

## 사용자 입력:
> try { jj_input_stream = new JavaCharStream(streamPRMT, null, 1, 1); } catch(java.io.UnsupportedEncodingException e)  위 코드는 javacc 코드 인데, 오류가 있나?

## ChatGPT 응답:
> The code you provided is incomplete, as the {% raw %}`streamPRMT`{% endraw %} variable is not defined. However, based on the code snippet you provided, there doesn't seem to be any error related to {% raw %}`java.io.UnsupportedEncodingException`{% endraw %}.

The line {% raw %}`jj_input_stream = new JavaCharStream(streamPRMT, null, 1, 1);`{% endraw %} is creating a new {% raw %}`JavaCharStream`{% endraw %} object, which is typically used as the input stream for the JavaCC parser. The constructor for {% raw %}`JavaCharStream`{% endraw %} takes several arguments, including the input stream ({% raw %}`streamPRMT`{% endraw %}), encoding ({% raw %}`null`{% endraw %} in this case), and line and column numbers.

If you are experiencing an error, it is likely related to another part of your code or the context in which this code is being used. Please provide more information or the complete code snippet if you need further assistance in identifying the issue.

## 사용자 입력:
> 한글로 알려줘

## ChatGPT 응답:
> 제공해 주신 코드에서는 {% raw %}`java.io.UnsupportedEncodingException`{% endraw %}과 관련된 오류는 발생하지 않습니다.

{% raw %}`jj_input_stream = new JavaCharStream(streamPRMT, null, 1, 1);`{% endraw %}라는 코드는 {% raw %}`JavaCharStream`{% endraw %} 객체를 생성하는 부분으로, 일반적으로 JavaCC 파서의 입력 스트림으로 사용됩니다. {% raw %}`JavaCharStream`{% endraw %} 생성자는 여러 인자를 받는데, 그 중 입력 스트림({% raw %}`streamPRMT`{% endraw %}), 인코딩({% raw %}`null`{% endraw %}로 표시됨), 그리고 라인과 컬럼 번호가 있습니다.

만약 오류가 발생한다면, 다른 부분의 코드나 해당 코드가 사용되는 컨텍스트와 관련이 있을 수 있습니다. 문제를 파악하기 위해 더 많은 정보나 전체 코드 스니펫을 제공해 주시면 추가적인 도움을 드릴 수 있습니다.

## 사용자 입력:
> 위 변수 jj_input_stream은 별도로 선언된 부분이 없어도 되는 내장변수인가?

## ChatGPT 응답:
> 제공해 주신 코드에서 {% raw %}`jj_input_stream`{% endraw %} 변수는 별도로 선언된 부분이 없기 때문에 내장 변수로 추측됩니다. 

일반적으로 JavaCC에서는 내장 변수로 {% raw %}`jj_input_stream`{% endraw %}과 같은 이름을 사용하여 입력 스트림을 참조합니다. 이 변수는 JavaCC에서 자동으로 생성되며, 입력 스트림과 관련된 작업을 수행하는 데 사용됩니다.

따라서 {% raw %}`jj_input_stream`{% endraw %} 변수를 사용하기 위해 별도로 선언할 필요 없이 해당 변수를 사용하면 됩니다. 그러나 이 변수를 사용하기 전에 해당 변수가 코드의 다른 부분에서 정의되거나 초기화되었는지 확인하는 것이 좋습니다.

## 사용자 입력:
> jj_nt, jj_gen,jj_la1,jj_2_rtns, JJCalls() 도 동일한 내장인가?

## ChatGPT 응답:
> {% raw %}`jj_nt`{% endraw %}, {% raw %}`jj_gen`{% endraw %}, {% raw %}`jj_la1`{% endraw %}, {% raw %}`jj_2_rtns`{% endraw %}, {% raw %}`JJCalls()`{% endraw %}는 JavaCC에서 자동으로 생성되는 내장 변수나 메서드입니다.

- {% raw %}`jj_nt`{% endraw %}: 현재 토큰을 나타내는 내부 변수입니다.
- {% raw %}`jj_gen`{% endraw %}: 현재 생성된 토큰을 나타내는 내부 변수입니다.
- {% raw %}`jj_la1`{% endraw %}: Lookahead 토큰을 나타내는 내부 변수입니다.
- {% raw %}`jj_2_rtns`{% endraw %}: {% raw %}`jj2()`{% endraw %} 메서드 호출에서 리턴되는 값을 저장하는 내부 변수입니다.
- {% raw %}`JJCalls()`{% endraw %}: 구문 분석 스택에서 호출 정보를 관리하기 위한 내부 클래스입니다.

이러한 내장 변수와 메서드는 JavaCC가 생성한 파서 클래스에서 자동으로 생성되며, 구문 분석과 관련된 작업을 수행하기 위해 사용됩니다. 일반적으로 JavaCC를 사용하여 생성된 파서 클래스 내부에서만 사용되며, 직접적으로 조작하거나 수정할 필요는 없습니다.

## 사용자 입력:
> {
  parserClass="com.github.oowekyala.ijcc.lang.parser.JavaccParser"
  parserUtilClass="com.github.oowekyala.ijcc.lang.parser.JavaccParserUtil"
  parserImports=[
    "com.github.oowekyala.ijcc.lang.psi.JccTypesExt.*"
  ]

  implements="com.github.oowekyala.ijcc.lang.psi.JccPsiElement"
  extends="com.github.oowekyala.ijcc.lang.psi.impl.JccPsiElementImpl"

  elementTypeHolderClass="com.github.oowekyala.ijcc.lang.JccTypes"
  elementTypePrefix="JCC_"
  elementTypeClass="com.github.oowekyala.ijcc.lang.psi.IJccElementType"
  tokenTypeClass="com.github.oowekyala.ijcc.lang.psi.IJccTokenType"
  // factory for stub types
  elementTypeFactory="com.github.oowekyala.ijcc.lang.psi.stubs.StubImplementationsKt.factory"

  psiClassPrefix="Jcc"
  psiImplClassSuffix="Impl"
  psiPackage="com.github.oowekyala.ijcc.lang.psi"
  psiImplPackage="com.github.oowekyala.ijcc.lang.psi.impl"

  // not on the classpath...
  psiImplUtilClass="com.github.oowekyala.ijcc.lang.psi.impl.JccPsiImplUtil"

  generate=[java="8"]

  parserImports = [
  'static com.intellij.psi.JavaTokenType.*'
  ]

  generatePsi=true
  tokens = [
        LOOKAHEAD_KEYWORD         = 'LOOKAHEAD'
        IGNORE_CASE_OPTION        = 'IGNORE_CASE'
        PARSER_BEGIN_KEYWORD      = 'PARSER_BEGIN'
        PARSER_END_KEYWORD        = 'PARSER_END'
        JAVACODE_KEYWORD          = 'JAVACODE'
        TOKEN_KEYWORD             = 'TOKEN'
        SPECIAL_TOKEN_KEYWORD     = 'SPECIAL_TOKEN'
        MORE_KEYWORD              = 'MORE'
        SKIP_KEYWORD              = 'SKIP'
        TOKEN_MGR_DECLS_KEYWORD   = 'TOKEN_MGR_DECLS'
        EOF_KEYWORD               = 'EOF'

        INJECT_KEYWORD            = 'INJECT'
        INCLUDE_KEYWORD           = 'INCLUDE'


        PRIVATE_KEYWORD		= "private"
        PUBLIC_KEYWORD		= "public"
        PROTECTED_KEYWORD	= "protected"
        SUPER_KEYWORD	= "super"
        EXTENDS_KEYWORD	= "extends"

        STATIC_KEYWORD	= "static"

        VOID_KEYWORD	= "void"

        FALSE_KEYWORD	= "false"
        TRUE_KEYWORD	= "true"

        THROWS_KEYWORD	= "throws"
        TRY_KEYWORD	    = "try"
        CATCH_KEYWORD	= "catch"
        FINALLY_KEYWORD	= "finally"

        IDENT       = 'regexp:\p{Alpha}\w*'

        POUND       = "#"
        COLON       = ":"

        LT          = "<"
        GT          = ">"

        LPARENTH	= "("
        RPARENTH	= ")"
        LBRACE		= "{"
        RBRACE		= "}"
        LBRACKET    = "["
        RBRACKET    = "]"

        SEMICOLON	= ";"
        COMMA	    = ","
        ELLIPSIS    = "..."
        DOT         = "."

        EQ          = "="

        UNION       = "|"
        TILDE       = "~"
        ASTERISK    = "*"
        PLUS        = "+"
        MINUS       = "-"
        QUESTION    = "?"

        /* Irrelevant Java tokens */

        EQEQ	    = "=="
        NE	        = "!="
        OROR	    = "||"
        PLUSPLUS	= "++"
        MINUSMINUS	= "--"
        ANDAND	    = "&&"
        AND	        = "&"
        LE	        = "<="
        GE	        = ">="
        PLUSEQ	    = "+="
        MINUSEQ	    = "-="
        ASTERISKEQ	= "*="
        DIVEQ	    = "/="
        ANDEQ	    = "&="
        OREQ	    = "|="
        XOREQ	    = "^="
        PERCEQ	    = "%="
        EXCL	    = "!"
        DIV	        = "/"
        PERC	    = "%"
        AT	        = "@"
        DOUBLE_COLON= "::"
        ARROW	    = "->"

        NULL_KEYWORD	= "null"
        ASSERT_KEYWORD	= "assert"
        ENUM_KEYWORD	= "enum"
        ABSTRACT_KEYWORD= "abstract"
        BREAK_KEYWORD	= "break"
        CASE_KEYWORD	= "case"
        CLASS_KEYWORD	= "class"
        CONST_KEYWORD	= "const"
        CONTINUE_KEYWORD= "continue"
        DEFAULT_KEYWORD	= "default"
        DO_KEYWORD	    = "do"
        ELSE_KEYWORD	= "else"
        FINAL_KEYWORD	= "final"
        FOR_KEYWORD	    = "for"
        GOTO_KEYWORD	= "goto"
        IF_KEYWORD	    = "if"
        IMPLEMENTS_KEYWORD	= "implements"
        IMPORT_KEYWORD	= "import"
        INSTANCEOF_KEYWORD	= "instanceof"
        INTERFACE_KEYWORD	= "interface"
        NATIVE_KEYWORD	= "native"
        NEW_KEYWORD	    = "new"
        PACKAGE_KEYWORD	= "package"
        SWITCH_KEYWORD	= "switch"
        SYNCHRONIZED_KEYWORD	= "synchronized"
        THIS_KEYWORD	= "this"
        THROW_KEYWORD	= "throw"
        TRANSIENT_KEYWORD	= "transient"
        RETURN_KEYWORD	= "return"
        STATIC_KEYWORD	= "static"
        STRICTFP_KEYWORD= "strictfp"
        WHILE_KEYWORD	= "while"
        VOLATILE_KEYWORD= "volatile"

        C_STYLE_COMMENT     = 'regexp:(/\*([^*]*+(\*[^/])?)*\*/)'
        END_OF_LINE_COMMENT = 'regexp:(//[^\r\n]*)'

        DOUBLE_LITERAL      = 'regexp:(0[xb])?[\da-fA-F]*(\.[\da-fA-F]+)?[dD]?'
        INTEGER_LITERAL     = 'regexp:(0[xb])?[\da-fA-F]+'
        STRING_LITERAL      = 'regexp:"([^"\\]|\\.)*"'
        FLOAT_LITERAL       = 'regexp:(0[xb])?[\da-fA-F]*(\.[\da-fA-F]+)?[fF]?'
        LONG_LITERAL        = 'regexp:(0[xb])?[\da-fA-F]+[lL]'
        CHARACTER_LITERAL   = "regexp:'([^\\'\r\n]|\\[^\n\r])'"

        BAD_CHARACTER   = "BAD_CHARACTER"
  ]

  extends("bnf_production") = "com.github.oowekyala.ijcc.lang.psi.impl.JccNonTerminalProductionImpl<com.github.oowekyala.ijcc.lang.psi.stubs.BnfProductionStubImpl>"
  extends("javacode_production") = "com.github.oowekyala.ijcc.lang.psi.impl.JccNonTerminalProductionImpl<com.github.oowekyala.ijcc.lang.psi.stubs.JavacodeProductionStubImpl>"
  implements("(bnf|javacode)_production") = non_terminal_production

  methods("(bnf|javacode)_production") = [header="java_non_terminal_production_header"]

  implements("(bnf|javacode)_production|.*production_header|jjtree_node_descriptor|named_regular_expression|token_reference_regex_unit|ref_regular_expression|non_terminal_production|non_terminal_expansion_unit|option_binding|scoped_expansion_unit")=identifier_owner

  methods(".*production_header|jjtree_node_descriptor|named_regular_expression|token_reference_regex_unit|non_terminal_expansion_unit|option_binding") = [
    nameIdentifier="identifier"
  ]

  methods("jjtree_node_descriptor") = [descriptorExpr="jjtree_node_descriptor_expr"]

  consumeTokenMethod(".*_regex_.*")="consumeTokenFast"

  // Use implements for fake rules
  // Use extends for real rules

  methods("expansion_unit")=[
    scoped_expansion_unit=""
    expansion_unit=""
  ]

  extends(".+_regex_unit")=regex_unit
  extends("regex_sequence_elt|regex_unit|regex_alternative_elt")=regex_element
  extends(".+_regular_expression")=regular_expression
  implements("regular_expression|regex_element")=regex_like

  extends("regex_expansion_unit|non_terminal_expansion_unit")=assignable_expansion_unit
  extends(".*_expansion_unit|parser_actions_unit|local_lookahead_unit|error_jjtree_annot")=expansion_unit
  extends("expansion_(sequence|unit)")=expansion_sequence_or_unit
  extends("expansion_(alternative|sequence_or_unit)")=expansion


  extends("one_or_more|zero_or_more|zero_or_one")=occurrence_indicator
  extends("repetition_range|occurrence_indicator")=occurrence_indicator_or_range

  extends("java_type")=java_return_type

  implements("production|token_manager_decls")="production_like"
  implements("non_terminal_production|regex_production")="production"
  implements("scoped_expansion_unit|non_terminal_production")="com.github.oowekyala.ijcc.lang.psi.JjtNodeClassOwner"
  implements("regex_spec|regex_expansion_unit")="regular_expression_owner"


  stubClass("bnf_production")="com.github.oowekyala.ijcc.lang.psi.stubs.BnfProductionStubImpl"
  stubClass("javacode_production")="com.github.oowekyala.ijcc.lang.psi.stubs.JavacodeProductionStubImpl"
  stubClass("scoped_expansion_unit")="com.github.oowekyala.ijcc.lang.psi.stubs.JccScopedExpansionUnitStub"


}

// Grammar for the JavaCC language
// Compared to the JavaCC grammar used by JavaCC itself, this grammar has a few significant differences:
// * It doesn't embed the Java grammar. Java elements are not parsed and are left to injection
// * The JavaCC grammar has fewer different nodes, but some are used in different syntactic contexts.
//   This grammar uses different nodes to keep the type hierarchy clear.
//   * E.g. in JavaCC, a literal string "foo" may occur either as an expansion unit, as a regular
//     expression (eg in token specs), and as a regex element (e.g. in < foo: "foo" >).
//   * In this grammar, those three contexts correspond to different type hierarchies:
//     JccExpansion, JccRegularExpression, and JccRegexElement. This makes ASTs deeper but
//     allows greater abstraction (on a counterpart, we have to take care when modifying PSI).
// * In JavaCC, an expansion is invariably an expansion alternative (RChoice), with possibly
//   just one child. Children are always expansion sequences (RSequence), with possibly just
//   one child. That tree structure is more predictable, but also much deeper than necessary.
//   Here, expansion alternatives and sequences are only pushed on the tree if they have at
//   least two children. That makes ASTs easier to navigate and query, but also harder to modify.

javacc_input 	    ::=
    grammar_file_root

// Root node of the grammar. Used in place of [JccFile]
// because the file element is not injectable. This allows
// file-wide Java injection.
grammar_file_root   ::=
    option_section?
    parser_declaration? // mandated in regular javacc
    ( production_impl )*
    {name="PARSER_BEGIN"}

option_section 	    ::=
    ("options" | "OPTIONS") [":"] "{" ( option_binding )* "}" {pin=1}

option_binding	    ::=
    option_name ["=" option_value] ";" {pin=1 recoverWhile="#auto"} // only optional in 21

option_value        ::=
    (INTEGER_LITERAL | string_literal | boolean_literal | identifier) // ident only possible in 21

private option_name ::=
        "LOOKAHEAD" | "IGNORE_CASE" | identifier // TODO "static" ?

parser_declaration  ::=
    parser_begin
    java_compilation_unit // TODO inject Java here
    parser_end
    {pin=1 name="PARSER_BEGIN"}

parser_begin        ::=
    "PARSER_BEGIN" "(" identifier ")"
    {pin=1 name="PARSER_BEGIN"}

parser_end          ::=
    "PARSER_END" "(" identifier ")"

/* Productions. */
/* These are the top-level blocks of a JavaCC file. */

/* Type hierarchy is

   JccProductionLike
   |- JccTokenManagerDecls
   |- JccProduction
      |- JccRegexProduction
      |- JccNonTerminalProduction
         |- JccBnfProduction
         |- JccJavacodeProduction
 */

private production_impl             ::=
        javacode_production
    |   bnf_production
    |   regex_production
    |   token_manager_decls
    |   inject_directive
    |   full_inject_directive
    |   grammar_inclusion
    //  errors rules are tried last
    |   error_production
    {recoverWhile="#auto"}


javacode_production                 ::=
        "JAVACODE"
        java_non_terminal_production_header [ jjtree_node_descriptor | error_jjtree_annot_in_header ]
        java_block
        {pin=2}

bnf_production                      ::=
        java_non_terminal_production_header [ jjtree_node_descriptor | error_jjtree_annot_in_header ] ":"
        java_block
        curly_then_expansion_choices
        "}"
        {pin=1}

// Yeah this is weird, but important to recover from incomplete code.
// It fixes the pin behaviour. BNF productions are pinned on their header.
// If the ":" is not present, it will try to match the expansion block anyway.
// Then
// * If the expansion is written "{" expansion_choices "}", the pin behaviour
//   will try to match expansion_choices even if there is no "{". Then the
//   recoverWhile of expansion_unit will take effect and swallow the whole file.
// * If the "{" is isolated in another rule (this one), the expansion_choices will
//   not be tried unless there is a "{".
//
// Keeping the "}" up there allows the production to recover from wrong lookahead
// specs that leave a comma dangling, which would make the failed match for "}"
// fail the whole expansion_choices (and hence drop it) if it wasn't accepted by
// the pin.
// TODO normally, however wrong it may be, a failed expansion unit shouldn't make
// the whole alternative fail. The comma problem is real though, since expansion_choices
// may be followed by a comma (just in syntactic lookaheads)
private curly_then_expansion_choices ::=
        "{" expansion_choices


java_non_terminal_production_header ::=  // TODO inject java here
        java_access_modifier
        java_return_type
        identifier
        java_parameter_list
        [ java_throws_list ]
        {pin=4 recoverWhile="#auto"}

inject_directive ::=
        "INJECT" "(" [ "interface" | "class" ] identifier ")" [":"]
        java_block
        java_block
        {pin=2}

full_inject_directive ::=
        "INJECT" ":" java_block

grammar_inclusion     ::=
        "INCLUDE" "(" (string_literal | identifier) ")" [ ":" "{" "}" ]
        {pin=1 recoverWhile="#auto"}

// incomplete productions fall down one of these productions.
// This is to be able to pin on the name of a production
// so that a valid production always has a name.
// The pin is actually on the parameter list so that there is no
// conflict with following return type (which itself can't be followed by parentheses)
// This allows incomplete code to not break the rest of the grammar file.
// See test file ProductionTolerance.jjt

error_production                    ::=
       error_just_javacode
   |   error_unnamed_or_no_params_production
   |   error_just_lexical_states

// catches single identifiers
private error_unnamed_or_no_params_production ::=
    ["JAVACODE"] java_return_type lone_prod_name no_params_error
    {pin=java_return_type}

private lone_prod_name              ::=
    identifier !(identifier)
    {name="identifier"}

private error_just_javacode         ::=
    "JAVACODE" no_header_error
    {pin=1}

private error_just_lexical_states   ::=
    lexical_state_list no_regex_kind_error
    {pin=1}


// improbable, unsatisfiable production, used to insert an error
// never pin!
private no_params_error         ::= "#" "#" "catch" "#" "throws" "#" "void" "#" "try" {name="parameter list"}
// these just rename it
private no_regex_kind_error     ::= no_params_error {name="regex kind"}
private no_header_error         ::= no_params_error {name="method header"}

java_throws_list ::=
        "throws" java_name ("," java_name)*


jjtree_node_descriptor      ::=
    "#" ( identifier | "void" )
    [ jjtree_node_descriptor_expr ]
    {pin=2}


error_jjtree_annot ::=
    "#" ( identifier | "void" )
    {pin=1}

error_jjtree_annot_in_header ::=
    "#" ( identifier | "void" )
    {pin=1}


// The expr is integer valued for definite nodes, is boolean valued for conditional nodes
jjtree_node_descriptor_expr ::=
    "(" [ ">" ] java_expression ")"

token_manager_decls ::=
        "TOKEN_MGR_DECLS" ":" java_block  {pin=1} // TODO inject java here


lexical_state_list  ::=
        "<" ( "*" | identifier ( "," identifier )* ) ">"
        {pin=1}

regex_kind          ::=
        "TOKEN"
    |   "SPECIAL_TOKEN"
    |   "SKIP"
    |   "MORE"

regex_production    ::=
    [ lexical_state_list ]
    regex_kind [ "[" "IGNORE_CASE" "]" ] ":"
    <<in_block regex_spec_list>>
    {pin="regex_kind"}

private regex_spec_list   ::=
    regex_spec ( "|" regex_spec )*
    {pin=1 recoverWhile="regex_spec_list_recover"}


private regex_spec_list_recover   ::= // exclude ident, most important is the rbrace
    !( 'JAVACODE' | 'MORE' | 'SKIP' | 'SPECIAL_TOKEN' | 'TOKEN' | 'TOKEN_MGR_DECLS' | '}')


regex_spec          ::=
    regular_expression
    [ jjtree_node_descriptor ] // only 21 (no expr is allowed, no #void is allowed)
    [ java_block ]
    [ ":" identifier ]
    {
        methods=[
            lexicalStateTransition="identifier"
            lexicalActions="java_block"
        ]
        recoverWhile="regex_spec_recover"
    }


private regex_spec_recover      ::= // exclude ident, most important is the pipe
    !('<' | 'JAVACODE' | 'MORE' | 'SKIP' | 'SPECIAL_TOKEN' | 'TOKEN' | 'TOKEN_MGR_DECLS' | 'private' | 'protected' | 'public' | 'void' | '|' | '}' | PRIMITIVE_TYPE)

/* Expansions */
/* These only occur within BNF productions. */

/* Type hierarchy is

   JccExpansion
   |- JccExpansionAlternative               seq | seq | ...
   |- JccExpansionSequenceOrUnit
      |- JccExpansionSequence               unit unit ...
      |- JccExpansionUnit
         |- JccLocalLookaheadUnit           LOOKAHEAD(...)
         |- JccParserActionsUnit            { java }
         |- JccParenthesizedExpansionUnit   ( exp )
         |- JccOptionalExpansionUnit        [ exp ]
         |- JccTryCatchExpansionUnit        try { exp } catch (...) { java }
         |- JccAssignedExpansionUnit        lhs = assignable
         |- JccAssignableExpansionUnit
            |- JccRegexExpansionUnit        regular_expression
            |- JccNonterminalExpansionUnit  name(..)
 */


private expansion_choices       ::=
        expansion_alternative
    |   expansion_sequence

expansion_alternative           ::=
    expansion_sequence ( alternative_tail )+
    {methods=[expansionList="expansion_sequence_or_unit"]}

private alternative_tail        ::=
    "|" expansion_sequence
    {pin=1}

expansion_sequence              ::=
    ( expansion_unit )+ {pin=1 recoverWhile="#auto"}



expansion_unit                  ::=
    priv_expansion_unit scoped_expansion_unit? error_jjtree_annot?
    {recoverWhile="#auto"}

left scoped_expansion_unit      ::=
    jjtree_node_descriptor
    { methods=[nameIdentifier="jjtree_node_descriptor/identifier"] }

private priv_expansion_unit     ::=
    (   local_lookahead_unit
    |   parser_actions_unit
    |   parenthesized_expansion_unit
    |   optional_expansion_unit
    |   try_catch_expansion_unit
    |   assigned_expansion_unit
    |   assignable_expansion_unit
    )

assignable_expansion_unit       ::=
      regex_expansion_unit
    | non_terminal_expansion_unit

assigned_expansion_unit         ::=
    java_assignment_lhs "=" assignable_expansion_unit
    {pin=2}

parser_actions_unit             ::=
    java_block

regex_expansion_unit            ::=
    regular_expression


local_lookahead_unit            ::=
    "LOOKAHEAD" "(" local_lookahead_tail ")" {pin=1}

/* TODO error recovery is made considerably harder by this shit
 * Maybe, if the parser accepted some invalid input (like the extra
 * commas and/or arguments), and we reported it in the highlight
 * visitor, we'd fare better.
 */
private local_lookahead_tail    ::=
      // java_expression takes precedence over expansion_choices
      // but only if the construct starts with a "{"
      // JavaCC effectively prohibits e.g. LOOKAHEAD({...}, {...}), so we do the same
      INTEGER_LITERAL [ "," !("{") expansion_choices ]  [ "," braced_expression ]
    | !("{") expansion_choices  [ "," braced_expression ]
    | braced_expression

private braced_expression       ::=
    "{" java_expression "}"

non_terminal_expansion_unit     ::=
    identifier "(" java_expression_list ")" {pin=1 recoverWhile="#auto"}

parenthesized_expansion_unit    ::=
    "(" expansion_choices ")" occurrence_indicator?  {pin=1}

optional_expansion_unit         ::=
    "[" expansion_choices "]"
    {pin=1}

try_catch_expansion_unit        ::=
    "try" "{" [ expansion_choices ] "}"
    (catch_clause)*
    finally_clause?
    {pin="try"}

catch_clause                    ::=
    "catch" "(" java_name identifier ")" java_block

finally_clause                  ::=
    "finally" java_block

/* Regular expressions */
/* These contain regex elements and may not occur as expansions. */
/* When occurring as expansions, they're wrapped in a JccRegexExpansionUnit. */

/* Type hierarchy is

  JccRegularExpression
   |- JccLiteralRegularExpression           "foo"
   |- JccRefRegularExpression               <FOO>
   |- JccNamedRegularExpression             < name: regex_element >
   |- JccContainerRegularExpression         < regex_element >
   |- JccEofRegularExpression               <EOF>
*/


regular_expression              ::=
        literal_regular_expression
    |   eof_regular_expression
    // Pins try to match these in order to allow autocompletion
    // If       ("<" IDENT ":") -> named regex
    // Else if  ("<" IDENT)     -> reference (autocomplete)
    // Else if  "<"             -> inline regex
    |   named_regular_expression
    |   ref_regular_expression
    |   container_regular_expression

literal_regular_expression      ::=
    literal_regex_unit
    {methods=[unit="literal_regex_unit"]}

eof_regular_expression          ::=
    "<" "EOF" ">"

named_regular_expression        ::=
    "<" [ "#" ] identifier ":" complex_regex_choices ">"
    {pin=4}

ref_regular_expression          ::=
    token_reference_regex_unit
    {methods=[unit="token_reference_regex_unit" nameIdentifier="token_reference_regex_unit/identifier"]}

container_regular_expression    ::=
    "<" complex_regex_choices ">"
    {pin=1}

/* Regex elements. */
/* These are always contained within regexes, and are not equivalent. */

/* Type hierarchy is

   JccRegexElement
   |- JccRegexAlternativeElt             seq | seq | ...
   |- JccRegexSequenceElt                unit unit ...
   |- JccRegexUnit
      |- JccLiteralRegexUnit             "foo"
      |- JccTokenReferenceRegexUnit      <FOO>
      |- JccParenthesizedExpansionUnit   ( elt )
      |- JccCharacterListRegexUnit       ["a"-"z", ...]
 */

private complex_regex_choices   ::=
        regex_alternative_elt
    |   regex_sequence_elt

regex_alternative_elt           ::=
    regex_sequence_elt ( "|" regex_sequence_elt )+

regex_sequence_elt              ::=
    regex_unit+ {pin('.*')=1}

regex_unit                      ::=
        literal_regex_unit
    |   token_reference_regex_unit
    |   character_list_regex_unit
    |   parenthesized_regex_unit
    {recoverWhile="#auto"}

literal_regex_unit              ::=
    string_literal

token_reference_regex_unit      ::=
    "<" identifier ">"
    {pin=identifier}

parenthesized_regex_unit        ::=
    "(" complex_regex_choices ")" occurrence_indicator_or_range?
    {methods=[occurrence_indicator="occurrence_indicator_or_range"]}

character_list_regex_unit       ::=
    [ "~" ] "[" [ character_descriptor ( "," character_descriptor )* ] "]" {pin="["}

character_descriptor            ::=
    STRING_LITERAL [ "-" STRING_LITERAL ]
    {pin=1 recoverWhile="#auto"}

/* Occurrence indicators */

/* Type hierarchy is
   JccOccurrenceIndicatorOrRange
   |- JccRepetitionRange                 {1, 2}
   |- JccOccurrenceIndicator
      |- JccOneOrMore                    +
      |- JccZeroOrMore                   *
      |- JccZeroOrOne                    ?
 */

occurrence_indicator_or_range   ::= occurrence_indicator | repetition_range
occurrence_indicator            ::= one_or_more | zero_or_one | zero_or_more
one_or_more                     ::= "+"
zero_or_more                    ::= "*"
zero_or_one                     ::= "?"
repetition_range                ::= "{" INTEGER_LITERAL [ "," INTEGER_LITERAL ] "}" {pin=1}

// Meta and fake rules

private meta in_block                   ::= "{" <<param>> "}"
private meta in_parens                  ::= "(" <<param>> ")"
private meta comma_separated_list       ::= [ <<param>> ( ',' <<param>> )* ]

fake identifier_owner           ::= identifier {
    methods=[nameIdentifier="identifier"]
    implements="com.intellij.psi.PsiNameIdentifierOwner"
}

fake regular_expression_owner   ::= regular_expression {
    implements="identifier_owner"
}

fake non_terminal_production    ::= java_non_terminal_production_header [jjtree_node_descriptor] java_block? {
    methods=[
        header="java_non_terminal_production_header"
        nameIdentifier="java_non_terminal_production_header/identifier"
    ]
}

// marker interfaces
fake expansion                  ::=
fake expansion_sequence_or_unit ::=
fake regex_element              ::=
fake regex_like                 ::=
fake production_like            ::=
fake production                 ::=

// Some java grammar

identifier                      ::=    IDENT                {name="identifier" methods=[leaf="IDENT"]}
private string_literal          ::=    STRING_LITERAL       {name="string literal"}

boolean_literal                 ::=    "true" | "false"     {name="boolean literal"}

java_name                       ::=    identifier ("." identifier)* {pin=1}

java_return_type                ::=    "void" | java_type
java_type                       ::=    java_annot* (PRIMITIVE_TYPE  (java_annot* "[" "]")* | java_ref_type)
private java_ref_type           ::=    java_coi_type (java_annot* "[" "]")* | PRIMITIVE_TYPE (java_annot* "[" "]")+ // don't pin
private java_coi_type           ::=    java_name java_type_args?

private java_type_args          ::=    "<" [ java_type_arg ( ',' java_type_arg )* ] ">"
private java_type_arg           ::=    java_ref_type | java_wildcard
private java_wildcard           ::=    "?" (java_wildcard_bound)?               {pin=1 recoverWhile="#auto"}
private java_wildcard_bound     ::=    ("extends" | "super") java_annot* java_ref_type      {pin=1 recoverWhile="#auto"}

java_access_modifier            ::=    [ "public" | "protected" | "private" ]

java_expression_list            ::=    <<comma_separated_list java_expression>>
private java_parameter_list     ::=   "(" [ java_formal_parameter [ ("," java_formal_parameter)* ] ] ")" {name = "parameter list"}
java_formal_parameter           ::=   java_annot* java_type [java_annot* "..."] identifier ("[" "]")* {pin=java_type recoverWhile="#auto"}

java_annot  ::= "@" java_name java_annot_members?
java_annot_members ::= "(" java_expression_list ")" // not really expressions, but whatever

// externals

java_compilation_unit               ::=     ext_java_compilation_unit
external ext_java_compilation_unit  ::=     parseJCompilationUnit

java_block                          ::=     "{" ext_java_block
external ext_java_block             ::=     parseJBlock

java_assignment_lhs                 ::=     ext_java_assignment_lhs
private ext_java_assignment_lhs     ::=     java_name  // FIXME
//external ext_java_assignment_lhs    ::=     parseJAssignmentLhs

java_expression                     ::=     ext_java_expression
external ext_java_expression        ::=     parseJExpression     이것은 javacc의 bnf인데, 내장변수같은게 오류로 표시되지 않으려면 어떻게 해야 하나?

## ChatGPT 응답:
> The code you provided appears to be a configuration file or code snippet related to the JavaCC language. JavaCC is a parser generator for creating Java parsers. It is used to generate Java code from a grammar specification.

The configuration file defines various settings and options for generating the parser. It specifies the parser class, parser utility class, parser imports, element types, token types, packages, and other related details. It also defines the grammar rules and productions using the JavaCC syntax.

The configuration file consists of different sections, such as option_section, parser_declaration, and production_impl. The option_section defines optional settings for the parser. The parser_declaration section specifies the parser declaration. The production_impl section defines different production rules, including javacode_production, bnf_production, regex_production, token_manager_decls, inject_directive, full_inject_directive, grammar_inclusion, and error_production.

Overall, this configuration file defines the grammar rules, settings, and options for generating a parser using JavaCC. It is not the actual grammar specification but rather a configuration file for generating the parser from a grammar specification.

## 사용자 입력:
> intellj javacc plugin 추천해줘

## ChatGPT 응답:
> 저는 2021년 9월 이후로 훈련된 모델이기 때문에 최신 IntelliJ JavaCC 플러그인에 대한 정보가 없습니다. 그러나 IntelliJ에서 JavaCC를 지원하는 여러 플러그인이 있으며, 일반적으로 사용자들에게 추천되는 플러그인 중 몇 가지를 소개해드릴 수 있습니다. 아래의 플러그인들은 IntelliJ에서 JavaCC 개발을 위해 사용되는 인기있는 플러그인입니다:

1. JavaCC Plugin by JetBrains: JetBrains에서 공식적으로 제공하는 JavaCC 플러그인입니다. 이 플러그인은 IntelliJ에서 JavaCC 파일의 구문 강조 표시, 자동 완성, 코드 양식 지원 등을 제공합니다.

2. Grammar-Kit: Grammar-Kit은 IntelliJ에서 구문 정의 언어와 어휘 정의 언어를 사용하여 사용자 정의 언어를 작성할 수 있는 강력한 플러그인입니다. JavaCC를 비롯한 다양한 언어 개발에 유용하게 사용될 수 있습니다.

3. ANTLR v4 grammar plugin: ANTLR v4 문법을 지원하는 플러그인입니다. JavaCC 외에도 다른 파서 생성기를 사용하는 경우에 유용합니다.

이는 일부 추천되는 플러그인 중 일부이며, IntelliJ 플러그인 저장소에서 더 많은 플러그인을 찾을 수 있습니다. IntelliJ의 플러그인 관리자를 사용하여 설치하거나, JetBrains 플러그인 저장소(https://plugins.jetbrains.com/)를 방문하여 플러그인을 검색하고 설치할 수 있습니다.

참고: 최신 버전의 IntelliJ와 호환되는 JavaCC 플러그인을 사용하기 전에 해당 플러그인의 호환성을 확인하는 것이 좋습니다.

## 사용자 입력:
> Welcome to week 7! This week, you will be learning about the support vector machine (SVM) algorithm. SVMs are considered by many to be the most powerful 'black box' learning algorithm, and by posing a cleverly-chosen optimization objective, one of the most widely used learning algorithms today.

As always, if you get stuck on the quiz and programming assignment, you should post on the Discussions to ask for help. (And if you finish early, I hope you'll go there to help your fellow classmates as well.)  해석해줘.

## ChatGPT 응답:
> 안녕하세요! 제 7주에 오신 것을 환영합니다! 이번 주에는 서포트 벡터 머신(SVM) 알고리즘에 대해 배우게 됩니다. SVM은 많은 사람들이 가장 강력한 '블랙 박스' 학습 알고리즘으로 여겨지며, 똑똑하게 선택된 최적화 목적 함수를 설정함으로써 오늘날 가장 널리 사용되는 학습 알고리즘 중 하나입니다.

항상 그렇듯이, 퀴즈나 프로그래밍 과제에서 막히는 경우, 도움을 요청하기 위해 토론 게시판에 글을 올리는 것이 좋습니다. (또한 일찍 마치고 다른 수강생들에게 도움을 주기 위해 토론 게시판에 참여하시는 것도 좋겠습니다.)

이번 주도 열심히 공부하시고 언제든지 질문이 있으면 물어보세요! 저는 여러분을 도와드리기 위해 여기에 있습니다.

## 사용자 입력:
> By now, you've seen a range of difference learning algorithms. With supervised learning, the performance of many supervised learning algorithms will be pretty similar, and what matters less often will be whether you use learning algorithm a or learning algorithm b, but what matters more will often be things like the amount of data you create these algorithms on, as well as your skill in applying these algorithms. Things like your choice of the features you design to give to the learning algorithms, and how you choose the colorization parameter, and things like that. But, there's one more algorithm that is very powerful and is very widely used both within industry and academia, and that's called the support vector machine. And compared to both logistic regression and neural networks, the Support Vector Machine, or SVM sometimes gives a cleaner, and sometimes more powerful way of learning complex non-linear functions. And so let's take the next videos to talk about that. Later in this course, I will do a quick survey of a range of different supervisory algorithms just as a very briefly describe them. But the support vector machine, given its popularity and how powerful it is, this will be the last of the supervisory algorithms that I'll spend a significant amount of time on in this course as with our development other learning algorithms, we're gonna start by talking about the optimization objective. So, let's get started on this algorithm.
:1:29부터 동영상을 재생하고 스크립트를 따라가세요.1:29
In order to describe the support vector machine, I'm actually going to start with logistic regression, and show how we can modify it a bit, and get what is essentially the support vector machine. So in logistic regression, we have our familiar form of the hypothesis there and the sigmoid activation function shown on the right.
:1:50부터 동영상을 재생하고 스크립트를 따라가세요.1:50
And in order to explain some of the math, I'm going to use z to denote theta transpose axiom.
:1:57부터 동영상을 재생하고 스크립트를 따라가세요.1:57
Now let's think about what we would like logistic regression to do. If we have an example with y equals one and by this I mean an example in either the training set or the test set or the cross-validation set, but when y is equal to one then we're sort of hoping that h of x will be close to one. Right, we're hoping to correctly classify that example. And what having x subscript 1, what that means is that theta transpose x must be must larger than 0. So there's greater than, greater than sign that means much, much greater than 0. And that's because it is z, the theta of transpose x is when z is much bigger than 0 is far to the right of the sphere. That the outputs of logistic progression becomes close to one.
:2:44부터 동영상을 재생하고 스크립트를 따라가세요.2:44
Conversely, if we have an example where y is equal to zero, then what we're hoping for is that the hypothesis will output a value close to zero. And that corresponds to theta transpose x of z being much less than zero because that corresponds to a hypothesis of putting a value close to zero.
:3:2부터 동영상을 재생하고 스크립트를 따라가세요.3:02
If you look at the cost function of logistic regression, what you'll find is that each example (x,y) contributes a term like this to the overall cost function, right?
:3:14부터 동영상을 재생하고 스크립트를 따라가세요.3:14
So for the overall cost function, we will also have a sum over all the chain examples and the 1 over m term, that this expression here, that's the term that a single training example contributes to the overall objective function so we can just rush them.
:3:32부터 동영상을 재생하고 스크립트를 따라가세요.3:32
Now if I take the definition for the fall of my hypothesis and plug it in over here, then what I get is that each training example contributes this term, ignoring the one over M but it contributes that term to my overall cost function for logistic regression.
:3:51부터 동영상을 재생하고 스크립트를 따라가세요.3:51
Now let's consider two cases of when y is equal to one and when y is equal to zero. In the first case, let's suppose that y is equal to 1. In that case, only this first term in the objective matters, because this one minus y term would be equal to zero if y is equal to one.
:4:13부터 동영상을 재생하고 스크립트를 따라가세요.4:13
So when y is equal to one, when in our example x comma y, when y is equal to 1 what we get is this term.. Minus log one over one, plus E to the negative Z where as similar to the last line I'm using Z to denote data transposed X and of course in a cost I should have this minus line that we just had if Y is equal to one so that's equal to one I just simplify in a way in the expression that I have written down here.
:4:41부터 동영상을 재생하고 스크립트를 따라가세요.4:41
And if we plot this function as a function of z, what you find is that you get this curve shown on the lower left of the slide. And thus, we also see that when z is equal to large, that is, when theta transpose x is large, that corresponds to a value of z that gives us a fairly small value, a very, very small contribution to the consumption. And this kinda explains why, when logistic regression sees a positive example, with y=1, it tries to set theta transport x to be very large because that corresponds to this term, in the cross function, being small. Now, to fill the support vec machine, here's what we're going to do. We're gonna take this cross function, this minus log 1 over 1 plus e to negative z, and modify it a little bit. Let me take this point 1 over here, and let me draw the cross functions you're going to use. The new pass functions can be flat from here on out, and then we draw something that grows as a straight line, similar to logistic regression. But this is going to be a straight line at this portion. So the curve that I just drew in magenta, and the curve I just drew purple and magenta, so if it's pretty close approximation to the cross function used by logistic regression. Except it is now made up of two line segments, there's this flat portion on the right, and then there's this straight line portion on the left. And don't worry too much about the slope of the straight line portion. It doesn't matter that much. But that's the new cost function we're going to use for when y is equal to one, and you can imagine it should do something pretty similar to logistic regression. But turns out, that this will give the support vector machine computational advantages and give us, later on, an easier optimization problem
:6:38부터 동영상을 재생하고 스크립트를 따라가세요.6:38
that would be easier for software to solve. We just talked about the case of y equals one. The other case is if y is equal to zero. In that case, if you look at the cost, then only the second term will apply because the first term goes away, right? If y is equal to zero, then you have a zero here, so you're left only with the second term of the expression above. And so the cost of an example, or the contribution of the cost function, is going to be given by this term over here. And if you plot that as a function of z, to have pure z on the horizontal axis, you end up with this one. And for the support vector machine, once again, we're going to replace this blue line with something similar and at the same time we replace it with a new cost, this flat out here, this 0 out here. And that then grows as a straight line, like so. So let me give these two functions names. This function on the left I'm going to call cost subscript 1 of z, and this function of the right I'm gonna call cost subscript 0 of z. And the subscript just refers to the cost corresponding to when y is equal to 1, versus when y Is equal to zero. Armed with these definitions, we're now ready to build a support vector machine. Here's the cost function, j of theta, that we have for logistic regression. In case this equation looks a bit unfamiliar, it's because previously we had a minus sign outside, but here what I did was I instead moved the minus signs inside these expressions, so it just makes it look a little different.
:8:12부터 동영상을 재생하고 스크립트를 따라가세요.8:12
For the support vector machine what we're going to do is essentially take this and replace this with cost1 of z, that is cost1 of theta transpose x. And we're going to take this and replace it with cost0 of z, that is cost0 of theta transpose x. Where the cost one function is what we had on the previous slide that looks like this. And the cost zero function, again what we had on the previous slide, and it looks like this. So what we have for the support vector machine is a minimization problem of one over M, the sum of Y I times cost one, theta transpose X I, plus one minus Y I times cause zero of theta transpose X I, and then plus my usual regularization parameter.
:9:21부터 동영상을 재생하고 스크립트를 따라가세요.9:21
Like so. Now, by convention, for the support of vector machine, we're actually write things slightly different. We re-parameterize this just very slightly differently.
:9:33부터 동영상을 재생하고 스크립트를 따라가세요.9:33
First, we're going to get rid of the 1 over m terms, and this just this happens to be a slightly different convention that people use for support vector machines compared to or just a progression. But here's what I mean. You're one way to do this, we're just gonna get rid of these one over m terms and this should give you me the same optimal value of beta right? Because one over m is just as constant so whether I solved this minimization problem with one over n in front or not. I should end up with the same optimal value for theta. Here's what I mean, to give you an example, suppose I had a minimization problem. Minimize over a long number U of U minus five squared plus one. Well, the minimum of this happens to be U equals five.
:10:23부터 동영상을 재생하고 스크립트를 따라가세요.10:23
Now if I were to take this objective function and multiply it by 10. So here my minimization problem is min over U, 10 U minus five squared plus 10. Well the value of U that minimizes this is still U equals five right? So multiply something that you're minimizing over, by some constant, 10 in this case, it does not change the value of U that gives us, that minimizes this function. So the same way, what I've done is by crossing out the M is all I'm doing is multiplying my objective function by some constant M and it doesn't change the value of theta. That achieves the minimum. The second bit of notational change, which is just, again, the more standard convention when using SVMs instead of logistic regression, is the following. So for logistic regression, we add two terms to the objective function. The first is this term, which is the cost that comes from the training set and the second is this row, which is the regularization term.
:11:27부터 동영상을 재생하고 스크립트를 따라가세요.11:27
And what we had was we had a, we control the trade-off between these by saying, what we want is A plus, and then my regularization parameter lambda. And then times some other term B, where I guess I'm using your A to denote this first term, and I'm using B to denote the second term, maybe without the lambda.
:11:51부터 동영상을 재생하고 스크립트를 따라가세요.11:51
And instead of prioritizing this as A plus lambda B, and so what we did was by setting different values for this regularization parameter lambda, we could trade off the relative weight between how much we wanted the training set well, that is, minimizing A, versus how much we care about keeping the values of the parameter small, so that will be, the parameter is B for the support vector machine, just by convention, we're going to use a different parameter. So instead of using lambda here to control the relative waiting between the first and second terms. We're instead going to use a different parameter which by convention is called C and is set to minimize C times a + B. So for logistic regression, if we set a very large value of lambda, that means you will give B a very high weight. Here is that if we set C to be a very small value, then that responds to giving B a much larger rate than C, than A. So this is just a different way of controlling the trade off, it's just a different way of prioritizing how much we care about optimizing the first term, versus how much we care about optimizing the second term. And if you want you can think of this as the parameter C playing a role similar to 1 over lambda. And it's not that it's two equations or these two expressions will be equal. This equals 1 over lambda, that's not the case. It's rather that if C is equal to 1 over lambda, then these two optimization objectives should give you the same value the same optimal value for theta so we just filling that in I'm gonna cross out lambda here and write in the constant C there.
:13:37부터 동영상을 재생하고 스크립트를 따라가세요.13:37
So that gives us our overall optimization objective function for the support vector machine. And if you minimize that function, then what you have is the parameters learned by the SVM.
:13:51부터 동영상을 재생하고 스크립트를 따라가세요.13:51
Finally unlike logistic regression, the support vector machine doesn't output the probability is that what we have is we have this cost function, that we minimize to get the parameter's data, and what a support vector machine does is it just makes a prediction of y being equal to one or zero, directly. So the hypothesis will predict one
:14:13부터 동영상을 재생하고 스크립트를 따라가세요.14:13
if theta transpose x is greater or equal to zero, and it will predict zero otherwise and so having learned the parameters theta, this is the form of the hypothesis for the support vector machine. So that was a mathematical definition of what a support vector machine does. In the next few videos, let's try to get back to intuition about what this optimization objective leads to and whether the source of the hypotheses SVM will learn and we'll also talk about how to modify this just a little bit to the complex nonlinear functions.  해석해줘

## ChatGPT 응답:
> The transcript you provided seems to be a lecture on Support Vector Machines (SVM) and its relation to logistic regression. The instructor begins by mentioning that SVM is a powerful and widely used algorithm for learning complex non-linear functions. They explain that they will start by describing logistic regression and show how it can be modified to obtain the SVM.

In logistic regression, the hypothesis function and sigmoid activation function are introduced. The instructor then defines the variable "z" as the transpose of theta times x. They explain that when y=1 (positive example), they hope the hypothesis will be close to 1, which corresponds to theta transpose x being much greater than 0. Similarly, when y=0 (negative example), they hope the hypothesis will be close to 0, which corresponds to theta transpose x being much less than 0.

Next, the cost function of logistic regression is discussed, where each example contributes a term based on the hypothesis. The instructor points out that when y=1, only the first term in the cost function matters, and when y=0, only the second term matters. They plot the cost functions as curves based on the value of z, showing that large positive or negative values of z result in smaller costs.

To introduce SVM, the instructor modifies the cost function of logistic regression. They replace the curve representing the cost for y=1 with a new cost function that has a flat portion on the right and a straight-line portion on the left. Similarly, they replace the curve representing the cost for y=0 with

## 사용자 입력:
> By now, you've seen a range of difference learning algorithms. With supervised learning, the performance of many supervised learning algorithms will be pretty similar, and what matters less often will be whether you use learning algorithm a or learning algorithm b, but what matters more will often be things like the amount of data you create these algorithms on, as well as your skill in applying these algorithms. Things like your choice of the features you design to give to the learning algorithms, and how you choose the colorization parameter, and things like that. But, there's one more algorithm that is very powerful and is very widely used both within industry and academia, and that's called the support vector machine. And compared to both logistic regression and neural networks, the Support Vector Machine, or SVM sometimes gives a cleaner, and sometimes more powerful way of learning complex non-linear functions. And so let's take the next videos to talk about that. Later in this course, I will do a quick survey of a range of different supervisory algorithms just as a very briefly describe them. But the support vector machine, given its popularity and how powerful it is, this will be the last of the supervisory algorithms that I'll spend a significant amount of time on in this course as with our development other learning algorithms, we're gonna start by talking about the optimization objective. So, let's get started on this algorithm.
:1:29부터 동영상을 재생하고 스크립트를 따라가세요.1:29
In order to describe the support vector machine, I'm actually going to start with logistic regression, and show how we can modify it a bit, and get what is essentially the support vector machine. So in logistic regression, we have our familiar form of the hypothesis there and the sigmoid activation function shown on the right.
:1:50부터 동영상을 재생하고 스크립트를 따라가세요.1:50
And in order to explain some of the math, I'm going to use z to denote theta transpose axiom.
:1:57부터 동영상을 재생하고 스크립트를 따라가세요.1:57
Now let's think about what we would like logistic regression to do. If we have an example with y equals one and by this I mean an example in either the training set or the test set or the cross-validation set, but when y is equal to one then we're sort of hoping that h of x will be close to one. Right, we're hoping to correctly classify that example. And what having x subscript 1, what that means is that theta transpose x must be must larger than 0. So there's greater than, greater than sign that means much, much greater than 0. And that's because it is z, the theta of transpose x is when z is much bigger than 0 is far to the right of the sphere. That the outputs of logistic progression becomes close to one.
:2:44부터 동영상을 재생하고 스크립트를 따라가세요.2:44
Conversely, if we have an example where y is equal to zero, then what we're hoping for is that the hypothesis will output a value close to zero. And that corresponds to theta transpose x of z being much less than zero because that corresponds to a hypothesis of putting a value close to zero.
:3:2부터 동영상을 재생하고 스크립트를 따라가세요.3:02
If you look at the cost function of logistic regression, what you'll find is that each example (x,y) contributes a term like this to the overall cost function, right?
:3:14부터 동영상을 재생하고 스크립트를 따라가세요.3:14
So for the overall cost function, we will also have a sum over all the chain examples and the 1 over m term, that this expression here, that's the term that a single training example contributes to the overall objective function so we can just rush them.
:3:32부터 동영상을 재생하고 스크립트를 따라가세요.3:32
Now if I take the definition for the fall of my hypothesis and plug it in over here, then what I get is that each training example contributes this term, ignoring the one over M but it contributes that term to my overall cost function for logistic regression.
:3:51부터 동영상을 재생하고 스크립트를 따라가세요.3:51
Now let's consider two cases of when y is equal to one and when y is equal to zero. In the first case, let's suppose that y is equal to 1. In that case, only this first term in the objective matters, because this one minus y term would be equal to zero if y is equal to one.
:4:13부터 동영상을 재생하고 스크립트를 따라가세요.4:13
So when y is equal to one, when in our example x comma y, when y is equal to 1 what we get is this term.. Minus log one over one, plus E to the negative Z where as similar to the last line I'm using Z to denote data transposed X and of course in a cost I should have this minus line that we just had if Y is equal to one so that's equal to one I just simplify in a way in the expression that I have written down here.
:4:41부터 동영상을 재생하고 스크립트를 따라가세요.4:41
And if we plot this function as a function of z, what you find is that you get this curve shown on the lower left of the slide. And thus, we also see that when z is equal to large, that is, when theta transpose x is large, that corresponds to a value of z that gives us a fairly small value, a very, very small contribution to the consumption. And this kinda explains why, when logistic regression sees a positive example, with y=1, it tries to set theta transport x to be very large because that corresponds to this term, in the cross function, being small. Now, to fill the support vec machine, here's what we're going to do. We're gonna take this cross function, this minus log 1 over 1 plus e to negative z, and modify it a little bit. Let me take this point 1 over here, and let me draw the cross functions you're going to use. The new pass functions can be flat from here on out, and then we draw something that grows as a straight line, similar to logistic regression. But this is going to be a straight line at this portion. So the curve that I just drew in magenta, and the curve I just drew purple and magenta, so if it's pretty close approximation to the cross function used by logistic regression. Except it is now made up of two line segments, there's this flat portion on the right, and then there's this straight line portion on the left. And don't worry too much about the slope of the straight line portion. It doesn't matter that much. But that's the new cost function we're going to use for when y is equal to one, and you can imagine it should do something pretty similar to logistic regression. But turns out, that this will give the support vector machine computational advantages and give us, later on, an easier optimization problem
:6:38부터 동영상을 재생하고 스크립트를 따라가세요.6:38
that would be easier for software to solve. We just talked about the case of y equals one. The other case is if y is equal to zero. In that case, if you look at the cost, then only the second term will apply because the first term goes away, right? If y is equal to zero, then you have a zero here, so you're left only with the second term of the expression above. And so the cost of an example, or the contribution of the cost function, is going to be given by this term over here. And if you plot that as a function of z, to have pure z on the horizontal axis, you end up with this one. And for the support vector machine, once again, we're going to replace this blue line with something similar and at the same time we replace it with a new cost, this flat out here, this 0 out here. And that then grows as a straight line, like so. So let me give these two functions names. This function on the left I'm going to call cost subscript 1 of z, and this function of the right I'm gonna call cost subscript 0 of z. And the subscript just refers to the cost corresponding to when y is equal to 1, versus when y Is equal to zero. Armed with these definitions, we're now ready to build a support vector machine. Here's the cost function, j of theta, that we have for logistic regression. In case this equation looks a bit unfamiliar, it's because previously we had a minus sign outside, but here what I did was I instead moved the minus signs inside these expressions, so it just makes it look a little different.
:8:12부터 동영상을 재생하고 스크립트를 따라가세요.8:12
For the support vector machine what we're going to do is essentially take this and replace this with cost1 of z, that is cost1 of theta transpose x. And we're going to take this and replace it with cost0 of z, that is cost0 of theta transpose x. Where the cost one function is what we had on the previous slide that looks like this. And the cost zero function, again what we had on the previous slide, and it looks like this. So what we have for the support vector machine is a minimization problem of one over M, the sum of Y I times cost one, theta transpose X I, plus one minus Y I times cause zero of theta transpose X I, and then plus my usual regularization parameter.
:9:21부터 동영상을 재생하고 스크립트를 따라가세요.9:21
Like so. Now, by convention, for the support of vector machine, we're actually write things slightly different. We re-parameterize this just very slightly differently.
:9:33부터 동영상을 재생하고 스크립트를 따라가세요.9:33
First, we're going to get rid of the 1 over m terms, and this just this happens to be a slightly different convention that people use for support vector machines compared to or just a progression. But here's what I mean. You're one way to do this, we're just gonna get rid of these one over m terms and this should give you me the same optimal value of beta right? Because one over m is just as constant so whether I solved this minimization problem with one over n in front or not. I should end up with the same optimal value for theta. Here's what I mean, to give you an example, suppose I had a minimization problem. Minimize over a long number U of U minus five squared plus one. Well, the minimum of this happens to be U equals five.
:10:23부터 동영상을 재생하고 스크립트를 따라가세요.10:23
Now if I were to take this objective function and multiply it by 10. So here my minimization problem is min over U, 10 U minus five squared plus 10. Well the value of U that minimizes this is still U equals five right? So multiply something that you're minimizing over, by some constant, 10 in this case, it does not change the value of U that gives us, that minimizes this function. So the same way, what I've done is by crossing out the M is all I'm doing is multiplying my objective function by some constant M and it doesn't change the value of theta. That achieves the minimum. The second bit of notational change, which is just, again, the more standard convention when using SVMs instead of logistic regression, is the following. So for logistic regression, we add two terms to the objective function. The first is this term, which is the cost that comes from the training set and the second is this row, which is the regularization term.
:11:27부터 동영상을 재생하고 스크립트를 따라가세요.11:27
And what we had was we had a, we control the trade-off between these by saying, what we want is A plus, and then my regularization parameter lambda. And then times some other term B, where I guess I'm using your A to denote this first term, and I'm using B to denote the second term, maybe without the lambda.
:11:51부터 동영상을 재생하고 스크립트를 따라가세요.11:51
And instead of prioritizing this as A plus lambda B, and so what we did was by setting different values for this regularization parameter lambda, we could trade off the relative weight between how much we wanted the training set well, that is, minimizing A, versus how much we care about keeping the values of the parameter small, so that will be, the parameter is B for the support vector machine, just by convention, we're going to use a different parameter. So instead of using lambda here to control the relative waiting between the first and second terms. We're instead going to use a different parameter which by convention is called C and is set to minimize C times a + B. So for logistic regression, if we set a very large value of lambda, that means you will give B a very high weight. Here is that if we set C to be a very small value, then that responds to giving B a much larger rate than C, than A. So this is just a different way of controlling the trade off, it's just a different way of prioritizing how much we care about optimizing the first term, versus how much we care about optimizing the second term. And if you want you can think of this as the parameter C playing a role similar to 1 over lambda. And it's not that it's two equations or these two expressions will be equal. This equals 1 over lambda, that's not the case. It's rather that if C is equal to 1 over lambda, then these two optimization objectives should give you the same value the same optimal value for theta so we just filling that in I'm gonna cross out lambda here and write in the constant C there.
:13:37부터 동영상을 재생하고 스크립트를 따라가세요.13:37
So that gives us our overall optimization objective function for the support vector machine. And if you minimize that function, then what you have is the parameters learned by the SVM.
:13:51부터 동영상을 재생하고 스크립트를 따라가세요.13:51
Finally unlike logistic regression, the support vector machine doesn't output the probability is that what we have is we have this cost function, that we minimize to get the parameter's data, and what a support vector machine does is it just makes a prediction of y being equal to one or zero, directly. So the hypothesis will predict one
:14:13부터 동영상을 재생하고 스크립트를 따라가세요.14:13
if theta transpose x is greater or equal to zero, and it will predict zero otherwise and so having learned the parameters theta, this is the form of the hypothesis for the support vector machine. So that was a mathematical definition of what a support vector machine does. In the next few videos, let's try to get back to intuition about what this optimization objective leads to and whether the source of the hypotheses SVM will learn and we'll also talk about how to modify this just a little bit to the complex nonlinear functions. 한국어로 해석해줘

## ChatGPT 응답:
> In this video, the instructor starts by discussing logistic regression and how it can be modified to obtain the Support Vector Machine (SVM). Logistic regression uses the sigmoid activation function to make predictions, and the goal is to correctly classify examples. When the output of the hypothesis (h(x)) is close to 1, it means that the prediction is correct for examples with y=1. Similarly, when the output is close to 0, it indicates a correct prediction for examples with y=0.

The cost function of logistic regression is then introduced, which contributes to the overall cost based on the predicted values. The instructor explains that for y=1, the cost function takes the form of -log(h(x)), and for y=0, it becomes -log(1 - h(x)). These cost functions determine the penalty for misclassifying examples and drive the learning process.

To derive the SVM, the instructor replaces the cost functions of logistic regression with modified ones. The new cost function for y=1 in SVM has a flat portion on the right and a straight-line portion on the left, while the cost function for y=0 is a straight line that intersects the y-axis at 1. These modified cost functions provide computational advantages and lead to an easier optimization problem for SVM.

Finally, the instructor formulates the SVM optimization problem by replacing the cost functions in logistic regression with the modified cost functions for SVM. The resulting objective function is minimized with respect to theta (the parameter vector) to find the optimal decision boundary. The instructor also mentions two notational changes commonly used for SVM: removing the 1/m term and replacing the regularization parameter lambda with C, which controls the trade-off between training set accuracy and parameter regularization.

It's important to note that this is a summary of the content covered in the video based on the script you provided. Watching the video itself will provide a more detailed understanding of the concepts and equations presented.

## 사용자 입력:
> 한국어로 해석해줘

## ChatGPT 응답:
> 지금까지 다양한 지도학습 알고리즘을 살펴보았습니다. 지도학습에서 많은 알고리즘들은 성능이 상당히 유사할 것이며, 종종 어떤 학습 알고리즘을 선택하느냐보다는 데이터의 양, 알고리즘 적용 기술 등이 더 중요할 수 있습니다. 특히, 어떤 특징을 설계하여 학습 알고리즘에 제공하느냐, 그리고 컬러화 매개변수를 어떻게 선택하느냐 등이 중요합니다. 그러나 산업 및 학계에서 광범위하게 사용되는 매우 강력한 알고리즘인 서포트 벡터 머신(Support Vector Machine, SVM)이 있습니다. 로지스틱 회귀(Logistic Regression)와 신경망(Neural Networks)과 비교했을 때, SVM은 복잡한 비선형 함수를 학습하는 더 깔끔하고 강력한 방법을 제공합니다. 그래서 이번 강의에서는 SVM에 대해 상당한 시간을 할애하도록 하겠습니다. 다른 학습 알고리즘에 대한 간략한 설명도 이 과정에서 짧게 다루도록 하겠습니다. 그러나 SVM의 인기와 강력함을 고려하여 이 과정에서 주요하게 다룰 지도학습 알고리즘은 SVM이 마지막입니다. SVM에 대한 개발과정을 시작하기 전에 최적화 목적 함수에 대해 설명하겠습니다. 지금부터 로지스틱 회귀를 시작으로 SVM을 어떻게 수정하여 얻을 수 있는지 알아보겠습니다. 로지스틱 회귀에서 우리는 익숙한 가설 형식과 우측에 보여지는 시그모이드 활성화 함수를 가지고 있습니다. 이번 강의에서 설명하기 위해 z를 세타 전치와 x의 내적으로 표시하겠습니다. 로지스틱 회귀가 어떤 작업을 수행하길 원하는지 생각해봅시다. y가 1인 경우, 우리는 h(x)가 1에 가까워지길 바랍니다. 즉, 해당 예제를 올바르게 분류하기를 희망합니다. 그리고 x 아래 첨자 1이라는 것은 세타 전치 x가 0보다 크다는 것을 의미합니다. 크게 더 크다는 것

## ChatGPT 응답:
> 은 더 큰 양수를 의미하고, z가 양수이면 h(x)는 1에 가까워질 것입니다. y가 0인 경우, 우리는 h(x)가 0에 가까워지길 바랍니다. 즉, 해당 예제를 올바르게 분류하기를 희망합니다. 그리고 x 아래 첨자 1이라는 것은 세타 전치 x가 0보다 작다는 것을 의미합니다. 작게 더 작다는 것은 음수를 의미하고, z가 음수이면 h(x)는 0에 가까워질 것입니다. 로지스틱 회귀의 비용 함수를 소개하기 전에 어떤 공식이 각 케이스에 대해 유도되었는지 보여드리겠습니다. y가 1인 경우, 비용 함수는 -log(h(x))의 형태를 가지며, y가 0인 경우에는 -log(1-h(x))가 됩니다. 이 비용 함수들은 예제의 잘못된 분류에 대한 벌칙을 결정하고 학습 과정을 이끕니다. 그런데 여기서 비용 함수들을 수정하여 SVM을 얻을 수 있습니다. SVM의 경우, y가 1일 때의 비용 함수는 오른쪽에 평탄한 부분과 왼쪽에 선형 부분을 가집니다. 반면, y가 0일 때의 비용 함수는 y축과 교차점이 1인 선형 함수입니다. 이러한 수정된 비용 함수들은 계산상의 이점을 제공하며 SVM에 대한 최적화 문제를 더 쉽게 만들어줍니다. 마지막으로, SVM 최적화 문제를 서술하기 위해 로지스틱 회귀의 비용 함수들을 SVM의 수정된 비용 함수들로 대체합니다. 이렇게 되면, 최적의 결정 경계를 찾기 위해 파라미터 벡터인 세타에 대해 목적 함수를 최소화하게 됩니다. 또한, SVM에서 자주 사용되는 두 가지 표기법 변경 사항을 언급하고 있습니다. 1/m 항을 제거하고 정규화 파라미터인 람다(lambda)를 C로 바꾸는 것입니다. C는 훈련 세트 정확도와 파라미터 정규화 사이의 균형을 조절하는 역할을 합니다. 이는 제공한 스크립트를 기반으로한 동영상 내용에 대한 요약입니다. 동영상 자체를 보시면 더 자세한 개념과 방정식에 대한 이해를 얻을 수 있을 것입니다.

