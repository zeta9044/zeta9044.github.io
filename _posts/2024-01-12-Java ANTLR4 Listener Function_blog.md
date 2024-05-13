---
layout: post
title: "Java ANTLR4 Listener Function"
date: 2024-01-12
tags: [Listener, Java, Function]
categories: blog
---

## 사용자 입력:
> ANTLR4의 C++ grammar로 java Listener를 만들었다. 그런데, 예를 들어 int i = 1;  i의 값은 1인데, 여기서 1은 리터럴이야.   그래서,  할당값이 되는 어떤 문자열을 어떤 함수에 넣었을 때, 리터럴인지 알려주는 어떤 함수를 만들어봐.

## ChatGPT 응답:
> ANTLR4로 생성된 파서를 사용하여 Java 코드를 분석하고, 특정 변수에 할당된 값이 리터럴인지 판별하는 함수를 만들려면, ANTLR4에서 생성된 파서 트리를 탐색하는 방법을 이해하고, 그에 따른 함수를 작성해야 합니다.

먼저, ANTLR4를 사용하여 생성된 Java 파서와 리스너를 포함하는 Java 코드를 가정합니다. 다음은 `JavaListener`를 확장하여 변수 할당을 처리하는 함수를 작성한 예제입니다:

```java
import org.antlr.v4.runtime.tree.ParseTreeListener;

public class MyJavaListener extends JavaBaseListener {

    // 변수명과 할당된 리터럴 값을 저장할 맵
    private Map<String, String> variableValues = new HashMap<>();

    @Override
    public void enterLocalVariableDeclaration(JavaParser.LocalVariableDeclarationContext ctx) {
        // 변수 선언 부분에 진입할 때 호출되는 메서드
        String variableName = ctx.variableDeclarators().variableDeclarator(0).variableDeclaratorId().getText();
        String literalValue = getLiteralValue(ctx.variableDeclarators().variableDeclarator(0).variableInitializer().expression());

        // 리터럴 값을 맵에 저장
        variableValues.put(variableName, literalValue);
    }

    // 표현식에서 리터럴 값을 추출하는 메서드
    private String getLiteralValue(JavaParser.ExpressionContext expressionContext) {
        // 간단한 예제이므로 간단한 방법으로 리터럴 값을 추출
        return expressionContext.getText();
    }

    // 변수명을 이용하여 리터럴 값을 가져오는 메서드
    public String getLiteralValueForVariable(String variableName) {
        return variableValues.get(variableName);
    }
}
```

이 리스너는 `enterLocalVariableDeclaration` 메서드에서 변수를 선언할 때 호출되며, 해당 변수의 이름과 리터럴 값(여기서는 간단히 표현식 전체로 처리)을 `variableValues` 맵에 저장합니다. `getLiteralValueForVariable` 메서드를 사용하여 변수명을 전달하면 해당 변수에 할당된 리터럴 값을 얻을 수 있습니다.

이 코드는 간단한 예제이며, 더 복잡한 경우에는 더 많은 처리가 필요할 수 있습니다.ANTLR4 파서 트리의 구조와 규칙을 이해하고, 그에 따라 코드를 작성하는 것이 중요합니다.

## ChatGPT 응답:
> ANTLR4를 사용하여 생성된 파서와 리스너에서 리터럴 여부를 확인하는 함수를 만들어보겠습니다. 먼저, ANTLR4에서 생성된 C++ 코드의 구조에 따라 예제 코드를 제공하겠습니다. 아래는 간단한 C++ 코드로 작성된 ANTLR4 문법의 예제입니다.

```antlr
// Example.g4
grammar Example;

// Parser rules
program : statement+ ;

statement : variableDeclaration ;

variableDeclaration : type Identifier '=' literal ';' ;

type : 'int' ;

literal : IntegerLiteral ;

// Lexer rules
IntegerLiteral : [0-9]+ ;
Identifier : [a-zA-Z]+ ;
```

이 예제에서는 정수 리터럴을 나타내는 `IntegerLiteral`이라는 토큰이 있습니다. 여기서는 해당 리터럴이 어떤 값을 가지는지 확인하는 함수를 만들어보겠습니다.

