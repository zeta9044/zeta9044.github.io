---
layout: post
title: "Refactor and Streamline Code"
date: 2023-11-07
tags: [and, Code, Refactor, Streamline]
categories: blog
---

## 사용자 입력:
>        sl.forEach(s -> {
            List<CPP14Parser.PointerDeclaratorContext> pdList = RuleUtil.findAll(s, CPP14Parser.PointerDeclaratorContext.class);
            List<CPP14Parser.PointerMemberExpressionContext> pmList = RuleUtil.findAll(s, CPP14Parser.PointerMemberExpressionContext.class);

            pdList.removeIf(pd -> RuleUtil.hasRule(pd, CPP14Parser.PointerDeclaratorContext.class));
            pdList.removeIf(pd -> {
                CPP14Parser.InitializerContext i = RuleUtil.findRule(pd.getParent().getParent(), CPP14Parser.InitializerContext.class);
                boolean result = false;
                if (i != null && i.LeftParen() != null && i.RightParen() != null) {
                    result = true;
                }
                if (RuleUtil.hasRule(pd, CPP14Parser.ParametersAndQualifiersContext.class)) {
                    result = true;
                }

                return result;
            });
            for (CPP14Parser.PointerDeclaratorContext pd : pdList) {
                CPP14Parser.UnqualifiedIdContext var = RuleUtil.findRule(pd, CPP14Parser.UnqualifiedIdContext.class);
                if (var != null) {
                    varNames.add(var);
                    logger.debug("line:" + s.start.getLine()+",pd:" + var.getText());
                    callpdMap.put(s,pd);
                }
            }

            pmList.removeIf(pm -> RuleUtil.hasRule(pm, CPP14Parser.PointerMemberExpressionContext.class));
            pmList.removeIf(pm -> RuleUtil.hasRule(pm, CPP14Parser.LiteralContext.class));
            pmList.removeIf(pm -> {
                List<CPP14Parser.PostfixExpressionContext> peList = RuleUtil.findAll(pm, CPP14Parser.PostfixExpressionContext.class);
                boolean result = false;
                for (CPP14Parser.PostfixExpressionContext pe : peList) {
                    if (pe.postfixExpression() != null && pe.LeftParen() != null && pe.RightParen() != null) {
                        result = true;
                    }
                }
                return result;
            });
            pmList.removeIf(pm -> "NULL".equals(pm.getText()));
            for (CPP14Parser.PointerMemberExpressionContext pm : pmList) {
                CPP14Parser.UnqualifiedIdContext var = RuleUtil.findRule(pm, CPP14Parser.UnqualifiedIdContext.class);
                if (var != null) {
                    varNames.add(var);
                    logger.debug("line:" + s.start.getLine()+",pm:" + var.getText());
                    callpmMap.put(s,pm);
                }
            }
        });

        el.forEach(s -> {
            List<CPP14Parser.PointerDeclaratorContext> pdList = RuleUtil.findAll(s, CPP14Parser.PointerDeclaratorContext.class);
            List<CPP14Parser.PointerMemberExpressionContext> pmList = RuleUtil.findAll(s, CPP14Parser.PointerMemberExpressionContext.class);

            pdList.removeIf(pd -> RuleUtil.hasRule(pd, CPP14Parser.PointerDeclaratorContext.class));
            pdList.removeIf(pd -> {
                CPP14Parser.InitializerContext i = RuleUtil.findRule(pd.getParent().getParent(), CPP14Parser.InitializerContext.class);
                boolean result = false;
                if (i != null && i.LeftParen() != null && i.RightParen() != null) {
                    result = true;
                }
                if (RuleUtil.hasRule(pd, CPP14Parser.ParametersAndQualifiersContext.class)) {
                    result = true;
                }

                return result;
            });
            for (CPP14Parser.PointerDeclaratorContext pd : pdList) {
                CPP14Parser.UnqualifiedIdContext var = RuleUtil.findRule(pd, CPP14Parser.UnqualifiedIdContext.class);
                if (var != null) {
                    varNames.add(var);
                    logger.debug("line:" + s.start.getLine()+",pd:" + var.getText());
                    callpdMap.put(s,pd);
                }
            }

            pmList.removeIf(pm -> RuleUtil.hasRule(pm, CPP14Parser.PointerMemberExpressionContext.class));
            pmList.removeIf(pm -> RuleUtil.hasRule(pm, CPP14Parser.LiteralContext.class));
            pmList.removeIf(pm -> {
                List<CPP14Parser.PostfixExpressionContext> peList = RuleUtil.findAll(pm, CPP14Parser.PostfixExpressionContext.class);
                boolean result = false;
                for (CPP14Parser.PostfixExpressionContext pe : peList) {
                    if (pe.postfixExpression() != null && pe.LeftParen() != null && pe.RightParen() != null) {
                        result = true;
                    }
                }
                return result;
            });
            pmList.removeIf(pm -> "NULL".equals(pm.getText()));
            for (CPP14Parser.PointerMemberExpressionContext pm : pmList) {
                CPP14Parser.UnqualifiedIdContext var = RuleUtil.findRule(pm, CPP14Parser.UnqualifiedIdContext.class);
                if (var != null) {
                    varNames.add(var);
                    logger.debug("line:" + s.start.getLine()+",pm:" + var.getText());
                    callpmMap.put(s,pm);
                }
            }
        });
