---
layout: post
title: "ANTLR4로 C++ 파서"
date: 2023-10-20
tags: [파서, C, 로]
categories: blog
---

## 사용자 입력:
> #include <vector>

std::vector<int> vec = {1, 2, 3, 4, 5};
vec.push_back(6);  // 6을 추가
이런 C++ 소스 코드가 있는데, antlr4로 C++파서를 만들고 싶어. 그런데, vec의 최종값을 파서에서 추출할려면?

## ChatGPT 응답:
> ANTLR4로 C++ 코드를 파싱하기 위해선, 우선 C++의 문법에 맞는 ANTLR4 문법파일(grammars)이 필요하다. C++의 문법은 복잡하므로, 정확하고 완전한 파서를 만드는 것은 어려울 수 있다. 다행히도, ANTLR4에 대한 다양한 문법 파일들이 오픈소스 커뮤니티에 공유되어 있다.

1. **C++ ANTLR4 Grammar 가져오기**
   - [GitHub의 ANTLR grammars repository](https://github.com/antlr/grammars-v4)에서 C++의 grammar를 찾을 수 있다. 이 repository에서 C++ 문법에 대한 `.g4` 파일들을 다운로드 받는다.

2. **ANTLR4 Tool을 사용하여 파서 생성**
   - `.g4` 파일을 사용하여 ANTLR4로부터 파서와 렉서 코드를 생성한다.
   - 예: `antlr4 CPP14.g4 -Dlanguage=Cpp -o generated/`

3. **소스코드 작성**
   - 생성된 파서와 렉서 코드를 사용하여 원하는 작업(여기서는 `vec`의 최종값 추출)을 수행하는 코드를 작성한다.

4. **vec의 최종값 추출하기**
   - `vec`의 최종값을 추출하려면, `push_back`이나 다른 수정 메서드들을 추적하고 그들의 인자를 해석해야 한다. 이를 위해 파서의 리스너나 방문자 패턴을 사용하여 해당 문장을 찾고 분석한다.

```cpp
class VecValueExtractor : public CPP14BaseListener {
public:
    void enterExpression(CPP14Parser::ExpressionContext *ctx) override {
        // "vec.push_back" 문장을 찾는 로직 작성
        // 찾으면 해당 문장의 인자를 해석
    }
};

int main() {
    // 파서 초기화 및 입력 코드 설정
    // ...

    CPP14Parser parser(&tokens);
    tree::ParseTree *tree = parser.translationUnit();

    VecValueExtractor extractor;
    tree::ParseTreeWalker::DEFAULT.walk(&extractor, tree);

    // vec의 최종값 출력
    // ...
}
```

참고로, 이는 기본적인 방법이며, 실제 C++ 코드의 복잡성 때문에 완벽한 결과를 얻기 위해서는 추가적인 작업이 필요할 수 있다.

