---
layout: post
title: "Maven Build - Javadoc Warnings"
tags: [Maven, Javadoc, Build, Warnings]
---

## 사용자 입력:
> [INFO] No previous run data found, generating javadoc.
[WARNING] Javadoc Warnings
[WARNING] C:\git\jgit\org.eclipse.jgit\src\org\eclipse\jgit\util\FS.java:33: warning: [removal] AccessControlException in java.security has been deprecated and marked for removal
[WARNING] import java.security.AccessControlException;
[WARNING] ^
[WARNING] C:\git\jgit\org.eclipse.jgit\src\org\eclipse\jgit\util\SystemReader.java:26: warning: [removal] AccessController in java.security has been deprecated and marked for removal
[WARNING] import java.security.AccessController;
[WARNING] ^
[WARNING] C:\git\jgit\org.eclipse.jgit\src\org\eclipse\jgit\transport\SshSessionFactory.java:14: warning: [removal] AccessController in java.security has been deprecated and marked for removal
[WARNING] import java.security.AccessController;
[WARNING] ^
[WARNING] C:\git\jgit\org.eclipse.jgit\src\org\eclipse\jgit\util\FS_Win32_Cygwin.java:17: warning: [removal] AccessController in java.security has been deprecated and marked for removal
[WARNING] import java.security.AccessController;
[WARNING] ^
[WARNING] C:\git\jgit\org.eclipse.jgit\src\org\eclipse\jgit\util\io\ThrowingPrintWriter.java:14: warning: [removal] AccessController in java.security has been deprecated and marked for removal
[WARNING] import java.security.AccessController;
[WARNING] ^
[WARNING] warning: The code being documented uses packages in the unnamed module, but the packages defined in https://docs.oracle.com/en/java/javase/11/docs/api/ are in named modules.
[WARNING] 6 warnings
[INFO] ------------------------------------------------------------------------
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  31.273 s (Wall Clock)
[INFO] Finished at: 2023-09-21T13:15:29+09:00
[INFO] ------------------------------------------------------------------------
[WARNING] 
[WARNING] Plugin validation issues were detected in 3 plugin(s)
[WARNING] 
[WARNING]  * org.jacoco:jacoco-maven-plugin:0.8.10
[WARNING]  * org.apache.maven.plugins:maven-compiler-plugin:3.11.0
[WARNING]  * org.apache.maven.plugins:maven-javadoc-plugin:3.5.0
[WARNING] 
[WARNING] For more or less details, use 'maven.plugin.validation' property with one of the values (case insensitive): [BRIEF, DEFAULT, VERBOSE]
[WARNING] 
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-javadoc-plugin:3.5.0:jar (default) on project org.eclipse.jgit: MavenReportException: Error while generating Javadoc: Project contains Javadoc Warnings -> [Help 1]
[ERROR] 
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR] 
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/MojoExecutionException

Process finished with exit code 1


## ChatGPT 응답:
> The log you provided indicates a build failure caused by Javadoc generation. Let's break down the issues and provide solutions:

1. **Javadoc Warnings**
   The Javadoc tool is warning about the use of classes/methods that have been deprecated and marked for removal in future versions of Java. Specifically, the classes {% raw %}`AccessControlException`{% endraw %} and {% raw %}`AccessController`{% endraw %} from the {% raw %}`java.security`{% endraw %} package are being referenced in multiple files:

   - {% raw %}`FS.java`{% endraw %}
   - {% raw %}`SystemReader.java`{% endraw %}
   - {% raw %}`SshSessionFactory.java`{% endraw %}
   - {% raw %}`FS_Win32_Cygwin.java`{% endraw %}
   - {% raw %}`ThrowingPrintWriter.java`{% endraw %}

   **Solution**: You should consider refactoring the code to avoid using deprecated features. If this is not an option, you can configure the Javadoc tool to ignore these warnings.

2. **Unnamed Module Warning**
   This warning suggests that the code uses packages in the unnamed module, but the packages defined in the given URL are in named modules. This is related to Java's module system introduced in Java 9.

   **Solution**: If you're not planning to use Java's module system, you can ignore this warning. Otherwise, consider creating a {% raw %}`module-info.java`{% endraw %} file to define your module.

3. **Plugin Validation Issues**
   Warnings are raised about potential issues in some Maven plugins:

   - {% raw %}`org.jacoco:jacoco-maven-plugin:0.8.10`{% endraw %}
   - {% raw %}`org.apache.maven.plugins:maven-compiler-plugin:3.11.0`{% endraw %}
   - {% raw %}`org.apache.maven.plugins:maven-javadoc-plugin:3.5.0`{% endraw %}

   **Solution**: Ensure that you're using the latest versions of these plugins. If you are, consult the documentation or issue trackers for these plugins to see if there are known issues or workarounds.

4. **Build Failure**
   The build failed because the Javadoc tool encountered warnings, and the build configuration likely treats these warnings as errors.

   **Solution**: If you cannot address the warnings immediately, one workaround is to configure the {% raw %}`maven-javadoc-plugin`{% endraw %} to allow warnings. You can do this by adding the following configuration to the plugin in your {% raw %}`pom.xml`{% endraw %}:

   {% raw %}```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-javadoc-plugin</artifactId>
       <version>3.5.0</version>
       <configuration>
           <failOnError>false</failOnError>
       </configuration>
   </plugin>
   ```{% endraw %}

   This will prevent the build from failing due to Javadoc warnings. However, it's a good practice to eventually address these warnings, especially if they relate to deprecated features.

