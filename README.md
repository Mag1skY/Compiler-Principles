# Compiler-Principles
词法分析 语法分析 中间代码生成 代码生成

------------------------------------------------------------------------------------------------------------------------------------------------------
2024词法分析作业题目
【问题描述】

请根据给定的文法设计并实现词法分析程序，从源程序中识别出单词，记录其单词类别和单词值，输入输出及处理要求如下：

   （1）数据结构和与语法分析程序的接口请自行定义；类别码需按下表格式统一定义；

   （2）你的词法分析程序需要将注释去掉，注释有两种：单行注释和多行注释，风格与C语言相同。

   （3）Ident为标识符，以字母或下划线开头，由字母、下划线、数字组成的串

   （4）IntConst为整型常量，仅包含10进制无符号整数

   （5）FormatString是用于printf中的格式化字符串，特殊字符仅包含%d和\n，例如"m=%d\n"

   （6）为了方便进行自动评测，输入的被编译源文件统一命名为 testfile.txt（注意不要写错文件名）；输出的结果文件统一命名为 output.txt（注意不要写错文件名），结果文件中每行按如下方式组织：

                单词类别码 单词的字符/字符串形式(中间仅用一个空格间隔)

                单词的类别码请统一按如下形式定义：

词法分析类别码定义如下：

单词名称	类别码	单词名称	类别码	单词名称	类别码	单词名称	类别码
Ident	IDENFR	!	NOT	*	MULT	=	ASSIGN
IntConst	INTCON	&&	AND	/	DIV	;	SEMICN
FormatString	STRCON	||	OR	%	MOD	,	COMMA
main	MAINTK	while	WHILETK	<	LSS	(	LPARENT
const	CONSTTK	getint	GETINTTK	<=	LEQ	)	RPARENT
int	INTTK	printf	PRINTFTK	>	GRE	[	LBRACK
break	BREAKTK	return	RETURNTK	>=	GEQ	]	RBRACK
continue	CONTINUETK	+	PLUS	==	EQL	{	LBRACE
if	IFTK	-	MINU	!=	NEQ	}	RBRACE
else	ELSETK	void	VOIDTK	



【输入形式】testfile.txt中的符合文法要求的测试程序。
【输出形式】要求将词法分析结果输出至output.txt中。

【特别提醒】  （1）读取的字符串要原样保留着便于输出，特别是数字，这里输出的并不是真正的单词值，其实是读入的字符串，单词值需另行记录。

（2）本次作业只考核对正确程序的处理，但需要为今后可能出现的错误情况预留接口。

（3）在今后的错误处理作业中，需要输出错误的行号，在词法分析的时候注意记录该信息。

（4）单词的类别和单词值以及其他关注的信息，在词法分析阶段获取后，后续的分析阶段会使用，请注意记录；当前要求的输出只是为了便于评测，完成编译器中无需出现这些信息，请设计为方便打开/关闭这些输出的方案。

------------------------------------------------------------------------------------------------------------------------------------------------------

2023语法分析作业题目-B（LR分析）
特别注意：第17条产生式改为

17) Stmt → 'while' '(' Cond ')' Stmt



【问题描述】

本次作业只测试一个含简单变量声明、赋值语句、输出语句、if语句和while语句的文法：


0)  CompUnit → Block

1)  Block → '{' BlockItemList '}'

2)  BlockItemList →  BlockItemList BlockItem 

3)  BlockItemList →  EPSILON

4)  BlockItem → VarDecl

5)  BlockItem → Stmt

6)  VarDecl → 'int' VarDeclList ';' 

7)  VarDeclList → VarDeclList  ',' VarDef

8)  VarDeclList → VarDef

9)  VarDef → Ident

10) VarDef → Ident '=' Exp

11) Stmt → Ident '=' Exp ';' 

12) Stmt → Exp ';'

13) Stmt → ';'

14) Stmt → Block

15) Stmt → 'if' '(' Cond ')' Block

16) Stmt → 'if' '(' Cond ')' Block 'else' Block

