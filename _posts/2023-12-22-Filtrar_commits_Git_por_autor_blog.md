---
layout: post
title: "Filtrar commits Git por autor"
tags: [Filtrar, commits, autor, por, Git]
---

## 사용자 입력:
> intellj의 git log화면에서 사용자로 필터하고 싶은데, 나이외에는 선택을 못하는 것 같은데.

## ChatGPT 응답:
> search("IntelliJ git log screen filter by user")

## ChatGPT 응답:
> mclick(["0", "1", "2", "3", "5"])

## ChatGPT 응답:
> In IntelliJ IDEA, you can filter the Git log by user using the User filter option in the Log tab of the Version Control tool window. Here's how you can do it:

1. Open the Git Log tool window by hovering over the quick access button in the bottom left of the IntelliJ IDEA window and selecting Git, or by using {% raw %}`Alt+9`{% endraw %} (Windows/Linux) or {% raw %}`⌘9`{% endraw %} (macOS).
2. In the Git Log tool window, you will see various filtering options including User. 
3. To filter commits by a specific author, click on "Select" under the User filter section and start typing the author's name. The interface allows you to specify the author's name, and if necessary, handle usernames with special characters like commas. Just ensure to specify each username on a new line if there are special characters involved.
4. After selecting the author, the log will update to show only commits made by that author.

Note that this feature is specifically for filtering commits by authors and does not support filtering out commits using regular expressions or other advanced filtering criteria directly in the User filter field. If you encounter issues with special characters in usernames, such as commas, you can still correctly filter by entering each username on a new line.

For more detailed information and instructions, you can refer to the [IntelliJ IDEA documentation](https://www.jetbrains.com/help/idea/log-tab.html) and the JetBrains community support pages ([filtering by user with comma in name](https://intellij-support.jetbrains.com/hc/en-us/community/posts/207721185-Version-control-log-tab-filtering-by-user-with-comma-in-name), [hide commits by user or regex in GIT Log view](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360000642619-Hide-commits-by-user-or-regex-in-GIT-Log-view-)).

## 사용자 입력:
> 한글로 설명해.

## ChatGPT 응답:
> IntelliJ IDEA에서 Git 로그를 사용자별로 필터링하는 방법은 다음과 같습니다:

1. IntelliJ IDEA의 왼쪽 하단에 있는 빠른 접근 버튼을 마우스로 가리키고 Git을 선택하거나 {% raw %}`Alt+9`{% endraw %}(Windows/Linux) 또는 {% raw %}`⌘9`{% endraw %}(macOS)를 사용하여 Git 로그 도구 창을 엽니다.
2. Git 로그 도구 창에서 여러 필터링 옵션이 표시됩니다. 여기에서 "User" 필터 옵션을 찾습니다.
3. 특정 사용자의 커밋을 필터링하려면 User 필터 섹션에서 "Select"를 클릭하고 사용자 이름을 입력합니다. 인터페이스에서는 필요한 경우 특수 문자(예: 쉼표)가 포함된 사용자 이름을 처리할 수 있습니다. 특수 문자가 포함된 경우 각 사용자 이름을 새 줄에 지정해야 합니다.
4. 사용자를 선택한 후 로그는 해당 사용자가 만든 커밋만 표시하도록 업데이트됩니다.

이 기능은 저자별 커밋을 필터링하는 데 특화되어 있으며, User 필터 필드에서 정규 표현식이나 다른 고급 필터링 기준을 직접 사용하는 것은 지원하지 않습니다. 사용자 이름에 특수 문자(예: 쉼표)가 있는 경우, 각 사용자 이름을 새 줄에 입력하여 올바르게 필터링할 수 있습니다.

자세한 정보와 지침은 [IntelliJ IDEA 문서](https://www.jetbrains.com/help/idea/log-tab.html) 및 JetBrains 커뮤니티 지원 페이지([쉼표가 있는 이름으로 사용자 필터링](https://intellij-support.jetbrains.com/hc/en-us/community/posts/207721185-Version-control-log-tab-filtering-by-user-with-comma-in-name), [Git 로그 뷰에서 사용자 또는 정규 표현식으로 커밋 숨기기](https://intellij-support.jetbrains.com/hc/en-us/community/posts/360000642619-Hide-commits-by-user-or-regex-in-GIT-Log-view-))를 참조하십시오.

