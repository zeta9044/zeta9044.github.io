---
layout: post
title: "Resolve Eclipse JGit Library"
date: 2023-09-20
tags: [JGit, Eclipse, Library, Resolve]
categories: blog
---

## 사용자 입력:
> FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':aiworks:compileJava'.
> Could not resolve all files for configuration ':aiworks:compileClasspath'.
   > Could not find org.eclipse.jgit:org.eclipse.jgit:5.13.0.202.0.0.0827-r.
     Searched in the following locations:
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit/5.13.0.202.0.0.0827-r/org.eclipse.jgit-5.13.0.202.0.0.0827-r.pom
     If the artifact you are trying to retrieve can be found in the repository but without metadata in 'Maven POM' format, you need to adjust the 'metadataSources { ... }' of the repository declaration.
     Required by:
         project :aiworks
   > Could not find org.eclipse.jgit:org.eclipse.jgit.archive:5.13.0.202.0.0.0827-r.
     Searched in the following locations:
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.archive/5.13.0.202.0.0.0827-r/org.eclipse.jgit.archive-5.13.0.202.0.0.0827-r.pom
     If the artifact you are trying to retrieve can be found in the repository but without metadata in 'Maven POM' format, you need to adjust the 'metadataSources { ... }' of the repository declaration.
     Required by:
         project :aiworks
   > Could not find org.eclipse.jgit:org.eclipse.jgit.ssh.jsch:5.13.0.202.0.0.0827-r.
     Searched in the following locations:
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.ssh.jsch/5.13.0.202.0.0.0827-r/org.eclipse.jgit.ssh.jsch-5.13.0.202.0.0.0827-r.pom
     If the artifact you are trying to retrieve can be found in the repository but without metadata in 'Maven POM' format, you need to adjust the 'metadataSources { ... }' of the repository declaration.
     Required by:
         project :aiworks

## ChatGPT 응답:
> The error message suggests that during the build process, the specified version of `org.eclipse.jgit` libraries could not be found in the mentioned repository. Here are some steps you can take to resolve the issue:

1. **Check Version Availability**: Go to the provided URLs in a browser and check if the specific version of the libraries is available. If not, you might want to consider downgrading to a version that is available or look for alternative repositories that might have the required version.

2. **Update the Repository**: If the mentioned repository is not the correct one or if it's outdated, you might want to update your `build.gradle` file to point to the correct or updated repository.

3. **Local Dependency**: If you have the JAR files for these libraries locally, you can include them directly in your project. Place the JARs in a `libs` directory in your project and include them in your `build.gradle` like this:

   ```gradle
   dependencies {
       implementation fileTree(dir: 'libs', include: ['*.jar'])
   }
   ```

4. **Check Maven Central or JCenter**: Often, libraries are available on Maven Central or JCenter. Check if the required version of the library is available there. If yes, include that repository in your `build.gradle`:

   ```gradle
   repositories {
       mavenCentral()
       // or
       jcenter()
   }
   ```

5. **Adjust Metadata Sources**: As the error suggests, if the artifact is available without the 'Maven POM' format metadata, adjust the `metadataSources` of the repository declaration in your `build.gradle`:

   ```gradle
   repositories {
       maven {
           url 'https://repo.eclipse.org/content/groups/releases/'
           metadataSources {
               gradleMetadata()
               mavenPom()
               artifact()
           }
       }
   }
   ```

6. **Check for Proxy or Network Issues**: Sometimes, due to network issues or company proxies, certain repositories might be inaccessible. Ensure that your network/proxy settings allow access to the Eclipse repository.

7. **Consider Using Another Version**: If a specific version of the library is causing issues, consider using another version that might be more readily available or stable.

After making these changes, try building your project again. If you continue to face issues, share more details about your `build.gradle` file and project setup so I can provide more specific advice.

