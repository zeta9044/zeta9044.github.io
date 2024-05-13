---
layout: post
title: "TOC/TOU 취약점 확인"
date: 2023-12-06
tags: [취약점, TOU, TOC, 확인]
categories: blog
---

## 사용자 입력:
> 				Reqlog.info( "dest source : "+repositoryHome );

				br = new BufferedReader( new FileReader( localFileListName ) );
				while ( ( line = br.readLine() ) != null ){
					tfo = new FileInfo();
					tfo.setData( line );
					fileUtil = new FileCopyUtil();
					vPath = tfo.getSrcPath().substring(srcPath.length());
					// 2010.10.04. LeeYH. Modify...
					fileUtil.copyToFile(tfo.getSrcPath(), readEnc, StringUtil.replaceSeparator(repositoryHome+vPath), parserEnc );
//					fileUtil.copyToFile(tfo.getSrcPath(), readEnc, StringUtil.replaceSeparator(repositoryHome+vPath) );
//					System.out.println( "Copy File : "+StringUtil.replaceSeparator(repositoryHome+vPath) );
				}  이 자바코드의 while부분이 TOC 취약점이 있나?

## ChatGPT 응답:
> TOC/TOU (Time of Check to Time of Use) 취약점은 파일이나 데이터가 검증된 후와 사용될 때 사이에 변경되어 보안 문제가 발생하는 상황을 말합니다. 제공된 Java 코드를 분석하면, `while` 루프 내에서 파일을 복사하는 과정이 포함되어 있습니다. 이 코드는 파일의 정보를 읽어서 다른 위치에 복사하는 작업을 수행합니다.

TOC/TOU 취약점이 발생하려면, 파일이 검증된 후와 사용될 때 사이에 외부에서 변경될 수 있는 여지가 있어야 합니다. 이 코드에서는 `FileInfo` 객체 (`tfo`)를 생성하고, `setData()` 메서드를 통해 데이터를 설정한 후, `FileCopyUtil` 객체 (`fileUtil`)를 사용하여 파일을 복사합니다.

코드에서 직접적으로 TOC/TOU 취약점이 발생할 수 있는 부분은 명확하게 보이지 않습니다. 그러나, 이것은 전체 애플리케이션의 컨텍스트나 다른 부분의 코드가 어떻게 작성되었는지에 따라 달라질 수 있습니다. 예를 들어, `FileInfo` 객체가 파일의 메타데이터만 다루고 실제 파일 콘텐츠에 접근하지 않는다면, 파일 콘텐츠가 `setData()` 호출 후 변경되었을 가능성에 대한 검사가 없다면 TOC/TOU 취약점이 발생할 수 있습니다.

따라서, 코드 자체만으로는 TOC/TOU 취약점의 존재 여부를 명확히 판단하기 어렵습니다. 전체 애플리케이션의 컨텍스트와 관련된 다른 코드 부분들을 함께 검토할 필요가 있습니다.

