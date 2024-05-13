---
layout: post
title: "TOCTOU 취약점 해설"
date: 2023-11-21
tags: [TOCTOU, 취약점, 해설]
categories: blog
---

## 사용자 입력:
> 		if(files[i].isDirectory()) {
			DeleteDirectory(files[i]);
		} else {
			files[i].delete();
		}
	}

자바코드 일부인데, TOCTOU_RACE_CONDITION 취약점이 있다고 하는데, 설명 좀 해줘.

## ChatGPT 응답:
> 이 자바 코드 조각에서 TOCTOU (Time of Check to Time of Use) 경쟁 조건 취약점이 발생할 수 있습니다. TOCTOU 취약점은 파일 또는 리소스의 상태가 검사 시점과 사용 시점 사이에 변경될 수 있는 상황에서 발생합니다. 이러한 상황은 보안 문제를 일으킬 수 있습니다.

이 코드에서는 `files[i].isDirectory()`와 `files[i].delete()` 호출 사이에 파일 시스템의 상태가 변경될 수 있습니다. 예를 들어, 다음과 같은 시나리오가 발생할 수 있습니다:

1. `isDirectory()` 메소드를 호출하여 파일이 디렉토리인지 확인합니다.
2. 다른 프로세스 또는 스레드가 해당 파일을 수정하거나 상태를 변경합니다. 예를 들어, 디렉토리를 파일로 바꾸거나 권한을 변경할 수 있습니다.
3. `delete()` 메소드가 호출되어 파일 또는 디렉토리를 삭제합니다.

이러한 상황에서, `delete()` 호출은 `isDirectory()` 호출 시점에서의 파일 상태가 아닌, 변경된 상태에 대해 수행됩니다. 이는 보안 문제를 일으킬 수 있으며, 특히 권한이나 파일 유형의 변경이 포함된 경우 위험할 수 있습니다.

이러한 취약점을 방지하기 위해서는 상태 검사와 사용을 원자적으로(즉, 분리할 수 없는 하나의 작업으로) 처리하는 것이 중요합니다. Java에서는 이를 위해 파일 시스템의 원자적 작업을 지원하는 API를 사용하거나, 다른 방법으로 상태 변경을 방지하는 조치를 취해야 할 수 있습니다.

## 사용자 입력:
> 위 코드를 해결해서 보여줘.

## ChatGPT 응답:
> 위의 코드에 있는 TOCTOU 취약점을 해결하기 위해서는 파일이나 디렉토리의 상태 검사와 사용을 원자적으로 처리하는 것이 중요합니다. 하지만 Java 표준 라이브러리에는 이러한 동작을 직접 지원하는 API가 없습니다. 대신, 예외 처리를 통해 파일이나 디렉토리를 안전하게 삭제하는 방법을 사용할 수 있습니다.

아래는 기존 코드를 수정한 예제입니다:

```java
for (int i = 0; i < files.length; i++) {
    try {
        if (files[i].isDirectory()) {
            DeleteDirectory(files[i]);
        } else {
            files[i].delete();
        }
    } catch (SecurityException e) {
        // 적절한 예외 처리 (예: 로그 기록, 사용자에게 알림 등)
    }
}
```

이 예제에서는 `SecurityException`을 캐치하여 파일 또는 디렉토리의 상태가 변경되어 발생할 수 있는 보안 문제를 처리합니다. 이렇게 하면 파일 상태가 변경되었을 때 예외가 발생하고, 이를 통해 적절한 처리를 할 수 있습니다.

또한, 재귀적으로 디렉토리를 삭제하는 `DeleteDirectory` 메소드의 구현도 중요합니다. 이 메소드는 디렉토리 내의 모든 파일과 서브디렉토리를 안전하게 삭제해야 합니다. 그러나 정확한 구현은 여러분의 애플리케이션 요구 사항과 환경에 따라 달라질 수 있습니다.

중요한 것은 파일 시스템의 상태가 변경될 수 있는 가능성을 항상 염두에 두고, 이에 대한 예외 처리를 통해 안전하게 코드를 작성하는 것입니다.

## 사용자 입력:
> 직접 지원하는 라이브러리 사용해서 해봐.

## ChatGPT 응답:
> Java의 `java.nio.file` 패키지는 파일 시스템 작업에 대해 보다 정교한 제어를 제공합니다. 이를 이용해 TOCTOU 취약점을 해결할 수 있습니다. `Files` 클래스의 메소드를 사용하여 파일이나 디렉토리를 안전하게 삭제하는 방법을 살펴보겠습니다.

