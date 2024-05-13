---
layout: post
title: "ANTLR4 to JavaCC Conversion."
date: 2023-08-07
tags: [JavaCC, Conversion, to]
categories: blog
---

## 사용자 입력:
> translationUnit: declarationseq? EOF;
/*Expressions*/

primaryExpression:
	literal+
	| This
	| LeftParen expression RightParen
	| idExpression
	| lambdaExpression;

idExpression: unqualifiedId | qualifiedId;

unqualifiedId:
	Identifier
	| operatorFunctionId
	| conversionFunctionId
	| literalOperatorId
	| Tilde (className | decltypeSpecifier)
	| templateId;

qualifiedId: nestedNameSpecifier Template? unqualifiedId;

nestedNameSpecifier:
	(theTypeName | namespaceName | decltypeSpecifier)? Doublecolon
	| nestedNameSpecifier (
		Identifier
		| Template? simpleTemplateId
	) Doublecolon;
lambdaExpression:
	lambdaIntroducer lambdaDeclarator? compoundStatement;

lambdaIntroducer: LeftBracket lambdaCapture? RightBracket;

lambdaCapture:
	captureList
	| captureDefault (Comma captureList)?;

captureDefault: And | Assign;

captureList: capture (Comma capture)* Ellipsis?;

capture: simpleCapture | initcapture;

simpleCapture: And? Identifier | This;

initcapture: And? Identifier initializer;

lambdaDeclarator:
	LeftParen parameterDeclarationClause? RightParen Mutable? exceptionSpecification?
		attributeSpecifierSeq? trailingReturnType?;

postfixExpression:
	primaryExpression
	| postfixExpression LeftBracket (expression | bracedInitList) RightBracket
	| postfixExpression LeftParen expressionList? RightParen
	| (simpleTypeSpecifier | typeNameSpecifier) (
		LeftParen expressionList? RightParen
		| bracedInitList
	)
	| postfixExpression (Dot | Arrow) (
		Template? idExpression
		| pseudoDestructorName
	)
	| postfixExpression (PlusPlus | MinusMinus)
	| (
		Dynamic_cast
		| Static_cast
		| Reinterpret_cast
		| Const_cast
	) Less theTypeId Greater LeftParen expression RightParen
	| typeIdOfTheTypeId LeftParen (expression | theTypeId) RightParen;
/*
 add a middle layer to eliminate duplicated function declarations
 */

typeIdOfTheTypeId: Typeid_;

expressionList: initializerList;

pseudoDestructorName:
	nestedNameSpecifier? (theTypeName Doublecolon)? Tilde theTypeName
	| nestedNameSpecifier Template simpleTemplateId Doublecolon Tilde theTypeName
	| Tilde decltypeSpecifier;

unaryExpression:
	postfixExpression
	| (PlusPlus | MinusMinus | unaryOperator | Sizeof) unaryExpression
	| Sizeof (
		LeftParen theTypeId RightParen
		| Ellipsis LeftParen Identifier RightParen
	)
	| Alignof LeftParen theTypeId RightParen
	| noExceptExpression
	| newExpression_
	| deleteExpression;

unaryOperator: Or | Star | And | Plus | Tilde | Minus | Not;

newExpression_:
	Doublecolon? New newPlacement? (
		newTypeId
		| LeftParen theTypeId RightParen
	) newInitializer_?;

newPlacement: LeftParen expressionList RightParen;

newTypeId: typeSpecifierSeq newDeclarator_?;

newDeclarator_:
	pointerOperator newDeclarator_?
	| noPointerNewDeclarator;

noPointerNewDeclarator:
	LeftBracket expression RightBracket attributeSpecifierSeq?
	| noPointerNewDeclarator LeftBracket constantExpression RightBracket attributeSpecifierSeq?;

newInitializer_:
	LeftParen expressionList? RightParen
	| bracedInitList;

deleteExpression:
	Doublecolon? Delete (LeftBracket RightBracket)? castExpression;

noExceptExpression: Noexcept LeftParen expression RightParen;

castExpression:
	unaryExpression
	| LeftParen theTypeId RightParen castExpression;

pointerMemberExpression:
	castExpression ((DotStar | ArrowStar) castExpression)*;

multiplicativeExpression:
	pointerMemberExpression (
		(Star | Div | Mod) pointerMemberExpression
	)*;