```cpp
#include "ExampleLexer.h"
#include "ExampleParser.h"
#include "ExampleBaseListener.h"

class MyListener : public ExampleBaseListener {
public:
    virtual void exitLiteral(ExampleParser::LiteralContext *ctx) override {
        std::string literalText = ctx->getText(); // Get the text of the literal
        if (isIntegerLiteral(literalText)) {
            std::cout << "Literal is an integer: " << literalText << std::endl;
        } else {
            std::cout << "Literal is not an integer: " << literalText << std::endl;
        }
    }

private:
    bool isIntegerLiteral(const std::string &text) {
        // Custom logic to check if the text is an integer literal
        // You can use std::stoi or other methods for more advanced checking
        try {
            std::stoi(text);
            return true;
        } catch (const std::invalid_argument& ia) {
            return false;
        } catch (const std::out_of_range& oor) {
            return false;
        }
    }
};

int main() {
    // Assuming lexer and parser are generated using ANTLR4
    antlr4::ANTLRInputStream input("int i = 1;");
    ExampleLexer lexer(&input);
    antlr4::CommonTokenStream tokens(&lexer);
    ExampleParser parser(&tokens);

    // Walk the parse tree with your custom listener
    MyListener listener;
    antlr4::tree::ParseTreeWalker::DEFAULT.walk(&listener, parser.program());

    return 0;
}
```

이 코드에서는 `MyListener` 클래스를 정의하여 `exitLiteral` 함수를 오버라이드하고, 해당 함수에서 리터럴의 텍스트를 얻어와서 `isIntegerLiteral` 함수로 전달합니다. `isIntegerLiteral` 함수에서는 리터럴이 정수인지 확인하는 간단한 로직을 사용하였습니다. 이 로직을 프로젝트에 맞게 수정하십시오.

위 코드는 예제일 뿐이며, 실제 프로젝트에 적용할 때는 파서와 리스너 클래스의 이름, 함수명 등을 프로젝트에 맞게 수정해야 합니다.

## 사용자 입력:
>     public static List<String> extractStringLiteral(String in) {
        // 문자열 리터럴을 저장할 리스트를 생성
        List<String> literals = new ArrayList<>();

        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        // 모든 토큰을 순회하며 문자열 리터럴을 찾음
        for (Token token : tokens.getTokens()) {
            // 토큰이 문자열 리터럴인 경우
            if (token.getType() == CPP14Lexer.StringLiteral) {
                // 발견된 문자열 리터럴을 출력하고 리스트에 추가
                System.out.println("Found literal: " + token.getText());
                literals.add(String.valueOf(token.getType()));
            }
        }

        // 추출된 문자열 리터럴의 리스트를 반환
        return literals;
    }  이건 StringLiteral  리스트를 만드는 함수인데,    이걸 참조해서            Stirng 이 입력됐을 때,    스트링 전체가  StringLiteral인지, CharLiteral인지, IntegerLiteral 이던지  하면, Boolean isValue(String in )  이라는  유틸함수를 만들어줘.

## ChatGPT 응답:
> 예를 들어, 입력된 문자열이 `StringLiteral`, `CharLiteral`, 또는 `IntegerLiteral` 중 어떤 종류의 리터럴인지 판별하는 `isValue` 함수를 만들어 보겠습니다. 아래는 Java로 작성된 함수입니다:

```java
public class LiteralType {
    public static final int STRING = CPP14Lexer.StringLiteral;
    public static final int CHAR = CPP14Lexer.CharacterLiteral;
    public static final int INTEGER = CPP14Lexer.IntegerLiteral;
}

public class LiteralUtil {
    public static boolean isValue(String in) {
        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        boolean foundStringLiteral = false;
        boolean foundCharLiteral = false;
        boolean foundIntegerLiteral = false;

        // 모든 토큰을 순회하며 리터럴을 찾음
        for (Token token : tokens.getTokens()) {
            switch (token.getType()) {
                case LiteralType.STRING:
                    foundStringLiteral = true;
                    break;
                case LiteralType.CHAR:
                    foundCharLiteral = true;
                    break;
                case LiteralType.INTEGER:
                    foundIntegerLiteral = true;
                    break;
            }
        }

        // 각 리터럴 종류에 따라 결과 반환
        if (foundStringLiteral && !foundCharLiteral && !foundIntegerLiteral) {
            System.out.println("The input is a StringLiteral");
            return true;
        } else if (!foundStringLiteral && foundCharLiteral && !foundIntegerLiteral) {
            System.out.println("The input is a CharLiteral");
            return true;
        } else if (!foundStringLiteral && !foundCharLiteral && foundIntegerLiteral) {
            System.out.println("The input is an IntegerLiteral");
            return true;
        } else {
            System.out.println("The input does not match any known literal types");
            return false;
        }
    }
}
```