중복제거해봐.

## ChatGPT 응답:
> The provided code snippets iterate over two collections `sl` and `el`, performing very similar operations on both. The code appears to be processing some kind of C++ code analysis using a parser for C++14. The goal seems to be to collect information about pointer declarators and pointer member expressions, remove certain ones based on various conditions, and then log and store information about the ones that remain.

To eliminate duplication, we can create a separate method that performs the common operations and call this method with each collection. Here's how you might refactor the code:

```java
public void processItems(List<?> items) {
    List<CPP14Parser.PointerDeclaratorContext> pdList = RuleUtil.findAll(items, CPP14Parser.PointerDeclaratorContext.class);
    List<CPP14Parser.PointerMemberExpressionContext> pmList = RuleUtil.findAll(items, CPP14Parser.PointerMemberExpressionContext.class);

    processPointerDeclarators(pdList, items);
    processPointerMemberExpressions(pmList, items);
}

private void processPointerDeclarators(List<CPP14Parser.PointerDeclaratorContext> pdList, List<?> items) {
    pdList.removeIf(pd -> RuleUtil.hasRule(pd, CPP14Parser.PointerDeclaratorContext.class));
    pdList.removeIf(pd -> checkPointerDeclarator(pd));
    pdList.forEach(pd -> logAndStorePointerDeclarator(pd, items));
}

private void processPointerMemberExpressions(List<CPP14Parser.PointerMemberExpressionContext> pmList, List<?> items) {
    pmList.removeIf(pm -> RuleUtil.hasRule(pm, CPP14Parser.PointerMemberExpressionContext.class));
    pmList.removeIf(pm -> RuleUtil.hasRule(pm, CPP14Parser.LiteralContext.class));
    pmList.removeIf(pm -> checkPointerMemberExpression(pm));
    pmList.removeIf(pm -> "NULL".equals(pm.getText()));
    pmList.forEach(pm -> logAndStorePointerMemberExpression(pm, items));
}

private boolean checkPointerDeclarator(CPP14Parser.PointerDeclaratorContext pd) {
    CPP14Parser.InitializerContext i = RuleUtil.findRule(pd.getParent().getParent(), CPP14Parser.InitializerContext.class);
    boolean result = false;
    if (i != null && i.LeftParen() != null && i.RightParen() != null) {
        result = true;
    }
    if (RuleUtil.hasRule(pd, CPP14Parser.ParametersAndQualifiersContext.class)) {
        result = true;
    }
    return result;
}

private void logAndStorePointerDeclarator(CPP14Parser.PointerDeclaratorContext pd, List<?> items) {
    CPP14Parser.UnqualifiedIdContext var = RuleUtil.findRule(pd, CPP14Parser.UnqualifiedIdContext.class);
    if (var != null) {
        varNames.add(var);
        logger.debug("line:" + ((RuleContext) items).start.getLine() + ",pd:" + var.getText());
        callpdMap.put(items, pd);
    }
}

private boolean checkPointerMemberExpression(CPP14Parser.PointerMemberExpressionContext pm) {
    List<CPP14Parser.PostfixExpressionContext> peList = RuleUtil.findAll(pm, CPP14Parser.PostfixExpressionContext.class);
    for (CPP14Parser.PostfixExpressionContext pe : peList) {
        if (pe.postfixExpression() != null && pe.LeftParen() != null && pe.RightParen() != null) {
            return true;
        }
    }
    return false;
}

private void logAndStorePointerMemberExpression(CPP14Parser.PointerMemberExpressionContext pm, List<?> items) {
    CPP14Parser.UnqualifiedIdContext var = RuleUtil.findRule(pm, CPP14Parser.UnqualifiedIdContext.class);
    if (var != null) {
        varNames.add(var);
        logger.debug("line:" + ((RuleContext) items).start.getLine() + ",pm:" + var.getText());
        callpmMap.put(items, pm);
    }
}

// Then call processItems with your lists:
processItems(sl);
processItems(el);
```

I've created several helper methods:
- `processItems` which accepts a list and processes it.
- `processPointerDeclarators` for the common operations on `PointerDeclaratorContext` objects.
- `processPointerMemberExpressions` for the common operations on `PointerMemberExpressionContext` objects.
- `checkPointerDeclarator` to encapsulate the complex condition for removing pointer declarators.
- `logAndStorePointerDeclarator` to encapsulate the logic of logging and storing pointer declarators.
- `checkPointerMemberExpression` to encapsulate the condition for removing pointer member expressions.
- `logAndStorePointerMemberExpression` to encapsulate the logic of logging and storing pointer member expressions.

Make sure to define `varNames`, `logger`, and `callpdMap` appropriately, as these are used in the helper methods and their definitions are not included in the original snippet. Also, replace the casts and method calls according to the actual types of your `sl`

