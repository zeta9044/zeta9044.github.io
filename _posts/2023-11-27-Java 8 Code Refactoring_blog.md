---
layout: post
title: "Java 8 Code Refactoring"
date: 2023-11-27
tags: [Code, Java, Refactoring]
categories: blog
---

## 사용자 입력:
>     private static String DecryptSecurity(String gubun) throws Exception
    {
    	String sRtn = "";
    	
    	//system에서 home 경로를 가져온다 
		String systemHome = System.getProperty("ais.home");
    	// 암호화 파일에서 읽기. 
    	File sf = new File(systemHome + "/config/security.properties");

    	if ( sf.exists())
    	{
        	BufferedReader bfr = new BufferedReader(new FileReader(sf));
        	try
        	{
        		String temp = bfr.readLine().trim();
        		int tempIndex = temp.indexOf("\\|");
        		String tempuser     = "";
        		String temppassword = "";
        		if ( tempIndex == -1)
        		{
        			// 2023.03.02. LeeYH. Add...
        			// 한번더 암호화한 상태 처리.
        			String tmpIDPW = DecryptFile.decryptStr(temp);
        			tempIndex      = tmpIDPW.indexOf("\\|");
            		temp           = tmpIDPW;
        		}
        		tempuser     = temp.substring(0, tempIndex);
        		temppassword = temp.substring(tempIndex+2);
        		
        		// 2015.01.21. LeeYH. Modify... -- BEGIN
        		// MetaStream에서 사용하는 library를 사용한 로직.
        		// INGLife는 내부적인 다른 로직을 사용.
        		if ( gubun.equals("ID") )
        		{
        			sRtn = DecryptFile.decryptStr(tempuser);
        		}
        		else if ( gubun.equals("PW") )
        		{
        			sRtn = DecryptFile.decryptStr(temppassword);
        		}
        		// 2015.01.21. LeeYH. Modify... -- END
        	}
        	catch (Exception e )
        	{
        		System.out.println("Security File Read Fail :" + e.getMessage());
        	}
        	finally
        	{
        		if( sf != null ) sf = null;
        		if( bfr != null) bfr.close();
        	}
    	}
    	else
    	{
    		System.out.println("Not exist Security File . . .");
    	}
    	
    	return sRtn;
    } 이 자바함수를 java8에서 동작할 수 있게 하고, try-with-resources문을 적용해줘.

## ChatGPT 응답:
> To make the provided Java function compatible with Java 8 and apply the `try-with-resources` statement, you need to modify it slightly. The `try-with-resources` statement ensures that each resource is closed at the end of the statement, which makes it easier to manage resources and prevents potential memory leaks. Here's how you can refactor the code:

```java
private static String DecryptSecurity(String gubun) throws Exception {
    String sRtn = "";

    // Get system home path
    String systemHome = System.getProperty("ais.home");
    
    // Define the security properties file
    File sf = new File(systemHome + "/config/security.properties");

    if (sf.exists()) {
        // Use try-with-resources for automatic resource management
        try (BufferedReader bfr = new BufferedReader(new FileReader(sf))) {
            String temp = bfr.readLine().trim();
            int tempIndex = temp.indexOf("\\|");
            String tempuser = "";
            String temppassword = "";

            if (tempIndex == -1) {
                // Handle encrypted state
                String tmpIDPW = DecryptFile.decryptStr(temp);
                tempIndex = tmpIDPW.indexOf("\\|");
                temp = tmpIDPW;
            }

            tempuser = temp.substring(0, tempIndex);
            temppassword = temp.substring(tempIndex + 2);

            // Decrypt based on 'gubun' value
            if ("ID".equals(gubun)) {
                sRtn = DecryptFile.decryptStr(tempuser);
            } else if ("PW".equals(gubun)) {
                sRtn = DecryptFile.decryptStr(temppassword);
            }
        } catch (Exception e) {
            System.out.println("Security File Read Fail: " + e.getMessage());
        }
    } else {
        System.out.println("Not exist Security File . . .");
    }

    return sRtn;
}
```

Changes made:
1. **Try-with-Resources:** The `BufferedReader` is now declared within a try-with-resources statement, ensuring it is closed automatically.
2. **Code Formatting:** The code is formatted for better readability.
3. **Exception Handling:** The catch block remains outside the try-with-resources block to handle any exceptions that might occur during file reading.

This updated code should work as expected in Java 8 while ensuring better resource management with the try-with-resources statement.

## 사용자 입력:
> 이 부분이  TOCTOU_RACE_CONDITION  취약점이 나왔는데, 바꿔봐. 자바8로

## ChatGPT 응답:
> The Time-Of-Check to Time-Of-Use (TOCTOU) race condition vulnerability you're encountering in the Java code is due to the check for the file's existence (`sf.exists()`) before actually opening it for reading. Between the check and the use, there's a window where the file's state could change due to other processes, leading to potential security issues.

To mitigate this vulnerability in Java 8, you can directly attempt to open the file for reading without checking its existence first. If the file does not exist or cannot be opened, an exception will be thrown and can be handled appropriately. Here's the revised code:

```java
private static String DecryptSecurity(String gubun) throws Exception {
    String sRtn = "";

    // Get system home path
    String systemHome = System.getProperty("ais.home");

    // Define the security properties file path
    Path securityFilePath = Paths.get(systemHome, "config", "security.properties");

    try (BufferedReader bfr = Files.newBufferedReader(securityFilePath)) {
        String temp = bfr.readLine().trim();
        int tempIndex = temp.indexOf("\\|");
        String tempuser = "";
        String temppassword = "";

        if (tempIndex == -1) {
            // Handle encrypted state
            String tmpIDPW = DecryptFile.decryptStr(temp);
            tempIndex = tmpIDPW.indexOf("\\|");
            temp = tmpIDPW;
        }

        tempuser = temp.substring(0, tempIndex);
        temppassword = temp.substring(tempIndex + 2);

        // Decrypt based on 'gubun' value
        if ("ID".equals(gubun)) {
            sRtn = DecryptFile.decryptStr(tempuser);
        } else if ("PW".equals(gubun)) {
            sRtn = DecryptFile.decryptStr(temppassword);
        }
    } catch (NoSuchFileException e) {
        System.out.println("Security File Not Found: " + e.getMessage());
    } catch (IOException e) {
        System.out.println("Error Reading Security File: " + e.getMessage());
    }

    return sRtn;
}
```

Changes:
1. **Removed `File.exists` Check:** The code now directly tries to open the file without checking if it exists.
2. **Exception Handling:** `NoSuchFileException` is specifically caught to handle the case where the file does not exist. `IOException` handles other I/O errors.
3. **Using `Paths` and `Files` API:** This is more idiomatic in Java 8 for dealing with file paths and reading files.

This approach ensures that the operation is safer against TOCTOU race conditions.

## 사용자 입력:
> 한글로

## ChatGPT 응답:
> Java 8에서 Time-Of-Check to Time-Of-Use (TOCTOU) 레이스 컨디션 취약점을 해결하려면, 파일이 존재하는지 확인하는 부분(`sf.exists()`)을 제거하고 파일을 직접 열어보는 방식으로 변경해야 합니다. 존재 확인과 사용 사이에 다른 프로세스에 의해 파일 상태가 변경될 수 있는 창을 줄일 수 있습니다.

다음은 수정된 코드입니다:

```java
private static String DecryptSecurity(String gubun) throws Exception {
    String sRtn = "";

    // 시스템 홈 경로 가져오기
    String systemHome = System.getProperty("ais.home");

    // 보안 프로퍼티 파일 경로 정의
    Path securityFilePath = Paths.get(systemHome, "config", "security.properties");

    try (BufferedReader bfr = Files.newBufferedReader(securityFilePath)) {
        String temp = bfr.readLine().trim();
        int tempIndex = temp.indexOf("\\|");
        String tempuser = "";
        String temppassword = "";

        if (tempIndex == -1) {
            // 암호화 상태 처리
            String tmpIDPW = DecryptFile.decryptStr(temp);
            tempIndex = tmpIDPW.indexOf("\\|");
            temp = tmpIDPW;
        }

        tempuser = temp.substring(0, tempIndex);
        temppassword = temp.substring(tempIndex + 2);

        // 'gubun' 값에 따라 복호화
        if ("ID".equals(gubun)) {
            sRtn = DecryptFile.decryptStr(tempuser);
        } else if ("PW".equals(gubun)) {
            sRtn = DecryptFile.decryptStr(temppassword);
        }
    } catch (NoSuchFileException e) {
        System.out.println("보안 파일을 찾을 수 없습니다: " + e.getMessage());
    } catch (IOException e) {
        System.out.println("보안 파일 읽기 오류: " + e.getMessage());
    }

    return sRtn;
}
```

변경 사항:
1. **`File.exists` 확인 제거:** 코드는 이제 파일이 존재하는지 확인하지 않고 직접 파일을 엽니다.
2. **예외 처리:** `NoSuchFileException`은 파일이 존재하지 않을 경우를 처리하기 위해 특별히 캐치됩니다. `IOException`은 다른 I/O 오류를 처리합니다.
3. **`Paths`와 `Files` API 사용:** Java 8에서 파일 경로 처리와 파일 읽기에 더 적합합니다.

이 방법은 TOCTOU 레이스 컨디션에 대한 안전성을 높입니다.

## 사용자 입력:
> 여기서 securityFilePath 가 null 이 될 경우는 없나?

## ChatGPT 응답:
> `securityFilePath`가 `null`이 되는 경우는 일반적으로 없습니다. `Paths.get()` 메서드는 주어진 경로 문자열들을 결합하여 `Path` 객체를 생성합니다. 이 메서드는 유효하지 않은 경로를 제공받거나 경로 생성 중 문제가 발생했을 때 `InvalidPathException`을 발생시킵니다.

`systemHome`이 `null`이거나 유효하지 않은 값일 때, `Paths.get(systemHome, "config", "security.properties")`는 `InvalidPathException`을 발생시킬 수 있습니다. 그러나 `null` 자체로는 `Path` 객체가 생성되지 않으므로, `securityFilePath`가 `null`이 되는 상황은 발생하지 않습니다.

예외적인 상황을 고려하여 `systemHome` 값이 `null`이거나 공백인 경우를 처리하는 로직을 추가할 수 있습니다. 이렇게 하면, `systemHome`에 대한 유효성 검사를 통해 더 견고한 코드를 작성할 수 있습니다. 예를 들면:

```java
private static String DecryptSecurity(String gubun) throws Exception {
    String sRtn = "";

    // 시스템 홈 경로 가져오기
    String systemHome = System.getProperty("ais.home");
    if (systemHome == null || systemHome.trim().isEmpty()) {
        throw new IllegalStateException("시스템 홈 경로가 설정되지 않았습니다.");
    }

    // 보안 프로퍼티 파일 경로 정의
    Path securityFilePath = Paths.get(systemHome, "config", "security.properties");
    // 나머지 코드...
}
```

이렇게 하면 `systemHome` 값이 `null`이거나 공백일 때 적절한 예외 처리를 할 수 있으며, `securityFilePath`가 유효하지 않은 상태로 생성되는 것을 방지할 수 있습니다.