이 함수는 입력된 문자열이 `StringLiteral`, `CharLiteral`, `IntegerLiteral` 중 어떤 종류의 리터럴인지 판별하고, 해당하는 결과를 출력합니다. 이 함수는 `LiteralType` 클래스를 사용하여 리터럴의 타입을 정의하고, `isValue` 함수에서는 각 리터럴의 발견 여부에 따라 결과를 결정합니다.

## 사용자 입력:
> 한 종류로만 되어 있는거야.... 각각이 아니라.

## ChatGPT 응답:
> 
알겠습니다. 입력된 문자열이 `StringLiteral`, `CharLiteral`, `IntegerLiteral` 중 어느 하나에 해당하는지 확인하는 함수를 만들어 보겠습니다. 이 함수는 해당 리터럴 종류에 해당하면 `true`를 반환하고, 그렇지 않으면 `false`를 반환합니다.

```java
public class LiteralUtil {
    public static boolean isStringLiteral(String in) {
        return isLiteral(in, CPP14Lexer.StringLiteral);
    }

    public static boolean isCharLiteral(String in) {
        return isLiteral(in, CPP14Lexer.CharacterLiteral);
    }

    public static boolean isIntegerLiteral(String in) {
        return isLiteral(in, CPP14Lexer.IntegerLiteral);
    }

    private static boolean isLiteral(String in, int literalType) {
        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        // 모든 토큰을 순회하며 해당 리터럴 타입이 존재하는지 확인
        for (Token token : tokens.getTokens()) {
            if (token.getType() == literalType) {
                return true;
            }
        }

        return false;
    }

    public static void main(String[] args) {
        String input = "Hello, World!"; // Example input
        System.out.println("Is String Literal: " + isStringLiteral(input));

        input = "'a'"; // Example input
        System.out.println("Is Char Literal: " + isCharLiteral(input));

        input = "42"; // Example input
        System.out.println("Is Integer Literal: " + isIntegerLiteral(input));
    }
}
```

이제 `isStringLiteral`, `isCharLiteral`, `isIntegerLiteral` 함수를 사용하여 각 리터럴 종류에 해당하는지 확인할 수 있습니다. 위의 `main` 함수는 간단한 예제로 이들 함수를 사용하는 방법을 보여줍니다.

## 사용자 입력:
> 거꾸로 만든 것 같은데... 

## ChatGPT 응답:
> 죄송합니다. 오해가 있었네요. 함수를 다시 수정하여 모든 리터럴 타입에 대해 하나의 함수로 확인할 수 있도록 만들었습니다.

```java
public class LiteralUtil {
    public static boolean isLiteral(String in) {
        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        // 모든 토큰을 순회하며 어떤 리터럴 타입이든 하나라도 발견되면 true 반환
        for (Token token : tokens.getTokens()) {
            if (token.getType() == CPP14Lexer.StringLiteral ||
                token.getType() == CPP14Lexer.CharacterLiteral ||
                token.getType() == CPP14Lexer.IntegerLiteral) {
                return true;
            }
        }

        // 어떤 리터럴 타입도 발견되지 않았으면 false 반환
        return false;
    }

    public static void main(String[] args) {
        String input = "Hello, World!"; // Example input
        System.out.println("Is Literal: " + isLiteral(input));

        input = "'a'"; // Example input
        System.out.println("Is Literal: " + isLiteral(input));

        input = "42"; // Example input
        System.out.println("Is Literal: " + isLiteral(input));
    }
}
```

이제 `isLiteral` 함수를 사용하여 입력된 문자열이 어떤 리터럴 타입에 해당하는지 확인할 수 있습니다. 위의 `main` 함수는 간단한 예제로 이 함수를 사용하는 방법을 보여줍니다.

## 사용자 입력:
> token루프로 돌렸을 때, 한쪽 종류만으로 이루어져 있어야 되는겨.

## ChatGPT 응답:
> 알겠습니다. 함수를 수정하여 입력된 문자열이 한 종류의 리터럴만으로 이루어져 있는지 확인하도록 만들겠습니다.

