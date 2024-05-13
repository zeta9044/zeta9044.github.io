---
layout: post
title: "JavaCC Gradle 태스크 제한"
tags: [제한, 태스크, Gradle, JavaCC]
---

## 사용자 입력:
> compileJavacc {
    inputDirectory = file(parserDir)
    outputDirectory = file(javaDir) } 이런 javaCC gradle태스크에서 outputDirectory에 들어갈 파일을 제한할 방법은?

## ChatGPT 응답:
> JavaCC (Java Compiler Compiler)는 주로 파서 및 어휘 분석기를 생성하는 데 사용되는 도구입니다. Gradle과 함께 사용하면 JavaCC를 빌드 프로세스의 일부로 통합할 수 있습니다.

{% raw %}`compileJavacc`{% endraw %} 태스크에서 {% raw %}`outputDirectory`{% endraw %}에 들어갈 파일을 제한하려면, 일반적으로 Gradle의 파일 필터링 기능을 사용할 수 있습니다. 그러나 JavaCC 자체는 입력 파일을 기반으로 출력 파일을 생성하므로, 직접적으로 {% raw %}`outputDirectory`{% endraw %}의 출력 파일을 제한하는 것은 적절하지 않을 수 있습니다.

대신, {% raw %}`inputDirectory`{% endraw %}의 입력 파일을 필터링하여 원하는 파일만 JavaCC로 처리하도록 할 수 있습니다.

다음은 입력 디렉토리에서 {% raw %}`.jj`{% endraw %} 확장자를 가진 파일만 처리하도록 하는 예제입니다:

{% raw %}```groovy
compileJavacc {
    inputDirectory = fileTree(dir: parserDir, include: '**/*.jj')
    outputDirectory = file(javaDir)
}
```{% endraw %}

이 방식을 사용하면 JavaCC가 처리할 입력 파일을 제한할 수 있으므로 결과적으로 {% raw %}`outputDirectory`{% endraw %}에 생성되는 출력 파일도 제한됩니다.

그러나 만약 이미 생성된 출력 파일을 삭제하거나 제한하려는 경우, 추가적인 Gradle 태스크를 작성하여 해당 작업을 수행할 수 있습니다.

## 사용자 입력:
> outputDirectory에 이미 A라는 클래스가 있는데, compileJavacc를 하면 A가 만들어져. 그래서, 만들어진 A를 덮어쓰지 못하게 하려고 그러는 거야.

## ChatGPT 응답:
> 아, 이해했습니다. {% raw %}`outputDirectory`{% endraw %}에 이미 있는 파일을 덮어쓰지 않도록 하려면 몇 가지 접근 방식을 사용할 수 있습니다.

1. **Pre-execution Check**: {% raw %}`compileJavacc`{% endraw %} 태스크를 실행하기 전에 해당 디렉토리에 파일이 있는지 확인하고, 있다면 태스크를 중단합니다.
2. **Backup Strategy**: 파일이 이미 존재하는 경우, 해당 파일을 백업하고 새 파일을 생성합니다.

다음은 첫 번째 방법인 pre-execution check를 구현한 예제입니다:

{% raw %}```groovy
compileJavacc {
    inputDirectory = file(parserDir)
    outputDirectory = file(javaDir)
    
    doFirst {
        // Check if A.class exists in the outputDirectory
        if (new File(outputDirectory, 'A.class').exists()) {
            throw new GradleException("A.class already exists in the output directory. Aborting task.")
        }
    }
}
```{% endraw %}

이 코드는 {% raw %}`compileJavacc`{% endraw %} 태스크가 실행되기 전에 {% raw %}`doFirst`{% endraw %} 블록 내부의 코드를 실행합니다. 만약 {% raw %}`A.class`{% endraw %} 파일이 {% raw %}`outputDirectory`{% endraw %}에 이미 존재하면 Gradle 예외를 발생시켜 태스크를 중단시킵니다.

이 방식을 사용하면 {% raw %}`A.class`{% endraw %} 파일이 이미 {% raw %}`outputDirectory`{% endraw %}에 있을 경우 {% raw %}`compileJavacc`{% endraw %} 태스크가 실행되지 않도록 할 수 있습니다.

## 사용자 입력:
> 중단시키는 것보다 skip하라고 처리할 수는 없나?

## ChatGPT 응답:
> 네, 그렇게 하려면 {% raw %}`onlyIf`{% endraw %} 조건을 사용하여 태스크를 스킵할 수 있습니다. 

{% raw %}`onlyIf`{% endraw %}는 주어진 조건이 {% raw %}`false`{% endraw %}를 반환하면 태스크를 실행하지 않게 해줍니다.

다음은 {% raw %}`A.class`{% endraw %} 파일이 {% raw %}`outputDirectory`{% endraw %}에 존재하는 경우 {% raw %}`compileJavacc`{% endraw %} 태스크를 스킵하는 예제입니다:

{% raw %}```groovy
compileJavacc {
    inputDirectory = file(parserDir)
    outputDirectory = file(javaDir)

    onlyIf {
        !new File(outputDirectory, 'A.class').exists()
    }
}
```{% endraw %}

이 코드는 {% raw %}`A.class`{% endraw %} 파일이 {% raw %}`outputDirectory`{% endraw %}에 존재하지 않을 경우에만 {% raw %}`compileJavacc`{% endraw %} 태스크를 실행하도록 조건을 설정합니다. 만약 {% raw %}`A.class`{% endraw %} 파일이 존재하면, 해당 태스크는 스킵됩니다.

## 사용자 입력:
> 위 내용은 compileJavacc를 실행또는 중단이잖아..나는 생성하는 파일을 제한하고 싶은거야.

## ChatGPT 응답:
> 죄송합니다, 제가 오해했습니다.

파일 생성을 제한하려면 {% raw %}`compileJavacc`{% endraw %} 태스크의 출력 결과를 조작하거나 후처리하는 추가 태스크를 생성해야 합니다.

다음의 접근 방식을 사용하여 원하는 결과를 얻을 수 있습니다:

1. {% raw %}`compileJavacc`{% endraw %} 태스크의 후처리 단계에서 {% raw %}`outputDirectory`{% endraw %}에 생성된 {% raw %}`A.class`{% endraw %} 파일을 삭제합니다.
2. {% raw %}`compileJavacc`{% endraw %} 태스크가 실행된 후, 생성되는 파일을 확인하고, 원치 않는 파일을 삭제하는 별도의 태스크를 생성합니다.

다음은 첫 번째 방법인 후처리 단계에서 {% raw %}`A.class`{% endraw %} 파일을 삭제하는 예제입니다:

{% raw %}```groovy
compileJavacc {
    inputDirectory = file(parserDir)
    outputDirectory = file(javaDir)
    
    doLast {
        // Delete A.class from the outputDirectory if it exists
        def aClassFile = new File(outputDirectory, 'A.class')
        if (aClassFile.exists()) {
            aClassFile.delete()
        }
    }
}
```{% endraw %}

이 코드는 {% raw %}`compileJavacc`{% endraw %} 태스크가 실행된 후에 {% raw %}`doLast`{% endraw %} 블록 내부의 코드를 실행합니다. 만약 {% raw %}`A.class`{% endraw %} 파일이 {% raw %}`outputDirectory`{% endraw %}에 생성되었다면 해당 파일을 삭제합니다. 

이렇게 하면 {% raw %}`compileJavacc`{% endraw %} 태스크가 실행된 후에도 {% raw %}`A.class`{% endraw %} 파일은 {% raw %}`outputDirectory`{% endraw %}에 존재하지 않게 됩니다.

## 사용자 입력:
> 이미 있는 파일이라서, 덮어쓰지 못하게 하려는 거야.

## ChatGPT 응답:
> 아, 그렇다면 원하는 결과를 얻기 위해 다음과 같은 접근 방식을 사용할 수 있습니다:

1. {% raw %}`compileJavacc`{% endraw %} 실행 전에 {% raw %}`A.class`{% endraw %} 파일을 임시로 다른 위치로 이동시킵니다.
2. {% raw %}`compileJavacc`{% endraw %} 실행 후에 임시로 이동한 {% raw %}`A.class`{% endraw %} 파일을 원래 위치로 다시 복원합니다.

다음은 이 접근 방식을 구현한 예제입니다:

{% raw %}```groovy
def backupFile = new File(outputDirectory, 'A.class.bak')

compileJavacc {
    inputDirectory = file(parserDir)
    outputDirectory = file(javaDir)
    
    doFirst {
        // Backup A.class to A.class.bak if it exists
        def aClassFile = new File(outputDirectory, 'A.class')
        if (aClassFile.exists()) {
            aClassFile.renameTo(backupFile)
        }
    }
    
    doLast {
        // Restore A.class from A.class.bak if it exists
        if (backupFile.exists()) {
            backupFile.renameTo(new File(outputDirectory, 'A.class'))
            backupFile.delete() // Optionally delete the backup
        }
    }
}
```{% endraw %}

이 코드는:

1. {% raw %}`compileJavacc`{% endraw %} 태스크 실행 전에 {% raw %}`A.class`{% endraw %} 파일이 존재하면 {% raw %}`A.class.bak`{% endraw %}로 이름을 바꿉니다.
2. {% raw %}`compileJavacc`{% endraw %} 태스크가 실행된 후에 {% raw %}`A.class.bak`{% endraw %} 파일이 존재하면 다시 {% raw %}`A.class`{% endraw %}로 이름을 바꾸고, 임시 백업 파일을 삭제합니다.

이렇게 하면 {% raw %}`compileJavacc`{% endraw %} 태스크가 {% raw %}`A.class`{% endraw %} 파일을 덮어쓰지 않습니다.