17) Stmt → 'while' '(' Cond ')' Stmt

18) Stmt → Ident '=' 'getint''('')'';'

19) Stmt → 'printf''('FormatString ExpList ')'';' 

20) ExpList → ExpList ',' Exp

21) ExpList → EPSILON

22) Exp → AddExp 

23) Cond → LOrExp 

24) PrimaryExp → '(' Exp ')'

25) PrimaryExp → Ident

26) PrimaryExp → IntConst 

27) UnaryExp → PrimaryExp

28) MulExp → UnaryExp 

29) MulExp → MulExp '*' UnaryExp

30) MulExp → MulExp '/' UnaryExp 

31) MulExp → MulExp '%' UnaryExp 

32) AddExp → MulExp

33) AddExp → AddExp '+' MulExp

34) AddExp → AddExp '-' MulExp 

35) RelExp → AddExp

36) RelExp → RelExp '<' AddExp

37) RelExp → RelExp '>' AddExp 

38) RelExp → RelExp '<=' AddExp 

39) RelExp → RelExp '>=' AddExp  

40) EqExp → RelExp

41) EqExp → EqExp '==' RelExp 

42) EqExp → EqExp '!=' RelExp 

43) LAndExp → EqExp

44) LAndExp → LAndExp '&&' EqExp 

45) LOrExp → LAndExp

46) LOrExp → LOrExp '||' LAndExp

其对应的LR1分析表共含有170个状态。

请根据该文法设计并实现LR语法分析程序，能基于上次作业的词法分析程序所识别出的单词，识别出各类语法成分。输入输出及处理要求如下：

（1）需按文法规则，用LR分析法法对文法中定义的语法成分进行分析（需要使用上次作业中的词法分析程序）；

（2）本项作业对应的文法已经在参考代码CFGBlock.java中实现。当然你也可以重新设计文法的数据结构

（3）本项作业需要根据所学知识自动生成LR1分析表，参考代码中给出了分析表的数据结构，你也可以自行设计。

（4）为了方便进行自动评测，输入的被编译源文件统一命名为testfile.txt（注意不要写错文件名）；输出的结果文件统一命名为output.txt（注意不要写错文件名）；结果文件中包含如下两种信息：

    1）按词法分析识别单词的顺序，按行输出每个单词的信息（要求同词法分析作业，对于预读的情况不能输出）。

    2）在文法中出现的语法分析成分分析结束前，另起一行输出当前语法成分的名字，形如“<AddExp>”

【输入形式】testfile.txt中的符合文法要求的测试程序。

【输出形式】按如上要求将语法分析结果输出至output.txt中。

【特别提醒】（1）本次作业只考核对正确程序的处理，但需要为今后可能出现的错误情况预留接口。

           （2）当前要求的输出只是为了便于评测，完成编译器中无需出现这些信息，请设计为方便打开/关闭这些输出的方案。

------------------------------------------------------------------------------------------------------------------------------------------------------

2023中间代码生成作业题目-B（Block语言的翻译）

特别注意：第17条产生式改为

17) Stmt → 'while' '(' Cond ')' Stmt



【问题描述】

语法分析作业中选B的同学完成本作业，需要在第二次作业的基础上完成。

本次作业将Block语言翻译成三地址代码，Block中包含变量声明、赋值语句、if语句、while语句、输入输出语句

文法和语法分析作业B中相同


0)  CompUnit → Block

1)  Block → '{' BlockItemList '}'

2)  BlockItemList →  BlockItemList BlockItem 

3)  BlockItemList →  EPSILON

4)  BlockItem → VarDecl

5)  BlockItem → Stmt

6)  VarDecl → 'int' VarDeclList ';' 

7)  VarDeclList → VarDeclList  ',' VarDef

8)  VarDeclList → VarDef

9)  VarDef → Ident

10) VarDef → Ident '=' Exp

11) Stmt → Ident '=' Exp ';' 

12) Stmt → Exp ';'

13) Stmt → ';'

14) Stmt → Block