5. **Additional Information**
   The log suggests using the {% raw %}`-e`{% endraw %} and {% raw %}`-X`{% endraw %} switches with Maven for more detailed error information.

   **Solution**: If you continue to encounter issues, consider running Maven with these switches to get more detailed logs:

   {% raw %}```
   mvn clean install -e -X
   ```{% endraw %}

In summary, you should address the Javadoc warnings related to deprecated features and consider updating or configuring your Maven plugins. If you're in a hurry, you can temporarily configure the build to allow Javadoc warnings, but it's a good practice to address them in the long run.

## 사용자 입력:
> 한글로 설명해.

## ChatGPT 응답:
> 네, 알겠습니다.

1. **Javadoc 경고**
   Javadoc 도구는 향후 Java 버전에서 삭제될 예정인 클래스/메서드의 사용에 대해 경고하고 있습니다. 특히 {% raw %}`java.security`{% endraw %} 패키지의 {% raw %}`AccessControlException`{% endraw %} 및 {% raw %}`AccessController`{% endraw %} 클래스가 여러 파일에서 참조되고 있습니다:

   - {% raw %}`FS.java`{% endraw %}
   - {% raw %}`SystemReader.java`{% endraw %}
   - {% raw %}`SshSessionFactory.java`{% endraw %}
   - {% raw %}`FS_Win32_Cygwin.java`{% endraw %}
   - {% raw %}`ThrowingPrintWriter.java`{% endraw %}

   **해결책**: 코드를 리팩토링하여 사용 중단된 기능을 사용하지 않도록 변경하는 것을 고려해야 합니다. 그렇지 않으면 Javadoc 도구를 구성하여 이러한 경고를 무시하도록 설정할 수 있습니다.

2. **이름 없는 모듈 경고**
   이 경고는 코드가 이름 없는 모듈의 패키지를 사용하지만 주어진 URL에서 정의된 패키지가 명명된 모듈에 있다고 제안합니다. 이는 Java 9에서 도입된 Java의 모듈 시스템과 관련이 있습니다.

   **해결책**: Java의 모듈 시스템을 사용할 계획이 없다면 이 경고는 무시해도 됩니다. 그렇지 않다면 모듈을 정의하기 위해 {% raw %}`module-info.java`{% endraw %} 파일을 생성하는 것을 고려해 보세요.

3. **플러그인 유효성 문제**
   일부 Maven 플러그인에서 잠재적인 문제에 대한 경고가 발생하였습니다:

   - {% raw %}`org.jacoco:jacoco-maven-plugin:0.8.10`{% endraw %}
   - {% raw %}`org.apache.maven.plugins:maven-compiler-plugin:3.11.0`{% endraw %}
   - {% raw %}`org.apache.maven.plugins:maven-javadoc-plugin:3.5.0`{% endraw %}

   **해결책**: 이러한 플러그인의 최신 버전을 사용하고 있는지 확인하세요. 사용 중이라면 해당 플러그인의 문서나 문제 추적기를 확인하여 알려진 문제나 해결 방법이 있는지 확인하세요.

4. **빌드 실패**
   Javadoc 도구가 경고를 발견하여 빌드 구성이 이러한 경고를 오류로 취급하기 때문에 빌드가 실패하였습니다.

   **해결책**: 경고를 즉시 해결할 수 없는 경우, {% raw %}`pom.xml`{% endraw %}의 플러그인에 다음 구성을 추가하여 Javadoc 경고를 허용하도록 설정하는 방법이 있습니다:

   {% raw %}```xml
   <plugin>
       <groupId>org.apache.maven.plugins</groupId>
       <artifactId>maven-javadoc-plugin</artifactId>
       <version>3.5.0</version>
       <configuration>
           <failOnError>false</failOnError>
       </configuration>
   </plugin>
   ```{% endraw %}

   이렇게 하면 Javadoc 경고로 인한 빌드 실패를 방지할 수 있습니다. 그러나 특히 사용 중단된 기능과 관련된 경고는 장기적으로 해결하는 것이 좋습니다.

5. **추가 정보**
   로그에서는 Maven을 {% raw %}`-e`{% endraw %} 및 {% raw %}`-X`{% endraw %} 스위치와 함께 사용하여 더 자세한 오류 정보를 얻을 것을 제안하고 있습니다.

   **해결책**: 계속 문제가 발생하면 더 자세한 로그를 얻기 위해 다음과 같은 명령으로 Maven을 실행해 보세요:

   {% raw %}```
   mvn clean install -e -X
   ```{% endraw %}

요약하면, 사용 중단된 기능과 관련된 Javadoc 경고를 해결하고 Maven 플러그인을 업데이트하거나 구성하는 것을 고려해야 합니다. 급하다면 일시적으로 Javadoc 경고를 허용하도록 빌드를 구성할 수 있지만, 장기적으로는 이러한 경고를 해결하는 것이 좋습니다.