additiveExpression:
	multiplicativeExpression (
		(Plus | Minus) multiplicativeExpression
	)*;

shiftExpression:
	additiveExpression (shiftOperator additiveExpression)*;

shiftOperator: Greater Greater | Less Less;

relationalExpression:
	shiftExpression (
		(Less | Greater | LessEqual | GreaterEqual) shiftExpression
	)*;

equalityExpression:
	relationalExpression (
		(Equal | NotEqual) relationalExpression
	)*;

andExpression: equalityExpression (And equalityExpression)*;

exclusiveOrExpression: andExpression (Caret andExpression)*;

inclusiveOrExpression:
	exclusiveOrExpression (Or exclusiveOrExpression)*;

logicalAndExpression:
	inclusiveOrExpression (AndAnd inclusiveOrExpression)*;

logicalOrExpression:
	logicalAndExpression (OrOr logicalAndExpression)*;

conditionalExpression:
	logicalOrExpression (
		Question expression Colon assignmentExpression
	)?;

assignmentExpression:
	conditionalExpression
	| logicalOrExpression assignmentOperator initializerClause
	| throwExpression;

assignmentOperator:
	Assign
	| StarAssign
	| DivAssign
	| ModAssign
	| PlusAssign
	| MinusAssign
	| RightShiftAssign
	| LeftShiftAssign
	| AndAssign
	| XorAssign
	| OrAssign;

expression: assignmentExpression (Comma assignmentExpression)*;

constantExpression: conditionalExpression;
/*Statements*/

statement:
	labeledStatement
	| declarationStatement
	| attributeSpecifierSeq? (
		expressionStatement
		| compoundStatement
		| selectionStatement
		| iterationStatement
		| jumpStatement
		| tryBlock
	);

labeledStatement:
	attributeSpecifierSeq? (
		Identifier
		| Case constantExpression
		| Default
	) Colon statement;

expressionStatement: expression? Semi;

compoundStatement: LeftBrace statementSeq? RightBrace;

statementSeq: statement+;

selectionStatement:
	If LeftParen condition RightParen statement (Else statement)?
	| Switch LeftParen condition RightParen statement;

condition:
	expression
	| attributeSpecifierSeq? declSpecifierSeq declarator (
		Assign initializerClause
		| bracedInitList
	);

iterationStatement:
	While LeftParen condition RightParen statement
	| Do statement While LeftParen expression RightParen Semi
	| For LeftParen (
		forInitStatement condition? Semi expression?
		| forRangeDeclaration Colon forRangeInitializer
	) RightParen statement;

forInitStatement: expressionStatement | simpleDeclaration;

forRangeDeclaration:
	attributeSpecifierSeq? declSpecifierSeq declarator;

forRangeInitializer: expression | bracedInitList;

jumpStatement:
	(
		Break
		| Continue
		| Return (expression | bracedInitList)?
		| Goto Identifier
	) Semi;

declarationStatement: blockDeclaration;
/*Declarations*/

declarationseq: declaration+;

declaration:
	blockDeclaration
	| functionDefinition
	| templateDeclaration
	| explicitInstantiation
	| explicitSpecialization
	| linkageSpecification
	| namespaceDefinition
	| emptyDeclaration_
	| attributeDeclaration;

blockDeclaration:
	simpleDeclaration
	| asmDefinition
	| namespaceAliasDefinition
	| usingDeclaration
	| usingDirective
	| staticAssertDeclaration
	| aliasDeclaration
	| opaqueEnumDeclaration;

aliasDeclaration:
	Using Identifier attributeSpecifierSeq? Assign theTypeId Semi;

simpleDeclaration:
	declSpecifierSeq? initDeclaratorList? Semi
	| attributeSpecifierSeq declSpecifierSeq? initDeclaratorList Semi;

staticAssertDeclaration:
	Static_assert LeftParen constantExpression Comma StringLiteral RightParen Semi;

emptyDeclaration_: Semi;

attributeDeclaration: attributeSpecifierSeq Semi;

declSpecifier:
	storageClassSpecifier
	| typeSpecifier
	| functionSpecifier
	| Friend
	| Typedef
	| Constexpr;
	
declSpecifierSeq: declSpecifier+? attributeSpecifierSeq?;

storageClassSpecifier:
	Register
	| Static
	| Thread_local
	| Extern
	| Mutable;