15) Stmt → 'if' '(' Cond ')' Block

16) Stmt → 'if' '(' Cond ')' Block 'else' Block

17) Stmt → 'while' '(' Cond ')' Stmt

18) Stmt → Ident '=' 'getint''('')'';'

19) Stmt → 'printf''('FormatString ExpList ')'';' 

20) ExpList → ExpList ',' Exp

21) ExpList → EPSILON

22) Exp → AddExp 

23) Cond → LOrExp 

24) PrimaryExp → '(' Exp ')'

25) PrimaryExp → Ident

26) PrimaryExp → IntConst 

27) UnaryExp → PrimaryExp

28) MulExp → UnaryExp 

29) MulExp → MulExp '*' UnaryExp

30) MulExp → MulExp '/' UnaryExp 

31) MulExp → MulExp '%' UnaryExp 

32) AddExp → MulExp

33) AddExp → AddExp '+' MulExp

34) AddExp → AddExp '-' MulExp 

35) RelExp → AddExp

36) RelExp → RelExp '<' AddExp

37) RelExp → RelExp '>' AddExp 

38) RelExp → RelExp '<=' AddExp 

39) RelExp → RelExp '>=' AddExp  

40) EqExp → RelExp

41) EqExp → EqExp '==' RelExp 

42) EqExp → EqExp '!=' RelExp 

43) LAndExp → EqExp

44) LAndExp → LAndExp '&&' EqExp 

45) LOrExp → LAndExp

46) LOrExp → LOrExp '||' LAndExp



请在LR语法分析程序的基础上添加相应的语义动作，完成三地址代码的翻译。输入输出及处理要求如下：

（1）需按文法规则，用LR分析法法对文法中定义的语法成分进行分析（需要使用第1次作业中的词法分析程序）；

（2）本项作业第2次作业的基础上进行；

（3）属性文法请自行设计；

（4）printf语句按照函数调用生成三地址代码，即param arg1; param arg2; param arg3; call func n;其中n为参数个数。和作业A不同，作业B的printf文法允许输出多个整数。

（5）对于输入语句a=getint();测试程序要求翻译成a=getint()

（6）最后需额外添加一条停语句，即halt，参见样例

（7）为了方便进行自动评测，输入的被编译源文件统一命名为testfile.txt（注意不要写错文件名）；输出的结果文件统一命名为output.txt（注意不要写错文件名）；结果文件中三地址代码的编号从0开始，输出时编号后面有冒号以及\t制表符

【输入形式】testfile.txt中的符合文法要求的测试程序。

【输出形式】按如上要求将三地址代码结果输出至output.txt中。

【特别提醒】（1）本次作业只考核对正确程序的处理，但需要为今后可能出现的错误情况预留接口。

           （2）当前要求的输出只是为了便于评测，完成编译器中无需出现这些信息，请设计为方便打开/关闭这些输出的方案。

           （3）请自行设计三地址代码的内部存储形式，注意它并非一个简单的字符串，否则后续目标代码生成阶段会比较麻烦。
------------------------------------------------------------------------------------------------------------------------------------------------------

2024秋代码生成作业-A（满分20分）
【问题描述】

请在词法分析、语法分析、中间代码生成作业的基础上，为编译器实现代码生成功能。输入输出及处理要求如下：

完成编译器，将源文件（统一命名为testfile.txt）编译生成MIPS汇编并输出到文件（统一命名为mips.txt），具体要求包括：

        a）需在中间代码的基础上生成MIPS汇编，请中间代码作业选A的继续完成本题目。

        b）自行调试时，可使用Mars仿真器（使用方法见“Mars仿真器使用说明.docx”），提交到平台的编译器只需要能按统一的要求生成MIPS汇编代码文件即可

        c）编译器仅读取testfile.txt文件并生成相应的MIPS代码，编译器自身不要读入标准输入中的内容。

【输入形式】testfile.txt为符合文法要求的测试程序。另外可能存在来自于标准输入的数据。

【输出形式】目标代码生成结果输出至mips.txt中。