## 사용자 입력:
> * What went wrong:
Execution failed for task ':aiworks:compileJava'.
> Could not resolve all files for configuration ':aiworks:compileClasspath'.
   > Could not find org.eclipse.jgit:org.eclipse.jgit:5.13.0.202.0.0.0827-r.
     Searched in the following locations:
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit/5.13.0.202.0.0.0827-r/org.eclipse.jgit-5.13.0.202.0.0.0827-r.module
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit/5.13.0.202.0.0.0827-r/org.eclipse.jgit-5.13.0.202.0.0.0827-r.pom
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit/5.13.0.202.0.0.0827-r/org.eclipse.jgit-5.13.0.202.0.0.0827-r.jar
     Required by:
         project :aiworks
   > Could not find org.eclipse.jgit:org.eclipse.jgit.archive:5.13.0.202.0.0.0827-r.
     Searched in the following locations:
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.archive/5.13.0.202.0.0.0827-r/org.eclipse.jgit.archive-5.13.0.202.0.0.0827-r.module
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.archive/5.13.0.202.0.0.0827-r/org.eclipse.jgit.archive-5.13.0.202.0.0.0827-r.pom
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.archive/5.13.0.202.0.0.0827-r/org.eclipse.jgit.archive-5.13.0.202.0.0.0827-r.jar
     Required by:
         project :aiworks
   > Could not find org.eclipse.jgit:org.eclipse.jgit.ssh.jsch:5.13.0.202.0.0.0827-r.
     Searched in the following locations:
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.ssh.jsch/5.13.0.202.0.0.0827-r/org.eclipse.jgit.ssh.jsch-5.13.0.202.0.0.0827-r.module
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.ssh.jsch/5.13.0.202.0.0.0827-r/org.eclipse.jgit.ssh.jsch-5.13.0.202.0.0.0827-r.pom
       - https://repo.eclipse.org/content/groups/releases/org/eclipse/jgit/org.eclipse.jgit.ssh.jsch/5.13.0.202.0.0.0827-r/org.eclipse.jgit.ssh.jsch-5.13.0.202.0.0.0827-r.jar
     Required by:
         project :aiworks

## ChatGPT 응답:
> The updated error message indicates that Gradle is looking for multiple file formats (`.module`, `.pom`, and `.jar`) of the `org.eclipse.jgit` libraries in the specified repository but can't find them.

Here's what you can do:

1. **Verify the Version**: The version you're trying to use (`5.13.0.202.0.0.0827-r`) might not be available in the repository. You can visit the repository URL to verify if that version is indeed available.

2. **Check Alternative Repositories**: Libraries are often available in multiple repositories. Maven Central is a popular one. If the library is available there, add it to your `build.gradle`:

   ```gradle
   repositories {
       mavenCentral()
   }
   ```

3. **Use a Different Version**: It's possible that the specific version you're trying to use is not available or has issues. Check the repository for the latest stable version or a version you know is available and update your dependencies accordingly.

4. **Local Dependency**: As previously mentioned, if you have the JAR files for these libraries, you can include them in your project. This is a quick fix if you have the JARs and don't want to deal with repository issues.

5. **Manually Download and Install**: If you can find the `.jar` files from a trusted source, you can manually download them and install them to your local maven repository. The command to install a JAR to the local maven repository is:

   ```bash
   mvn install:install-file -Dfile=<path-to-jar> -DgroupId=org.eclipse.jgit -DartifactId=org.eclipse.jgit -Dversion=5.13.0.202.0.0.0827-r -Dpackaging=jar
   ```

   You would need to run similar commands for the other missing JARs, adjusting the `-DartifactId` parameter accordingly.

6. **Proxy Issues**: If you're behind a corporate firewall, there might be restrictions that prevent you from accessing external repositories. Make sure you have the necessary permissions or are using the correct proxy settings.

7. **Reach Out**: Consider reaching out to the maintainers of the repository or the library. They might provide insights into whether the version you're trying to use has issues or if there are better alternatives.

After trying the above solutions, attempt to rebuild your project. If the problem persists, consider sharing your `build.gradle` or any other configurations that might be relevant, so I can provide further guidance.