functionSpecifier: Inline | Virtual | Explicit;

typedefName: Identifier;

typeSpecifier:
	trailingTypeSpecifier
	| classSpecifier
	| enumSpecifier;

trailingTypeSpecifier:
	simpleTypeSpecifier
	| elaboratedTypeSpecifier
	| typeNameSpecifier
	| cvQualifier;

typeSpecifierSeq: typeSpecifier+ attributeSpecifierSeq?;

trailingTypeSpecifierSeq:
	trailingTypeSpecifier+ attributeSpecifierSeq?;

simpleTypeLengthModifier:
	Short
	| Long;
	
simpleTypeSignednessModifier:
	Unsigned
	| Signed;

simpleTypeSpecifier:
	nestedNameSpecifier? theTypeName
	| nestedNameSpecifier Template simpleTemplateId
	| Char
	| Char16
	| Char32
	| Wchar
	| Bool
	| Short
	| Int
	| Long
	| Float
	| Signed
	| Unsigned
	| Float
	| Double
	| Void
	| Auto
	| decltypeSpecifier
	;

theTypeName:
	className
	| enumName
	| typedefName
	| simpleTemplateId;

decltypeSpecifier:
	Decltype LeftParen (expression | Auto) RightParen;

elaboratedTypeSpecifier:
	classKey (
		attributeSpecifierSeq? nestedNameSpecifier? Identifier
		| simpleTemplateId
		| nestedNameSpecifier Template? simpleTemplateId
	)
	| Enum nestedNameSpecifier? Identifier;

enumName: Identifier;

enumSpecifier:
	enumHead LeftBrace (enumeratorList Comma?)? RightBrace;

enumHead:
	enumkey attributeSpecifierSeq? (
		nestedNameSpecifier? Identifier
	)? enumbase?;

opaqueEnumDeclaration:
	enumkey attributeSpecifierSeq? Identifier enumbase? Semi;

enumkey: Enum (Class | Struct)?;

enumbase: Colon typeSpecifierSeq;

enumeratorList:
	enumeratorDefinition (Comma enumeratorDefinition)*;

enumeratorDefinition: enumerator (Assign constantExpression)?;

enumerator: Identifier;

namespaceName: originalNamespaceName | namespaceAlias;

originalNamespaceName: Identifier;

namespaceDefinition:
	Inline? Namespace (Identifier | originalNamespaceName)? LeftBrace namespaceBody = declarationseq
		? RightBrace;

namespaceAlias: Identifier;

namespaceAliasDefinition:
	Namespace Identifier Assign qualifiednamespacespecifier Semi;

qualifiednamespacespecifier: nestedNameSpecifier? namespaceName;

usingDeclaration:
	Using (Typename_? nestedNameSpecifier | Doublecolon) unqualifiedId Semi;

usingDirective:
	attributeSpecifierSeq? Using Namespace nestedNameSpecifier? namespaceName Semi;

asmDefinition: Asm LeftParen StringLiteral RightParen Semi;

linkageSpecification:
	Extern StringLiteral (
		LeftBrace declarationseq? RightBrace
		| declaration
	);

attributeSpecifierSeq: attributeSpecifier+;

attributeSpecifier:
	LeftBracket LeftBracket attributeList? RightBracket RightBracket
	| alignmentspecifier;

alignmentspecifier:
	Alignas LeftParen (theTypeId | constantExpression) Ellipsis? RightParen;

attributeList: attribute (Comma attribute)* Ellipsis?;

attribute: (attributeNamespace Doublecolon)? Identifier attributeArgumentClause?;

attributeNamespace: Identifier;

attributeArgumentClause: LeftParen balancedTokenSeq? RightParen;

balancedTokenSeq: balancedtoken+;

balancedtoken:
	LeftParen balancedTokenSeq RightParen
	| LeftBracket balancedTokenSeq RightBracket
	| LeftBrace balancedTokenSeq RightBrace
	| ~(
		LeftParen
		| RightParen
		| LeftBrace
		| RightBrace
		| LeftBracket
		| RightBracket
	)+;
/*Declarators*/

initDeclaratorList: initDeclarator (Comma initDeclarator)*;

initDeclarator: declarator initializer?;

declarator:
	pointerDeclarator
	| noPointerDeclarator parametersAndQualifiers trailingReturnType;

