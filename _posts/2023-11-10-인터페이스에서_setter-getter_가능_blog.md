---
layout: post
title: "인터페이스에서 setter/getter 가능"
tags: [getter, 가능, 인터페이스, setter]
---

## 사용자 입력:
> 자바 인터페이스에서 setter/getter.를 할 수 있나?

## ChatGPT 응답:
> 자바 인터페이스에서는 {% raw %}`setter`{% endraw %}와 {% raw %}`getter`{% endraw %} 메소드를 정의할 수 있습니다. 하지만, 인터페이스 자체는 상태(필드)를 가질 수 없기 때문에, 이 메소드들은 구체적인 구현을 포함하지 않습니다. 대신, 이 메소드들은 인터페이스를 구현하는 클래스에서 구현해야 합니다.

예를 들어:

{% raw %}```java
public interface MyInterface {
    void setMyValue(int value); // setter
    int getMyValue(); // getter
}

public class MyClass implements MyInterface {
    private int myValue;

    @Override
    public void setMyValue(int value) {
        this.myValue = value;
    }

    @Override
    public int getMyValue() {
        return this.myValue;
    }
}
```{% endraw %}

여기서 {% raw %}`MyInterface`{% endraw %}는 {% raw %}`setMyValue`{% endraw %}와 {% raw %}`getMyValue`{% endraw %} 메소드를 정의하지만, 실제 구현은 {% raw %}`MyClass`{% endraw %}에서 이루어집니다. {% raw %}`MyClass`{% endraw %}는 {% raw %}`MyInterface`{% endraw %}를 구현하고, {% raw %}`myValue`{% endraw %}라는 상태를 관리합니다.