아래는 `java.nio.file`을 사용한 코드 예제입니다:

```java
import java.nio.file.*;
import java.io.IOException;

public class SafeFileDeletion {
    public static void deleteFileOrDirectory(Path path) throws IOException {
        if (Files.isDirectory(path, LinkOption.NOFOLLOW_LINKS)) {
            try (DirectoryStream<Path> entries = Files.newDirectoryStream(path)) {
                for (Path entry : entries) {
                    deleteFileOrDirectory(entry);
                }
            }
            Files.delete(path);
        } else {
            Files.deleteIfExists(path);
        }
    }

    public static void main(String[] args) {
        // 예를 들어, 삭제할 경로를 지정합니다.
        Path pathToDelete = Paths.get("some/path/to/delete");
        try {
            deleteFileOrDirectory(pathToDelete);
        } catch (IOException e) {
            e.printStackTrace();
            // 적절한 예외 처리
        }
    }
}
```

이 코드에서는 다음과 같은 방식으로 작업을 처리합니다:

- `Files.isDirectory` 메소드를 사용하여 주어진 경로가 디렉토리인지 확인합니다. `LinkOption.NOFOLLOW_LINKS` 옵션은 심볼릭 링크를 따르지 않도록 합니다.
- 디렉토리인 경우, `Files.newDirectoryStream`을 사용하여 디렉토리 내의 모든 항목을 반복 처리하고, 각 항목에 대해 재귀적으로 `deleteFileOrDirectory` 메소드를 호출합니다.
- 마지막으로, `Files.delete` 또는 `Files.deleteIfExists` 메소드를 사용하여 파일이나 디렉토리를 삭제합니다. 이 메소드들은 원자적으로 파일이나 디렉토리를 삭제하며, 파일 상태가 변경되었을 경우 적절한 예외를 발생시킵니다.

이 방식은 `java.io.File`을 사용하는 것보다 더 안전하고 유연하게 파일 시스템 작업을 처리할 수 있습니다.

## 사용자 입력:
> 	public void DeleteDirectory(File file) {
		if(file.exists()) {
			File[] files =file.listFiles();

			for(int i=0; i<files.length; i++) {
				if(files[i].isDirectory()) {
					DeleteDirectory(files[i]);
				} else {
					files[i].delete();
				}
			}

			file.delete();
		} else {
			return;
		}
	} 이 함수의 취약점을 없애봐. 

## ChatGPT 응답:
> 이 `DeleteDirectory` 함수는 재귀적으로 디렉토리를 삭제하는 방법을 사용하고 있습니다. 그러나 이 함수는 여전히 TOCTOU 취약점을 포함하고 있으며, 이를 `java.nio.file` 패키지를 사용하여 해결할 수 있습니다.

아래는 `java.nio.file`을 이용해 `DeleteDirectory` 함수를 수정한 예제입니다:

```java
import java.nio.file.*;
import java.io.IOException;

public class DirectoryDeleter {

    public static void deleteDirectory(Path directory) throws IOException {
        if (Files.exists(directory, LinkOption.NOFOLLOW_LINKS)) {
            try (DirectoryStream<Path> stream = Files.newDirectoryStream(directory)) {
                for (Path entry : stream) {
                    if (Files.isDirectory(entry, LinkOption.NOFOLLOW_LINKS)) {
                        deleteDirectory(entry);
                    } else {
                        Files.delete(entry);
                    }
                }
            }
            Files.delete(directory);
        }
    }

    public static void main(String[] args) {
        // 예를 들어, 삭제할 디렉토리 경로를 지정합니다.
        Path pathToDelete = Paths.get("path/to/directory");
        try {
            deleteDirectory(pathToDelete);
        } catch (IOException e) {
            e.printStackTrace();
            // 적절한 예외 처리
        }
    }
}
```

이 코드에서 `deleteDirectory` 메소드는 다음과 같이 작동합니다:

1. `Files.exists`를 사용하여 디렉토리의 존재 여부를 확인합니다. `LinkOption.NOFOLLOW_LINKS`는 심볼릭 링크를 따르지 않도록 합니다.
2. 디렉토리 스트림(`DirectoryStream`)을 사용하여 디렉토리 내의 모든 파일과 하위 디렉토리를 반복 처리합니다.
3. 각 항목에 대해, `Files.isDirectory`를 사용하여 디렉토리인지 확인하고, 디렉토리인 경우 재귀적으로 `deleteDirectory`를 호출합니다. 파일인 경우, `Files.delete`를 사용하여 파일을 삭제합니다.
4. 모든 하위 파일과 디렉토리가 삭제된 후, `Files.delete`를 사용하여 상위 디렉토리를 삭제합니다.

이 방법을 사용하면 TOCTOU 취약점을 해결할 수 있으며, 파일 시스템의 상태 변경에 대응하는 더 안정적이고 견고한 코드를 작성할 수 있습니다.

## 사용자 입력:
> LinkOption.NOFOLLOW_LINKS 이건 왜 필요한 거야?

## ChatGPT 응답:
> `LinkOption.NOFOLLOW_LINKS` 옵션은 Java의 `java.nio.file` 패키지에서 파일이나 디렉토리와 관련된 작업을 할 때 심볼릭 링크를 따르지 않도록 지정하는 데 사용됩니다. 이 옵션의 주요 목적은 보안과 관련된 이유 때문입니다.

심볼릭 링크(Symbolic Link)는 한 파일 또는 디렉토리를 가리키는 또 다른 경로입니다. 이러한 링크는 파일 시스템에서 다른 위치에 있는 파일이나 디렉토리처럼 보이게 할 수 있습니다. 

`LinkOption.NOFOLLOW_LINKS` 옵션을 사용하는 이유는 다음과 같습니다:

1. **보안**: 심볼릭 링크를 통해 의도치 않은 파일이나 디렉토리에 접근할 수 있습니다. 예를 들어, 애플리케이션은 특정 디렉토리의 파일만을 처리하려고 할 때, 심볼릭 링크를 통해 민감한 시스템 파일이나 다른 사용자의 파일에 접근할 수 있는 위험이 있습니다. `NOFOLLOW_LINKS` 옵션을 사용하면 이러한 심볼릭 링크를 따르지 않아, 이러한 보안 위험을 줄일 수 있습니다.

2. **예측 가능성과 정확성**: 파일이나 디렉토리의 실제 경로를 정확하게 알아야 하는 경우가 많습니다. 심볼릭 링크를 따르게 되면, 실제로는 다른 위치의 파일이나 디렉토리에 작업을 수행하게 될 수 있습니다. `NOFOLLOW_LINKS`를 사용하면 이러한 혼란을 방지하고, 작업이 수행되는 실제 경로에 대한 확신을 가질 수 있습니다.

따라서, `LinkOption.NOFOLLOW_LINKS`는 심볼릭 링크를 통한 예기치 않은 동작이나 보안 문제를 예방하는 데 도움이 됩니다. 이는 특히 보안이 중요한 애플리케이션에서 파일이나 디렉토리를 안전하게 처리하기 위해 중요한 옵션입니다.

## 사용자 입력:
> LinkOption.NOFOLLOW_LINKS 이 걸 하면, 심볼릭링크 파일은 제외하는 건가?

## ChatGPT 응답:
> 네, 맞습니다. `LinkOption.NOFOLLOW_LINKS`를 사용하면 Java 파일 API는 심볼릭 링크를 따르지 않게 됩니다. 이는 심볼릭 링크가 가리키는 실제 파일이나 디렉토리 대신 심볼릭 링크 자체에 대한 작업을 수행한다는 의미입니다.

예를 들어, `Files.isDirectory(path, LinkOption.NOFOLLOW_LINKS)`를 호출할 때, 주어진 `path`가 심볼릭 링크이고 그 링크가 디렉토리를 가리키고 있더라도, `NOFOLLOW_LINKS` 옵션은 `isDirectory` 메소드가 심볼릭 링크 자체의 속성을 검사하도록 합니다. 만약 그 링크 자체가 디렉토리 형식의 링크가 아니라면 (즉, 심볼릭 링크 파일 자체가 디렉토리가 아니라면), `isDirectory`는 `false`를 반환합니다.

이러한 방식으로, `NOFOLLOW_LINKS` 옵션은 심볼릭 링크를 통한 간접적인 파일 접근을 방지하고, 파일 시스템에서의 보안을 강화하는 데 도움이 됩니다. 심볼릭 링크가 가리키는 대상이 아닌 링크 자체에만 작업을 적용하려는 경우에 이 옵션을 사용하면 됩니다.