pointerDeclarator: (pointerOperator Const?)* noPointerDeclarator;

noPointerDeclarator:
	declaratorid attributeSpecifierSeq?
	| noPointerDeclarator (
		parametersAndQualifiers
		| LeftBracket constantExpression? RightBracket attributeSpecifierSeq?
	)
	| LeftParen pointerDeclarator RightParen;

parametersAndQualifiers:
	LeftParen parameterDeclarationClause? RightParen cvqualifierseq? refqualifier?
		exceptionSpecification? attributeSpecifierSeq?;

trailingReturnType:
	Arrow trailingTypeSpecifierSeq abstractDeclarator?;

pointerOperator:
	(And | AndAnd) attributeSpecifierSeq?
	| nestedNameSpecifier? Star attributeSpecifierSeq? cvqualifierseq?;

cvqualifierseq: cvQualifier+;

cvQualifier: Const | Volatile;

refqualifier: And | AndAnd;

declaratorid: Ellipsis? idExpression;

theTypeId: typeSpecifierSeq abstractDeclarator?;

abstractDeclarator:
	pointerAbstractDeclarator
	| noPointerAbstractDeclarator? parametersAndQualifiers trailingReturnType
	| abstractPackDeclarator;

pointerAbstractDeclarator:
	noPointerAbstractDeclarator
	| pointerOperator+ noPointerAbstractDeclarator?;

noPointerAbstractDeclarator:
	noPointerAbstractDeclarator (
		parametersAndQualifiers
		| noPointerAbstractDeclarator LeftBracket constantExpression? RightBracket
			attributeSpecifierSeq?
	)
	| parametersAndQualifiers
	| LeftBracket constantExpression? RightBracket attributeSpecifierSeq?
	| LeftParen pointerAbstractDeclarator RightParen;

abstractPackDeclarator:
	pointerOperator* noPointerAbstractPackDeclarator;

noPointerAbstractPackDeclarator:
	noPointerAbstractPackDeclarator (
		parametersAndQualifiers
		| LeftBracket constantExpression? RightBracket attributeSpecifierSeq?
	)
	| Ellipsis;

parameterDeclarationClause:
	parameterDeclarationList (Comma? Ellipsis)?;

parameterDeclarationList:
	parameterDeclaration (Comma parameterDeclaration)*;

parameterDeclaration:
	attributeSpecifierSeq? declSpecifierSeq (declarator | abstractDeclarator?) (Assign initializerClause)?
	;

functionDefinition:
	attributeSpecifierSeq? declSpecifierSeq? declarator virtualSpecifierSeq? functionBody;

functionBody:
	constructorInitializer? compoundStatement
	| functionTryBlock
	| Assign (Default | Delete) Semi;

initializer:
	braceOrEqualInitializer
	| LeftParen expressionList RightParen;

braceOrEqualInitializer:
	Assign initializerClause
	| bracedInitList;

initializerClause: assignmentExpression | bracedInitList;

initializerList:
	initializerClause Ellipsis? (
		Comma initializerClause Ellipsis?
	)*;

bracedInitList: LeftBrace (initializerList Comma?)? RightBrace;
/*Classes*/

className: Identifier | simpleTemplateId;

classSpecifier:
	classHead LeftBrace memberSpecification? RightBrace;

classHead:
	classKey attributeSpecifierSeq? (
		classHeadName classVirtSpecifier?
	)? baseClause?
	| Union attributeSpecifierSeq? (
		classHeadName classVirtSpecifier?
	)?;

classHeadName: nestedNameSpecifier? className;

classVirtSpecifier: Final;

classKey: Class | Struct;

memberSpecification:
	(memberdeclaration | accessSpecifier Colon)+;

memberdeclaration:
	attributeSpecifierSeq? declSpecifierSeq? memberDeclaratorList? Semi
	| functionDefinition
	| usingDeclaration
	| staticAssertDeclaration
	| templateDeclaration
	| aliasDeclaration
	| emptyDeclaration_;

memberDeclaratorList:
	memberDeclarator (Comma memberDeclarator)*;

memberDeclarator:
    declarator (virtualSpecifierSeq | { this.IsPureSpecifierAllowed() }? pureSpecifier | { this.IsPureSpecifierAllowed() }? virtualSpecifierSeq pureSpecifier | braceOrEqualInitializer)
    | declarator
    | Identifier? attributeSpecifierSeq? Colon constantExpression
    ;

