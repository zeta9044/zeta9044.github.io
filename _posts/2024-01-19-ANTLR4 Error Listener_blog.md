---
layout: post
title: "ANTLR4 Error Listener"
date: 2024-01-19
tags: [Listener, Error]
categories: blog
---

## 사용자 입력:
> public class CppMgr extends ParserImpl {

    private static final Logger logger = LogManager.getLogger(CppMgr.class);
    private static final String PARSER_ID = "P0031";
    private static final String PARSER_FILE_EXT = "CPP";

    @Override
    public void parse(String prjIdPRMT, FileInfo foPRMT, RuleSet ruleSetPRMT, MultiValueMap uniqueOwnerInfoPRMT, HashMap<String, String> ffdColListPRMT, MultiKeyMap jobSchemaMPPRMT) throws Exception {

        String analysisYn = ConstDef.YN_N;

        try {
            CharStream input = CharStreams.fromFileName(foPRMT.getSrcPath());
            CPP14Lexer lexer = new CPP14Lexer(input);
            CommonTokenStream tokens = new CommonTokenStream(lexer);

            CPP14Parser parser = new CPP14Parser(tokens);
            ParserErrorListener errorListener = new ParserErrorListener(prjIdPRMT, foPRMT.sFullPath);
            parser.removeErrorListeners();
            parser.addErrorListener(errorListener);

            CPP14Parser.TranslationUnitContext tree = parser.translationUnit();

            Reqlog.info("Cpp(C++) Parser START...");
            Cpp14ParsingListener p = new Cpp14ParsingListener(tokens, prjIdPRMT, foPRMT, ruleSetPRMT, uniqueOwnerInfoPRMT, ffdColListPRMT);
            ParseTreeWalker walker = new ParseTreeWalker();
            walker.walk(p, tree);


            analysisYn = ConstDef.YN_Y;
            Reqlog.info("Cpp(C++) Parser END...");
        } catch (Exception e) {
            logger.error("file path: " + foPRMT.sFullPath);
            logger.error(LoggingUtils.getStackTraceAsString(e));
        } finally {
            _makeFile(prjIdPRMT, foPRMT.nIndex, foPRMT, analysisYn);
        }

    }

    private void _makeFile(String prjId, int fileId, FileInfo parseFile, String analysisYn) {
        parseFile.setParserId(PARSER_ID);
        parseFile.sFileExt = PARSER_FILE_EXT;
        ChannelWriter.addFile(prjId, fileId, parseFile, analysisYn);
    }
}  이 클래스에 대한  errorListener를 만들어줘. ANTLR4와는 관계없어.

## ChatGPT 응답:
> Sure, I can help you create an error listener for the `CppMgr` class. In your code, you already have a class named `ParserErrorListener`. Assuming this class is used for error handling in the ANTLR4 parser, you can modify it to extend the `BaseErrorListener` class provided by ANTLR4.

Here's an example of how you can modify your `ParserErrorListener`:

```java
import org.antlr.v4.runtime.BaseErrorListener;
import org.antlr.v4.runtime.RecognitionException;
import org.antlr.v4.runtime.Recognizer;

public class ParserErrorListener extends BaseErrorListener {

    private String prjId;
    private String filePath;

    public ParserErrorListener(String prjId, String filePath) {
        this.prjId = prjId;
        this.filePath = filePath;
    }

    @Override
    public void syntaxError(Recognizer<?, ?> recognizer, Object offendingSymbol, int line, int charPositionInLine, String msg, RecognitionException e) {
        // Handle syntax errors here
        // You can log the error or perform any other action
        // For example, you can use the prjId and filePath fields
        // to log additional information about the error location.
        // You can customize this method based on your requirements.

        // Example: Logging the error
        System.err.println("Syntax error at " + filePath + " line " + line + ":" + charPositionInLine + " - " + msg);
    }
}
```

In this example, the `ParserErrorListener` class extends `BaseErrorListener` and overrides the `syntaxError` method to handle syntax errors. You can customize this method to log or handle errors based on your specific needs.

Remember to update the instantiation of `ParserErrorListener` in your `CppMgr` class accordingly.

## 사용자 입력:
> ANTLR4로 만들라는게 아니라, ANTLR4의 ErrorListener처럼  어떤 클래스의  ErrorListener를 만들어 달라는 거야. 

## ChatGPT 응답:
> I see, my apologies for the confusion. If you want to create a custom error listener for your `CppMgr` class without using ANTLR4, you can define your own error listener interface and implement it. Here's an example:

```java
public interface MyErrorListener {
    void syntaxError(String filePath, int line, int charPositionInLine, String message);
}

public class CppMgr extends ParserImpl {

    private static final Logger logger = LogManager.getLogger(CppMgr.class);
    private static final String PARSER_ID = "P0031";
    private static final String PARSER_FILE_EXT = "CPP";

    // Custom error listener for CppMgr
    private MyErrorListener myErrorListener;

    // ... (rest of the code)

    public void setMyErrorListener(MyErrorListener errorListener) {
        this.myErrorListener = errorListener;
    }

    @Override
    public void parse(String prjIdPRMT, FileInfo foPRMT, RuleSet ruleSetPRMT, MultiValueMap uniqueOwnerInfoPRMT, HashMap<String, String> ffdColListPRMT, MultiKeyMap jobSchemaMPPRMT) throws Exception {
        // ... (existing code)

        try {
            // ... (existing code)

            CPP14Parser.TranslationUnitContext tree = parser.translationUnit();

            Reqlog.info("Cpp(C++) Parser START...");
            Cpp14ParsingListener p = new Cpp14ParsingListener(tokens, prjIdPRMT, foPRMT, ruleSetPRMT, uniqueOwnerInfoPRMT, ffdColListPRMT);
            ParseTreeWalker walker = new ParseTreeWalker();
            walker.walk(p, tree);

            analysisYn = ConstDef.YN_Y;
            Reqlog.info("Cpp(C++) Parser END...");
        } catch (Exception e) {
            // ... (existing code)

            // Notify the custom error listener about the syntax error
            if (myErrorListener != null) {
                myErrorListener.syntaxError(foPRMT.sFullPath, -1, -1, e.getMessage());
            }
        } finally {
            _makeFile(prjIdPRMT, foPRMT.nIndex, foPRMT, analysisYn);
        }
    }

    // ... (existing code)
}
```

Now, when you use the `CppMgr` class, you can set an instance of `MyErrorListener` using the `setMyErrorListener` method. This listener will be notified when a syntax error occurs, and you can customize its implementation based on your requirements.