## 사용자 입력:
> 예를 들어,    루트 아래 /a가  있는데, 이걸   심볼릭링크로  /b로 만들면, /b를 지워버린다는 건가?

## ChatGPT 응답:
> 네, 맞습니다. 예를 들어, `/a`가 실제 디렉토리나 파일이고, `/b`가 `/a`를 가리키는 심볼릭 링크인 경우, `LinkOption.NOFOLLOW_LINKS`를 사용하여 `/b`를 삭제하면 심볼릭 링크인 `/b` 자체만 삭제됩니다. 심볼릭 링크가 가리키는 실제 대상인 `/a`는 영향을 받지 않습니다.

즉, 이 옵션을 사용할 때 `Files.delete(Path path)`와 같은 메소드로 `/b`를 삭제하면, `/b`라는 심볼릭 링크 파일만 제거되며, 이 링크가 가리키는 원본 대상 `/a`는 그대로 유지됩니다. 이는 심볼릭 링크를 사용하여 간접적으로 다른 파일이나 디렉토리에 접근하는 것을 방지하고자 할 때 유용합니다.

## 사용자 입력:
> import java.nio.file.*;
import java.io.IOException;

public class DirectoryDeleter {

    public static void deleteDirectory(Path directory) throws IOException {
        if (Files.exists(directory, LinkOption.NOFOLLOW_LINKS)) {
            try (DirectoryStream<Path> stream = Files.newDirectoryStream(directory)) {
                for (Path entry : stream) {
                    if (Files.isDirectory(entry, LinkOption.NOFOLLOW_LINKS)) {
                        deleteDirectory(entry);
                    } else {
                        Files.delete(entry);
                    }
                }
            }
            Files.delete(directory);
        }
    }

    public static void main(String[] args) {
        // 예를 들어, 삭제할 디렉토리 경로를 지정합니다.
        Path pathToDelete = Paths.get("path/to/directory");
        try {
            deleteDirectory(pathToDelete);
        } catch (IOException e) {
            e.printStackTrace();
            // 적절한 예외 처리
        }
    }
}
함수에서 Path가 매개변수인데, String Path로 주어질때도 처리하는 함수 만들어줘.

## ChatGPT 응답:
> 매개변수로 `String` 형태의 경로가 주어졌을 때도 처리할 수 있도록 `deleteDirectory` 함수를 오버로드(overload)하는 방법을 사용할 수 있습니다. 오버로딩은 동일한 이름의 메소드에 다른 매개변수 목록을 제공하는 기법입니다. 이 경우, `String`을 받아 `Path` 객체로 변환한 후 기존의 `deleteDirectory(Path directory)` 메소드를 호출할 수 있습니다.

아래는 `String` 매개변수를 받는 `deleteDirectory` 함수의 오버로드 버전입니다:

```java
import java.nio.file.*;
import java.io.IOException;

public class DirectoryDeleter {

    // Path 객체를 매개변수로 받는 기존 메소드
    public static void deleteDirectory(Path directory) throws IOException {
        if (Files.exists(directory, LinkOption.NOFOLLOW_LINKS)) {
            try (DirectoryStream<Path> stream = Files.newDirectoryStream(directory)) {
                for (Path entry : stream) {
                    if (Files.isDirectory(entry, LinkOption.NOFOLLOW_LINKS)) {
                        deleteDirectory(entry);
                    } else {
                        Files.delete(entry);
                    }
                }
            }
            Files.delete(directory);
        }
    }

    // String 매개변수를 받는 오버로드된 메소드
    public static void deleteDirectory(String directoryPath) throws IOException {
        deleteDirectory(Paths.get(directoryPath));
    }

    public static void main(String[] args) {
        // 예를 들어, 삭제할 디렉토리 경로를 지정합니다 (String 형태).
        String pathToDelete = "path/to/directory";
        try {
            deleteDirectory(pathToDelete);
        } catch (IOException e) {
            e.printStackTrace();
            // 적절한 예외 처리
        }
    }
}
```

이 코드에서 `deleteDirectory(String directoryPath)` 메소드는 `String` 형태의 경로를 받아 `Paths.get(directoryPath)`를 사용하여 `Path` 객체로 변환한 후, 기존의 `deleteDirectory(Path directory)` 메소드를 호출합니다. 이렇게 하면 `String` 형태의 경로로도 디렉토리를 삭제할 수 있게 됩니다.