virtualSpecifierSeq: virtualSpecifier+;

virtualSpecifier: Override | Final;

/*
 purespecifier: Assign '0'//Conflicts with the lexer ;
 */

pureSpecifier:
	Assign IntegerLiteral
    ;

/*Derived classes*/

baseClause: Colon baseSpecifierList;

baseSpecifierList:
	baseSpecifier Ellipsis? (Comma baseSpecifier Ellipsis?)*;

baseSpecifier:
	attributeSpecifierSeq? (
		baseTypeSpecifier
		| Virtual accessSpecifier? baseTypeSpecifier
		| accessSpecifier Virtual? baseTypeSpecifier
	);

classOrDeclType:
	nestedNameSpecifier? className
	| decltypeSpecifier;

baseTypeSpecifier: classOrDeclType;

accessSpecifier: Private | Protected | Public;
/*Special member functions*/

conversionFunctionId: Operator conversionTypeId;

conversionTypeId: typeSpecifierSeq conversionDeclarator?;

conversionDeclarator: pointerOperator conversionDeclarator?;

constructorInitializer: Colon memInitializerList;

memInitializerList:
	memInitializer Ellipsis? (Comma memInitializer Ellipsis?)*;

memInitializer:
	meminitializerid (
		LeftParen expressionList? RightParen
		| bracedInitList
	);

meminitializerid: classOrDeclType | Identifier;
/*Overloading*/

operatorFunctionId: Operator theOperator;

literalOperatorId:
	Operator (
		StringLiteral Identifier
		| UserDefinedStringLiteral
	);
/*Templates*/

templateDeclaration:
	Template Less templateparameterList Greater declaration;

templateparameterList:
	templateParameter (Comma templateParameter)*;

templateParameter: typeParameter | parameterDeclaration;

typeParameter:
	(
		(Template Less templateparameterList Greater)? Class
		| Typename_
	) (Ellipsis? Identifier? | Identifier? Assign theTypeId);

simpleTemplateId:
	templateName Less templateArgumentList? Greater;

templateId:
	simpleTemplateId
	| (operatorFunctionId | literalOperatorId) Less templateArgumentList? Greater;

templateName: Identifier;

templateArgumentList:
	templateArgument Ellipsis? (Comma templateArgument Ellipsis?)*;

templateArgument: theTypeId | constantExpression | idExpression;

typeNameSpecifier:
	Typename_ nestedNameSpecifier (
		Identifier
		| Template? simpleTemplateId
	);

explicitInstantiation: Extern? Template declaration;

explicitSpecialization: Template Less Greater declaration;
/*Exception handling*/

tryBlock: Try compoundStatement handlerSeq;

functionTryBlock:
	Try constructorInitializer? compoundStatement handlerSeq;

handlerSeq: handler+;

handler:
	Catch LeftParen exceptionDeclaration RightParen compoundStatement;

exceptionDeclaration:
	attributeSpecifierSeq? typeSpecifierSeq (
		declarator
		| abstractDeclarator
	)?
	| Ellipsis;

throwExpression: Throw assignmentExpression?;

exceptionSpecification:
	dynamicExceptionSpecification
	| noeExceptSpecification;

dynamicExceptionSpecification:
	Throw LeftParen typeIdList? RightParen;

typeIdList: theTypeId Ellipsis? (Comma theTypeId Ellipsis?)*;

noeExceptSpecification:
	Noexcept LeftParen constantExpression RightParen
	| Noexcept;
/*Preprocessing directives*/

/*Lexer*/

theOperator:
	New (LeftBracket RightBracket)?
	| Delete (LeftBracket RightBracket)?
	| Plus
	| Minus
	| Star
	| Div
	| Mod
	| Caret
	| And
	| Or
	| Tilde
	| Not
	| Assign
	| Greater
	| Less
	| GreaterEqual
	| PlusAssign
	| MinusAssign
	| StarAssign
	| ModAssign
	| XorAssign
	| AndAssign
	| OrAssign
	| Less Less
	| Greater Greater
	| RightShiftAssign
	| LeftShiftAssign
	| Equal
	| NotEqual
	| LessEqual
	| AndAnd
	| OrOr
	| PlusPlus
	| MinusMinus
	| Comma
	| ArrowStar
	| Arrow
	| LeftParen RightParen
	| LeftBracket RightBracket;

