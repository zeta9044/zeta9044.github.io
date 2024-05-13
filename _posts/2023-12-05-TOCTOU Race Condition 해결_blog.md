---
layout: post
title: "TOCTOU Race Condition 해결"
date: 2023-12-05
tags: [TOCTOU, Condition, 해결, Race]
categories: blog
---

## 사용자 입력:
> 			File csf = new File(systemHome + "/config/security.properties");
			try
			{
				if ( csf.exists() )
				{
					BufferedReader bfr = new BufferedReader(new FileReader(csf));
					String temp = bfr.readLine().trim();
					int tempIndex = temp.indexOf("\\|");
	        		String tempuser     = "";
	        		String temppassword = "";
	        		if ( tempIndex == -1)
	        		{
	        			// 2023.03.02. LeeYH. Add...
	        			// 한번더 암호화한 상태 처리.
	        			String tmpIDPW = DecryptFile.decrypt(temp);
	        			tempIndex      = tmpIDPW.indexOf("\\|");
	            		temp           = tmpIDPW;
	        		}
	        		tempuser     = temp.substring(0, tempIndex);
	        		temppassword = temp.substring(tempIndex+2);
					if (!tempuser.isEmpty() && !temppassword.isEmpty())
					{
						bReturn = true;
					}
				}
			}
			catch (Exception e)
			{
				bReturn = false;
				System.out.println("Security File Read Fail :" +  e.getMessage());
				logger.debug(e);
			}
			finally
			{
				if ( csf != null ) csf = null;
			}
위 자바코드에서 TOCTOU_RACE_CONDITION을 없애는 코드로 바꿔줘.

## ChatGPT 응답:
> 이 자바 코드는 TOCTOU (Time-of-check to time-of-use) 경쟁 조건의 문제를 가지고 있습니다. 이러한 문제는 파일이나 리소스의 상태를 확인한 후 그 상태가 사용될 때까지 변경될 수 있기 때문에 발생합니다. 이 코드에서는 `csf.exists()`를 통해 파일이 존재하는지 확인한 후에 파일을 읽고 있습니다. 파일이 존재 확인과 파일 읽기 사이에 파일 상태가 변경될 수 있으므로, 이것은 TOCTOU 경쟁 조건을 초래합니다.

TOCTOU 경쟁 조건을 방지하기 위해, 파일 존재 여부를 별도로 확인하는 대신, 파일을 직접 열고 그 결과를 바탕으로 처리를 진행하는 것이 좋습니다. 파일이 존재하지 않는 경우, `FileReader` 또는 `BufferedReader` 생성시 예외가 발생하며, 이를 적절하게 처리할 수 있습니다.

다음은 개선된 코드 예시입니다:

```java
File csf = new File(systemHome + "/config/security.properties");
BufferedReader bfr = null;

try {
    bfr = new BufferedReader(new FileReader(csf));
    String temp = bfr.readLine().trim();
    int tempIndex = temp.indexOf("\\|");
    String tempuser = "";
    String temppassword = "";

    if (tempIndex == -1) {
        // 한번 더 암호화한 상태 처리
        String tmpIDPW = DecryptFile.decrypt(temp);
        tempIndex = tmpIDPW.indexOf("\\|");
        temp = tmpIDPW;
    }

    tempuser = temp.substring(0, tempIndex);
    temppassword = temp.substring(tempIndex + 2);

    if (!tempuser.isEmpty() && !temppassword.isEmpty()) {
        bReturn = true;
    }
} catch (FileNotFoundException e) {
    bReturn = false;
    System.out.println("Security File Not Found :" + e.getMessage());
    logger.debug(e);
} catch (Exception e) {
    bReturn = false;
    System.out.println("Security File Read Fail :" + e.getMessage());
    logger.debug(e);
} finally {
    if (bfr != null) {
        try {
            bfr.close();
        } catch (IOException e) {
            logger.debug(e);
        }
    }
}
```

이 코드에서는 파일이 존재하지 않는 경우에 `FileNotFoundException`이 발생하며, 이를 통해 파일 존재 여부를 확인합니다. 또한 `finally` 블록을 사용하여 `BufferedReader`를 안전하게 닫을 수 있도록 합니다. 이런 방식으로 TOCTOU 경쟁 조건을 방지할 수 있습니다.

## 사용자 입력:
> 위 코드에서 file은 close 안해도 되나?

## ChatGPT 응답:
> 코드에서 사용된 `File` 객체 `csf`는 실제로 파일을 열거나 닫는 데 사용되지 않습니다. Java에서 `File` 클래스의 인스턴스는 파일 시스템의 파일이나 디렉토리의 경로를 추상화합니다. 이 클래스는 파일을 열거나 읽고 쓰는 기능을 제공하지 않습니다. 대신, 파일 콘텐츠에 접근하려면 `FileReader`, `BufferedReader`, `FileInputStream` 등과 같은 다른 클래스를 사용해야 합니다.

위 코드에서는 `BufferedReader` 객체 `bfr`를 사용하여 실제 파일 내용을 읽고 있습니다. 따라서 `bfr` 객체를 사용한 후에는 이 객체를 닫아야 합니다. `FileReader` 객체도 내부적으로 파일을 열기 때문에 닫는 것이 중요합니다. 하지만 `BufferedReader`를 닫으면 그 안에 있는 `FileReader`도 자동으로 닫히기 때문에 별도로 `FileReader`를 닫을 필요는 없습니다.

따라서 위 코드에서는 `File` 객체 `csf`를 닫을 필요가 없지만, `BufferedReader` 객체 `bfr`는 `finally` 블록 안에서 닫아야 합니다. 이미 이를 코드에 반영하고 있습니다:

```java
finally {
    if (bfr != null) {
        try {
            bfr.close();
        } catch (IOException e) {
            logger.debug(e);
        }
    }
}
```

이렇게 하면 파일을 안전하게 읽은 후 자원을 적절히 해제할 수 있습니다.

