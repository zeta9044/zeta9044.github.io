---
layout: post
title: "TOC/TOU 취약점 해결"
tags: [TOU, TOC, 해결, 취약점]
---

## 사용자 입력:
> 이전에 네가 다음 코드에서 TOC 취약점을 없앤다고, 	File f = new File(ctx.get(Context.CTX_REPOSITORYS) + "/" + prjId);
		if ( !f.exists() ) f.mkdirs();
		else {
			f.delete();
			f.mkdirs();
		}  이것을 Path directoryPath = Paths.get(getRepositoryhome(), prjId);

		// 디렉토리 내용 삭제 및 재생성
		if (Files.exists(directoryPath)) {
			Files.walk(directoryPath)
					.sorted(Comparator.reverseOrder())
					.map(Path::toFile)
					.forEach(File::delete);
		}
		Files.createDirectories(directoryPath);  이렇게 바꾸었어. 그런데, 원래 비지니스 목적은 디렉토리가 없으면 만들고, 그리고, 디렉토리 안에 파일 없으면 디렉토리를 지우고 새로 디렉토리 를 만드는 거였어.

## ChatGPT 응답:
> 이전에 제시한 코드는 Time of Check to Time of Use (TOC/TOU) 취약점을 방지하기 위한 것이었습니다. 이 취약점은 파일이나 디렉토리의 상태를 확인한 시점과 이를 사용하는 시점 사이에 발생할 수 있는 보안 문제를 말합니다. 예를 들어, 파일이 존재하는지 확인한 후 실제로 파일을 사용할 때까지의 시간 동안 파일의 상태가 변경되면 보안 문제가 발생할 수 있습니다.

원래 코드는 {% raw %}`File`{% endraw %} 클래스를 사용하여 디렉토리의 존재 여부를 확인하고, 디렉토리가 존재하지 않으면 생성하거나, 존재할 경우 삭제한 후 다시 생성하는 로직이었습니다. 하지만 이 접근 방식은 TOC/TOU 취약점에 노출될 수 있습니다.

수정한 코드는 {% raw %}`Path`{% endraw %}와 {% raw %}`Files`{% endraw %} 클래스를 사용하여 동일한 기능을 수행하지만, 보안 측면에서 더 안전합니다. 이 코드는 디렉토리의 존재 여부를 확인하고, 존재할 경우 디렉토리 내의 모든 파일을 삭제한 다음 디렉토리를 재생성합니다.

하지만, 여러분의 비즈니스 요구사항에 따르면, 디렉토리가 없으면 생성하고, 디렉토리 내에 파일이 없을 경우에만 디렉토리를 삭제하고 재생성하는 것이 목적이었습니다. 수정한 코드는 이 요구사항을 충족하지 않습니다. 디렉토리 내에 파일이 있는지 여부를 확인하고, 파일이 없을 때만 디렉토리를 삭제하고 재생성하는 로직을 추가해야 할 것 같습니다.

다음은 이 요구사항을 충족하는 코드 예시입니다:

{% raw %}```java
Path directoryPath = Paths.get(getRepositoryhome(), prjId);

// 디렉토리가 존재하지 않으면 생성
if (!Files.exists(directoryPath)) {
    Files.createDirectories(directoryPath);
} else {
    // 디렉토리 내에 파일이 없는 경우에만 디렉토리 삭제 및 재생성
    try (Stream<Path> files = Files.list(directoryPath)) {
        if (!files.findAny().isPresent()) {
            Files.walk(directoryPath)
                .sorted(Comparator.reverseOrder())
                .map(Path::toFile)
                .forEach(File::delete);
            Files.createDirectories(directoryPath);
        }
    }
}
```{% endraw %}

이 코드는 디렉토리가 존재하지 않으면 새로 생성합니다. 디렉토리가 존재할 경우, {% raw %}`Files.list(directoryPath)`{% endraw %}를 사용하여 디렉토리 내에 파일이 있는지 확인하고, 파일이 없을 경우에만 디렉토리를 삭제하고 재생성합니다. 이렇게 하면 보안을 유지하면서도 원하는 비즈니스 로직을 구현할 수 있습니다.