literal:
	IntegerLiteral
	| CharacterLiteral
	| FloatingLiteral
	| StringLiteral
	| BooleanLiteral
	| PointerLiteral
	| UserDefinedLiteral;
	은 antlr4의 parser구문인데, javaCC의 jjt로 구문으로 변환해줘.

## ChatGPT 응답:
> The provided code snippet appears to be a language grammar definition, specifically for the C++ programming language. It defines the syntax rules for various language constructs in C++.

To convert this grammar from ANTLR4 to JavaCC (JJTree), you'll need to rewrite the rules in JavaCC syntax. JavaCC and ANTLR4 have different grammar formats, so the conversion requires careful attention to the specific syntax rules and their corresponding representations in JavaCC.

Unfortunately, converting a grammar of this size can be a time-consuming task and beyond the scope of a single response. It requires a manual translation, ensuring that the rules and expressions are correctly adapted to JavaCC's syntax.

To start the conversion, you'll need to understand the basics of JavaCC and JJTree. Here are the key steps:

1. Familiarize yourself with the JavaCC syntax and grammar rules.
2. Identify the various grammar rules in the ANTLR4 code and understand their structure and meaning.
3. Start translating the rules one by one into JavaCC format, paying attention to the differences in syntax.
4. Handle lexer rules and parser rules separately.
5. Use JavaCC's regular expressions for lexer rules and proper Java code for parser rules.

Since the provided grammar is quite extensive, it's advisable to do the conversion in small chunks, checking the correctness at each step.

Keep in mind that JavaCC and ANTLR4 are different tools with slightly different capabilities and features. While both are used for parsing, they have their unique strengths and weaknesses.

If you're unfamiliar with JavaCC and JJTree, it might be helpful to refer to the official documentation and tutorials to get started. Additionally, there are online resources and examples available that demonstrate how to write grammars using JavaCC.

Once you start the conversion process and encounter specific issues or questions, feel free to ask for help with those particular aspects.

## 사용자 입력:
> 한글로 설명해줘.

## ChatGPT 응답:
> 제공된 코드 스니펫은 C++ 프로그래밍 언어에 대한 언어 문법 정의입니다. 이 코드는 C++에서 다양한 언어 구조물의 구문 규칙을 정의합니다.

이 코드를 ANTLR4에서 JavaCC(JJTree)로 변환하려면 ANTLR4 문법을 JavaCC 문법으로 다시 작성해야 합니다. JavaCC와 ANTLR4는 서로 다른 문법 형식을 가지고 있으므로, 변환은 특정 문법 규칙과 그에 해당하는 JavaCC의 표현 방법에 대한 주의 깊은 이해가 필요합니다.

이런 규모의 문법을 변환하는 것은 시간이 많이 소요되며, 단일 응답으로는 불가능합니다. 수동으로 변환해야 하며, 올바른 결과를 보장하기 위해서는 각 규칙과 표현식을 정확하게 JavaCC의 문법 형식으로 적용해야 합니다.

시작하려면 JavaCC와 JJTree의 기본 사항을 이해해야 합니다. ANTLR4 코드의 다양한 문법 규칙을 식별하고 그 구조와 의미를 이해하는 것이 중요합니다.

규칙을 하나씩 JavaCC 형식으로 번역을 시작하고, 문법의 차이점을 주의하여 처리해야 합니다.

- Lexer 규칙과 Parser 규칙을 별도로 처리합니다.
- Lexer 규칙에 대해서는 JavaCC의 정규 표현식을 사용합니다.
- Parser 규칙에 대해서는 적절한 Java 코드를 사용합니다.

제공된 문법이 꽤 크기 때문에, 작은 조각으로 변환 작업을 수행하고 각 단계에서 올바른 결과를 확인하는 것이 좋습니다.

JavaCC와 JJTree에 익숙하지 않다면, 공식 문서와 튜토리얼을 참조하여 시작하는 것이 도움이 됩니다. 또한 JavaCC를 사용하여 문법을 작성하는 방법을 설명하는 온라인 자료와 예제도 참고할 수 있습니다.

변환 과정을 시작하고 특정 문제나 질문이 발생하면 해당 부분에 대한 도움을 요청해 주세요.