```java
public class LiteralUtil {
    public static boolean isSingleLiteralType(String in) {
        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        boolean foundStringLiteral = false;
        boolean foundCharLiteral = false;
        boolean foundIntegerLiteral = false;

        // 모든 토큰을 순회하며 해당 리터럴 타입이 존재하는지 확인
        for (Token token : tokens.getTokens()) {
            switch (token.getType()) {
                case CPP14Lexer.StringLiteral:
                    foundStringLiteral = true;
                    break;
                case CPP14Lexer.CharacterLiteral:
                    foundCharLiteral = true;
                    break;
                case CPP14Lexer.IntegerLiteral:
                    foundIntegerLiteral = true;
                    break;
            }
        }

        // 한 종류의 리터럴만 존재하는지 확인
        return (foundStringLiteral && !foundCharLiteral && !foundIntegerLiteral) ||
               (!foundStringLiteral && foundCharLiteral && !foundIntegerLiteral) ||
               (!foundStringLiteral && !foundCharLiteral && foundIntegerLiteral);
    }

    public static void main(String[] args) {
        String input = "Hello, World!"; // Example input
        System.out.println("Is Single Literal Type: " + isSingleLiteralType(input));

        input = "'a'"; // Example input
        System.out.println("Is Single Literal Type: " + isSingleLiteralType(input));

        input = "42"; // Example input
        System.out.println("Is Single Literal Type: " + isSingleLiteralType(input));

        input = "'a' 42"; // Example input with multiple literals
        System.out.println("Is Single Literal Type: " + isSingleLiteralType(input));
    }
}
```

이제 `isSingleLiteralType` 함수를 사용하여 입력된 문자열이 한 종류의 리터럴만으로 이루어져 있는지 확인할 수 있습니다. 위의 `main` 함수는 간단한 예제로 이 함수를 사용하는 방법을 보여줍니다.

## 사용자 입력:
> 다시 정확히 리터럴 종류를 알려줄테니, 다시 만들어줘. IntegerLiteral,CharacterLiteral,FloatingLiteral,StringLiteral

## ChatGPT 응답:
> 알겠습니다. 각각의 리터럴 종류를 확인하여 해당하는 리터럴만으로 이루어져 있는지 확인하는 함수를 만들겠습니다.

```java
public class LiteralUtil {
    public static String getLiteralType(String in) {
        // 입력된 문자열에서 CharStream을 생성
        CharStream input = CharStreams.fromString(in);

        // C++ 언어에 대한 Lexer를 생성
        CPP14Lexer lexer = new CPP14Lexer(input);

        // 토큰을 처리하기 위한 CommonTokenStream을 생성
        CommonTokenStream tokens = new CommonTokenStream(lexer);

        // 토큰을 채우기 위해 fill 메소드 호출
        tokens.fill();

        boolean foundIntegerLiteral = false;
        boolean foundCharLiteral = false;
        boolean foundFloatingLiteral = false;
        boolean foundStringLiteral = false;

        // 모든 토큰을 순회하며 해당 리터럴 타입이 존재하는지 확인
        for (Token token : tokens.getTokens()) {
            switch (token.getType()) {
                case CPP14Lexer.IntegerLiteral:
                    foundIntegerLiteral = true;
                    break;
                case CPP14Lexer.CharacterLiteral:
                    foundCharLiteral = true;
                    break;
                case CPP14Lexer.FloatingLiteral:
                    foundFloatingLiteral = true;
                    break;
                case CPP14Lexer.StringLiteral:
                    foundStringLiteral = true;
                    break;
            }
        }

        // 리터럴 종류 결정
        if (foundIntegerLiteral && !foundCharLiteral && !foundFloatingLiteral && !foundStringLiteral) {
            return "IntegerLiteral";
        } else if (!foundIntegerLiteral && foundCharLiteral && !foundFloatingLiteral && !foundStringLiteral) {
            return "CharacterLiteral";
        } else if (!foundIntegerLiteral && !foundCharLiteral && foundFloatingLiteral && !foundStringLiteral) {
            return "FloatingLiteral";
        } else if (!foundIntegerLiteral && !foundCharLiteral && !foundFloatingLiteral && foundStringLiteral) {
            return "StringLiteral";
        } else {
            return "MixedLiterals";
        }
    }

    public static void main(String[] args) {
        String input = "42"; // Example input
        System.out.println("Literal Type: " + getLiteralType(input));

        input = "'a'"; // Example input
        System.out.println("Literal Type: " + getLiteralType(input));

        input = "3.14"; // Example input
        System.out.println("Literal Type: " + getLiteralType(input));

        input = "\"Hello, World!\""; // Example input
        System.out.println("Literal Type: " + getLiteralType(input));

        input = "42 'a'"; // Example input with mixed literals
        System.out.println("Literal Type: " + getLiteralType(input));
    }
}
```

위 코드는 `getLiteralType` 함수를 사용하여 입력된 문자열이 어떤 리터럴 종류에 해당하는지 확인합니다. `main` 함수는 간단한 예제로 이 함수를 사용하는 방법을 보여줍니다.

