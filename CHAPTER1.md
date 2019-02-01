# CHAPTER 1 THE ASSEMBLER | 第一章 汇编器
## <a id="1.1"></a>1.1  THE ASXXXX ASSEMBLERS | ASXXXX汇编器

The ASxxxx assemblers are a series of microprocessor assemblers written in the C programming language. Each assembler has a device specific section which includes:

ASxxxx汇编器（集合）是一系列用C语言编写的微处理器汇编器。每种汇编器都有一个特定于设备的节，其中包含了：

1. device description, byte order, and file extension information\
\
设备描述、字节顺序和文件扩展名信息

2. a table of the assembler general directives, special device directives, assembler mnemonics and associated operation codes\
\
由汇编通用指示符、特殊设备指示符、汇编助记符和相关操作码构成的表格

3. machine specific code for processing the device mnemonics, addressing modes, and special directives\
\
特定于机器的代码，用于处理设备助记符、地址模式和特殊指示符

The device specific information is detailed in the appendices.

特定于设备的详细信息请参见附录（译注：未翻译）。

The assemblers have a common device independent section which handles the details of file input/output, symbol table generation, program/data areas, expression analysis, and assembler directive processing.

这些汇编器有一个公用的设备无关节，用于处理与文件输入/输出、符号表生成、程序数据分区、表达式分析、以及汇编指示符处理相关的细节。

The assemblers provide the following features:

这些汇编器提供了下列特性：

1. Command string control of assembly functions\
\
汇编功能的命令字符串控制

2. Alphabetized, formatted symbol table listing\
\
按字母排序的、格式化过的符号表列表

3. Relocatable object modules\
\
可重定位的目标模块

4. Global symbols for linking object modules\
\
用于连接目标模块的全局符号

5. Conditional assembly directives\
\
条件汇编指示符

6. Program sectioning directives\
\
程序节指示符

ASxxxx assembles one or more source files into a single relocatable ascii object file. The output of the ASxxxx assemblers consists of an ascii relocatable object file(\*.rel), an assembly listing file(\*.lst), and a symbol file(\*.sym) each controlled by an assembler option. If both the object and listing files are
 specified then a listing to relocated listing hint file  (*.hlr) is created as a helper for the linker to properly create the relocated listing file.

ASxxxx可以将一个或多个源文件汇编成一个单独的、基于ASCII的可重定位目标文件。ASxxxx汇编器的输出由一个基于ASCII的可重定位目标文件（\*.rel）、一个汇编列表文件（\*.lst）和一个符号文件（\*.sym）组成，均由汇编器选项控制。如果同时指定了目标文件和列表文件，则会同时创建一个重定位列表提示文件（/*.hlr），用于协助连接器正确地创建可重定位列表文件。

### <a id="1.1.1"></a>1.1.1 Assembly Pass 1 | 第一遍汇编

During pass 1, ASxxxx opens all source files and performs a rudimentary assembly of each source statement. During this process all symbol tables are built, program sections defined, and number of bytes for each assembled source line is estimated.

在第一遍中，ASxxxx打开所有源文件，并对每条源语句进行初步汇编。在这个过程中，会建立所有符号表、定义程序节，并估算每行汇编代码的字节数量。

At the end of pass 1 all undefined symbols may be made global (external) using the ASxxxx switch `-g`, otherwise undefined symbols will be flagged as errors during succeeding passes.

在第一遍的结束，如果使用了ASxxxx的`-g`开关，则所有未定义符号将被作为全局（外部）符号，否则会被接下来的几遍标记为错误。

### <a id="1.1.2"></a>1.1.2 Assembly Pass 2 | 第二遍汇编

During pass 2 the ASxxxx assembler resolves forward references and determines the number of bytes for each assembled line. The number of bytes used by a particular assembler instruction may depend upon the addressing mode, whether the instruction allows multiple forms based upon the relative distance to the addressed location, or other factors. Pass 2 resolves these cases and determines the address of all symbols.

在第二遍中，ASxxxx汇编器解决向前引用并确定每行汇编的字节数。某个汇编指令所使用的字节数取决于地址模式，指令是否允许多种格式取决于它与寻址位置的相对距离或其他因素。第二遍解决这些情形并确定所有符号的地址。

### <a id="1.1.3"></a>1.1.3  Assembly Pass 3 | 第三遍汇编

Pass 3 by the assembler generates the listing file, the relocatable output file, the listing to relocated listing hint file, and the symbol tables. Also during pass 3 the errors will be reported.

在第三遍中，汇编器生成列表文件、可重定位输出文件、重定位列表提示文件以及符号表。同时在第三遍中，错误将被报告出来。

 The relocatable object file is an ascii file containing symbol references and definitions, program area definitions, and the relocatable assembled code, the linker ASLINK will use this information to generate an absolute load file (Intel, Motorola or Tandy CoCo Disk Basic formats).

可重定位目标文件是一个ASCII文件，包含了符号引用和定义、程序区域定义和可重定位的汇编代码，连接器ALINK将会使用这些信息生成绝对加载文件（Intel、Motorola或Tandy CoCo Disk Basic格式）。

## <a id="1.2"></a>1.2 SOURCE PROGRAM FORMAT | 源程序格式

### <a id="1.2.1"></a>1.2.1 Statement Format | 语句格式

A source program is composed of assembly-language statements. Each statement must be completed on one line. A line may contain a maximum of 128 characters, longer lines are truncated and lost.

一个源程序由汇编语言语句构成。每条语句必须在一行内结束。一行最多128字符，超过部分将被截断并丢弃。

An ASxxxx assembler statement may have as many as four fields. These fields are identified by their order within the statement and/or by separating characters between fields. The general format of the ASxxxx statement is:

一条ASxxxx汇编语句做多具有四个字段。这些字段通过其在语句中的位置和/或字段间的分隔字符进行识别。ASxxxx语句的一般形式为：

```
        [label:]  Operator        Operand         [;Comment(s)]

        [标号:]    操作符          操作数           [;注释]
```

The label and comment fields are optional. The operator and operand fields are interdependent. The operator field may be an assembler directive or an assembly mnemonic. The operand field may be optional or required as defined in the context of the operator.

标号和注释字段是可选的。操作符和操作数互相依赖。操作符字段可以是一个汇编器指示符或一个汇编助记符。操作数字段可能是可选的也可能是必须的，由操作符的上下文进行定义。

 ASxxxx interprets and processes source statements one at a time. Each statement causes a particular operation to be performed.

ASxxxx一次解释和处理一条源语句。每条语句都导致特定的操作被执行。

#### <a id="1.2.1.1"></a>1.2.1.1 Label Field | 标号字段

A label is a user-defined symbol which is assigned the value of the current location counter and entered into the user defined symbol table. The current location counter is used by ASxxxx to assign memory addresses to the source program statements as they are encountered during the assembly process. Thus a label is a means of symbolically referring to a specific statement.

标号是一个用户定义的符号，具有当前位置计数器的值，并会被放置到用户定义符号表中。当ASxxxx在汇编过程中遇到原程序语句，它会使用当前位置计数器为该语句赋予内存地址。所以标号的意思就是对一条特定语句的符号化引用。

When a program section is absolute, the value of the current location counter is absolute; its value references an absolute memory address. Similarly, when a program section is relocatable, the value of the current location counter is relocatable. A relocation bias calculated at link time is added to the apparent value of the current location counter to establish its effective absolute address at execution time. (The user can also force the linker to relocate sections defined as absolute. This may be required under special circumstances.)

当一个程序节是绝对定位的，当前位置计数器的值也是绝对的；其值引用一个绝对内存地址。类似的，当一个程序节是可重定位的，当前位置计数器的值也是可重定位的。连接时计算的重定位偏移量将会被加到当前位置计数器的值上，形成运行时所需的有效绝对地址。（用户也可以强制连接器重定位一个被定义为绝对定位的节。这在某些特殊情形下是必须的。）

If present, a label must be the first field in a source statement and must be terminated by a colon (`:`). For example, if the value of the current location counter is absolute `01F0(H)`, the statement:

如果出现了标号，则它必须是源语句中的第一个字段，并且必须以冒号（`:`）结束。例如，如果当前位置计数器的值是绝对值`01F0(H)`，该语句：

```
        abcd:     nop
```

assigns the value `01F0(H)` to the label `abcd`. If the location counter value were relocatable, the final value of `abcd` would be `01F0(H)+K`, where `K` represents the relocation bias of the program section, as calculated by the linker at link time.

将值`01F0(H)`赋给标号`abcd`。如果位置计数器的值是可重定位的，`abcd`的最终值将为`01F0(H)+K`，其中`K`表示程序节的重定位偏移量，由连接器在连接时计算。

More than one label may appear within a single label field. Each label so specified is assigned the same address value. For example, if the value of the current location counter is `1FF0(H)`, the multiple labels in the following statement are each assigned the value `1FF0(H)`:

一个单独的标号字段中可以出现多于一个标号。其中指定的每一个标号都具有相同的地址值。例如，如果当前位置计数器的值是`1FF0(H)`，下面语句中的每一个标号都将被赋值`1FF0(H)`：

```
        abcd:     aq:     $abc:   nop
```

 Multiple labels may also appear on successive lines. For example, the statements

多个标号也可以出现在连续的行上。例如，该语句：

```
        abcd:
        aq:
        $abc:     nop
```

likewise cause the same value to be assigned to all three labels.

同样导致相同的值被赋给所有三个标号。

A double colon (`::`) defines the label as a global symbol. For example, the statement

双冒号（`::`）将标号定义为全局符号。例如，该语句：

```
        abcd::    nop
```

establishes the label `abcd` as a global symbol. The distinguishing attribute of a global symbol is that it can be referenced from within an object module other than the module in which the symbol is defined. References to this label in other modules are resolved when the modules are linked as a composite executable image.

将标号`abcd`设立为全局符号。全局符号的明显特性是，它可以被定义它的目标模块之外的其他模块引用。在其他模块中对该标号的引用将在模块被连接并构成可执行镜像的时候进行解决。

The legal characters for defining labels are:

可用于定义标号的合法字符有：

```
        A through Z              A 到 Z
        a through z              a 到 z
        0 through 9              0 到 9
        . (Period)               . （点）
        $ (Dollar sign)          $ （美元符）
        _ (underscore)           _ （下划线）
```

Alabel may be any length, however only the first 79 characters are significant and, therefore must be unique among all labels in the source program (not necessarily among separately compiled modules). An error code(s) (`<m>` or `<p>`) will be generated in the assembly listing if the first 79 characters in two or more labels are the same. The `<m>` code is caused by the redeclaration of the symbol or its reference by another statement. The `<p>` code is generated because the symbols location is changing on each pass through the source file.

标号可以具有任意长度，但只有前79个字符有用，因此前面这些字符在源程序的所有标号中必须唯一（对于不同的编译模块则无此必要）。如果两个或更多个标号的前79个字符一样，则会在汇编列表中生成一个错误码（`<m>`或`<p>`）。符号被重声明或被其他语句使用会导致`<m>`错误码。在每一遍遍历源文件的过程中，符号位置发生变化会导致`<p>`错误码。

The label must not start with the characters `0-9`, as this designates a reusable symbol with special attributes described in a later section.

标号不能以字符`0-9`开头，这种格式专用于具有特殊属性的可重用符号，这将在后面的小节中介绍。

 The label must not start with the sequence `$$`, as this represents the temporary radix 16 for constants.

标号不能以序列`$$`开头，这种格式表示临时的16进制常量。

#### <a id="1.2.1.2"></a>1.2.1.2 Operator Field | 操作符字段

The operator field specifies the action to be performed. It may consist of an instruction mnemonic (op code) or an assembler directive.

操作符字段制定了要执行的动作。它由指令助记符（操作码）或汇编器指示符构成。

When the operator is an instruction mnemonic, a machine instruction is generated and the assembler evaluates the addresses of the operands which follow. When the operator is a directive ASxxxx performs certain control actions or processing operations during assembly of the source program.

如果操作符是一个指令助记符，汇编器会生成一条机器指令，并对后续操作数的地址进行求值。如果操作符是一个指示符，ASxxxx会执行特定的控制动作或完成汇编源程序过程中的处理操作。

Leading and trailing spaces or tabs in the operator field have no significance; such characters serve only to separate the operator field from the preceeding and following fields.

操作符前后的空格和制表符是没用的；这些字符只用于将操作符字段和前后的其他字段分开。

An operator is terminated by a space, tab or end of line.

操作符由空格、制表符或行尾终结。

#### <a id="1.2.1.3"></a>1.2.1.3 Operand Field | 操作数字段

When the operator is an instruction mnemonic (op code), the operand field contains program variables that are to be evaluated/manipulated by the operator.

如果操作符是一个指令助记符（操作码），则操作数字段包含程序变量，会被操作符进行求值/操作。

Operands may be expressions or symbols, depending on the operator. Multiple expressions used in the operand fields may be separated by a comma. An operand should be preceeded by an operator field; if it is not, the statement will give an error (`<q>` or `<o>`). All operands following instruction mnemonics are treated as expressions.

操作数可以使表达式或符号，取决于操作符。操作数字段中使用的多个表达式，可以用逗号进行分隔。操作数前必须有操作符字段；如果没有的话，该语句会产生错误（`<p>`或`<o>`）。指令助记符后跟的所有操作数都会被视为表达式。

The operand field is terminated by a semicolon when the field is followed by a comment. For example, in the following statement:

如果操作数字段后根由注释，则操作数字段被分号终结。例如，在下面的语句中：

```
        label:    lda     abcd,x          ;Comment field
```

the tab between `lda` and `abcd` terminates the operator field and defines the beginning of the operand field; a comma separates the operands `abcd` and `x`; and a semicolon terminates the operand field and defines the beginning of the comment field. When no comment field follows, the operand field is terminated by the end of the source line.

`lda`和`abcd`之间的制表符终结了操作符字段并定义了操作数字段的起始；逗号分隔了操作数`abcd`和`x`；分号终结了操作数字段并定义了注释字段的起始。如果没有后跟注释字段，则操作数字段被源代码行的结束所终结。

#### <a id="1.2.1.4"></a>1.2.1.4 Comment Field | 注释字段

The comment field begins with a semicolon and extends through the end of the line. This field is optional and may contain any 7-bit ascii character except `null`.

注释字段由分号开始，一直扩展到行尾。该字段是可选的，并可以包含除了`null`以外的任意7位ASCII字符。

Comments do not affect assembly processing or program execu- tion.

注释不影响会变过程或程序执行。

#### <a id="1.3"></a>1.3 SYMBOLS AND EXPRESSIONS | 符号和表达式

This section describes the generic components of the ASxxxx assemblers: the character set, the conventions observed in constructing symbols, and the use of numbers, operators, and expressions.

这一小节介绍ASxxxx的通用组件——字符集、构造符号时需要遵守的约定以及数字、操作符和表达式的使用。

#### <a id="1.3.1"></a>1.3.1 Character Set | 字符集

The following characters are legal in ASxxxx source programs:

下面的字符在ASxxxx源程序中是合法的：

1. The letters `A` through `Z`. Both upper- and lower-case letters are acceptable. The assemblers, by default, are case sensitive, i.e. `ABCD` and `abcd` are not the same symbols. (The assemblers can be made case insensitive by using the `-z` command line option.)\
\
字母`A`到`Z`。大小写字母均可接受。汇编器默认是大小写敏感的，即`ABCD`和`abcd`是不同的符号。（汇编器也可以通过`-z`命令行选项编程大小写不敏感的。）

2. The digits `0` through `9`\
\
数字`0`到`9`

3. The characters `.` (period), `$` (dollar sign), and `_` (un- derscore).\
\
字符`.`（点）、`$`（美元符）和`_`（下划线）。

4. The special characters listed in Tables 1 through 6.\
\
表1到6种列出的特殊字符。

Tables 1 through 6 describe the various ASxxxx label and field terminators, assignment operators, operand separators, assembly, unary, binary, and radix operators.

表1到6介绍了各种ASxxxx标号和字段分隔符、赋值操作符、操作数分隔符、汇编、一元、二元和进制运算符。

```
        Table 1         Label Terminators and Assignment Operators
           表 1         标号终结符和赋值运算符
        ----------------------------------------------------------------

                :   Colon               Label terminator.
                    冒号                标号终结符

                ::  Double colon        Label  Terminator;   defines the
                                        label as a global label.
                    双冒号              标号终结符；
                                        将标号定义为全局标号。

                =   Equal sign          Direct assignment operator.
                    等号                直接赋值运算符。

                ==  Global equal        Direct assignment operator;  de-
                                        fines the  symbol  as  a  global
                                        symbol.
                    全局等号             直接赋值运算符；
                                        将符号定义为全局符号。

                =:  Local equal         Direct assignment operator;  de-
                                        fines the symbol as a local sym-
                                        bol.
                    局部等号             直接赋值运算符；
                                        将符号定义为局部符号。

        ----------------------------------------------------------------
```

```
        Table 2         Field Terminators and Operand Separators
           表 2         字段终结符和操作数分隔符
        ----------------------------------------------------------------

                    Tab                 Item or field terminator.
                    制表符               项或字段终结符。

                    Space               Item or field terminator.
                    空格                 项或字段终结符。

                ,   Comma               Operand field separator.
                    逗号                操作数字段分隔符。

                ;   Semicolon           Comment field indicator.
                    分号                 注释字段指示符。

        ----------------------------------------------------------------
```

```
        Table 3         Assembler Operators
           表 3         汇编器运算符
        ----------------------------------------------------------------

                #   Number sign         Immediate expression indicator.
                    数字符               立即（数）表达式指示符。

                .   Period              Current location counter.
                    句点                 当前位置计数器。

                (   Left parenthesis    Expression delimiter.
                    左括号               表达式分界符。

                )   Right parenthesis   Expression delimeter.
                    右括号               表达式分界符。

        ----------------------------------------------------------------
```

```
        Table 4         Unary Operators
           表 4         一元运算符
        ----------------------------------------------------------------

                <   Left bracket        <FEDC   Produces  the lower byte
                                                value of the expression.
                                                (DC)
                    左尖括号                     生成表达式的低字节值（DC）

                >   Right bracket       >FEDC   Produces  the upper byte
                                                value of the expression.
                                                (FE)
                    右尖括号                     生成表达式的高字节值（FE）

                +   Plus sign           +A      Positive value of A
                    加号                         A的正值

                -   Minus sign          -A      Produces   the  negative
                                                (2's complement) of A.
                    减号                         产生A的负值（2的补码）

                ~   Tilde               ~A      Produces the 1's comple-
                                                ment of A.
                    波浪号                       产生A的1的补码

                '   Single quote        'D      Produces  the  value  of
                                                the character D.
                    单引号                       产生字符D的值

                "   Double quote        "AB     Produces the double byte
                                                value for AB.
                    双引号                      产生AB的双字节值

                \   Backslash           '\n     Unix style characters
                                                \b, \f, \n, \r, \t
                                     or '\001   or octal byte values.
                    反斜线                       Unix风格（转义）字符
                                                或八进制字节值

        ----------------------------------------------------------------
```

```
        Table 5         Binary Operators
           表 5         二元运算符
        ----------------------------------------------------------------

                <<  Double          0800 << 4   Produces the 4 bit
                    Left bracket                left-shifted   value  of
                                                0800.  (8000)
                    双左尖括号                   产生0800左移4位的值（8000）

                >>  Double          0800 >> 4   Produces the 4 bit
                    Right bracket               right-shifted  value  of
                                                0800.  (0080)
                    双右尖括号                   产生0800右移4位的值（0080）

                +   Plus sign       A + B       Arithmetic      Addition
                                                operator.
                    加号                         算术加法运算

                -   Minus sign      A - B       Arithmetic   Subtraction
                                                operator.
                    减号                         算数减法运算

                *   Asterisk        A * B       Arithmetic   Multiplica-
                                                tion operator.
                    星号                         算术乘法运算

                /   Slash           A / B       Arithmetic      Division
                                                operator.
                    斜线                        算数除法运算

                &   Ampersand       A & B       Logical AND operator.
                    “和”字符                    逻辑“与”运算

                |   Bar             A | B       Logical OR operator.
                    竖线                        逻辑“或”运算

                %   Percent sign    A % B       Modulus operator.
                    百分号                       取模运算

                ^   Up arrow or     A ^ B       EXCLUSIVE OR operator.
                    circumflex
                    上箭头或声调符号              异或运算

        ----------------------------------------------------------------
```

```
        Table 6         Temporary Radix Operators
           表 6         临时进制运算符
        ----------------------------------------------------------------

                $%,   0b, 0B            Binary radix operator.
                                        二进制运算符

                $&,   0o, 0O, 0q, 0Q    Octal radix operator.
                                        八进制运算符

                $#,   0d, 0D            Decimal radix operator.
                                        十进制运算符

                $$,   0h, 0H, 0x, 0X    Hexidecimal radix operator.
                                        十六进制运算符


                Potential ambiguities arising from the use of 0b and 0d
                as temporary radix operators may be circumvented by pre-
                ceding all non-prefixed hexidecimal numbers with 00.

                Leading 0's are required in any case where the first
                hexidecimal digit is abcdef as the assembler will treat
                the letter sequence as a label.

                使用0b或0d做临时进制运算符可能会产生歧义，这可以通过在所有
                无前缀的十六进制数前添加00进行避免。

                任何时候，如果十六进制数的第一个数字是abcdef，
                则前导0是必须的，否则汇编器将会将其视为一个标号的字母序列。

        ----------------------------------------------------------------
```

#### <a id="1.3.2"></a>1.3.2 User-Defined Symbols | 用户定义的符号

User-defined symbols are those symbols that are equated to a specific value through a direct assignment statement or appear as labels. These symbols are added to the User Symbol Table as they are encountered during assembly.

用户定义的符号包括通过直接赋值语句产生的符号以及作为标号出现的符号。在汇编过程中遇到这些符号，会将其添加到用户符号表中。

The following rules govern the creation of user-defined symbols:

创建用户定义的符号时，有下列规则：

1. Symbols can be composed of alphanumeric characters, dollar signs (`$`), periods (`.`), and underscores (`_`) only.\
\
符号只能由字母数字、美元符（`$`）、句点（`.`）和下划线（`_`）组成。

2. The first character of a symbol must not be a number (except in the case of reusable symbols).\
\
符号的第一个字符不能是数字（可重用符号除外）。

3. The first 79 characters of a symbol must be unique. A symbol can be written with more than 79 legal characters, but the 80th and subsequent characters are ignored.\
\
一个符号的前79个字符必须唯一。一个符号可以写进多于79个合法字符，但第80个及以后的字符将被忽略。

4. Spaces and Tabs must not be embedded within a symbol.\
\
符号中不能嵌入空格和制表符。

#### <a id="1.3.3"></a>1.3.3 Reusable Symbols | 可重用符号

Reusable symbols are specially formatted symbols used as labels within a block of coding that has been delimited as a reusable symbol block. Reusable symbols are of the form `n$`, where `n` is a decimal integer from `0` to `65535`, inclusive. Examples of reusable symbols are:

可重用符号是特殊格式的符号，用在被限定为可重用符号块的代码块中。可重用符号的形式为`n$`，其中`n`是一个从`0`到`65535`（含）的十进制整数。可重用符号的例子有：

```
        1$
        27$
        138$
        244$
```

The range of a reusable symbol block consists of those statements between two normally constructed symbolic labels. Note that a statement of the form:

可重用符号块的范围是两个常规的符号化标号之间的部分。注意对于这种形式的语句：

```
        ALPHA = EXPRESSION
```

is a direct assignment statement but does not create a label and thus does not delimit the range of a reusable symbol block.

这是一个直接赋值语句，但是没有创建标号，所以不用于界定可重用符号块的范围。

Note that the range of a reusable symbol block may extend across program areas.

注意可重用符号块的范围可能会跨程序区域。

Reusable symbols provide a convenient means of generating labels for branch instructions and other such references within reusable symbol blocks. Using reusable symbols reduces the possibility of symbols with multiple definitions appearing within a user program. In addition, the use of reusable symbols differentiates entry-point labels from other labels, since reusable labels cannot be referenced from outside their respective symbol blocks. Thus, reusable symbols of the same name can appear in other symbol blocks without conflict. Reusable symbols require less symbol table space than normal symbols. Their use is recommended.

在分支指令和其他类似的可重用符号块中，可重用符号提供了一种方便的标号创建方式。使用可重用符号可以降低用户程序中出现多重复号定义的可能性。另外，使用可重用符号还能突出入口点标号，因为可重用符号不能在与其相关的可重用符号块之外引用。因此，在不同的可重用符号块中，可重用符号可以有相同的名字，不会冲突。可重用符号需要的符号表空间比普通符号要小。推荐使用。

The use of the same reusable symbol within a symbol block will generate one or both of the `<m>` or `<p>` errors.

在同一个可重用符号块中定义相同的可重用符号，会产生`<m>`或`<p>`两种错误之一，或都产生。

Example of reusable symbols:

可重用符号的示例：

```
        a:      ldx     #atable ;get table address   表的地址
                lda     #0d48   ;table length        表的长度
        1$:     clr     ,x+     ;clear               清空
                deca
                bne     1$

        b:      ldx     #btable ;get table address   表的地址
                lda     #0d48   ;table length        表的长度
        1$:     clr     ,x+     ;clear               清空
                deca
                bne     1$
```

#### <a id="1.3.4"></a>1.3.4 Current Location Counter | 当前位置计数器

The period (`.`) is the symbol for the current location counter. When used in the operand field of an instruction, the period represents the address of the first byte of the instruction:

句点（`.`）时表示当前位置计数器的符号。在一条指令的操作数字段中使用句点时，该句点表示这条指令第一个字节的地址。

```
        AS:     ldx     #.      ;The period (.) refers to    句点（.）表示
                                ;the address of the ldx      ldx指令
                                ;instruction.                的地址
```

When used in the operand field of an ASxxxx directive, it represents the address of the current byte or word:

在ASxxxx指示符的操作数字段中使用句点时，该句点表示当前字节或字的地址。

```
        QK = 0

        .word   0xFFFE,.+4,QK   ;The operand .+4 in the .word
                                ;directive represents a value
                                ;stored in the second of the
                                ;three words during assembly.

                                ;.word指示符中的操作数.+4
                                ;表示汇编时遇到的三个字中
                                ;第二个字的位置
```

If we assume the current value of the program counter is `0H0200`, then during assembly, ASxxxx reserves three words of storage starting at location `0H0200`. The first value, a hexidecimal constant `FFFE`, will be stored at location `0H0200`. The second value represented by `.+4` will be stored at location `0H0202`, its value will be `0H0206` (` = 0H0202 + 4`). The third value defined by the symbol `QK` will be placed at location `0H0204`.

如果我们假设在汇编过程中，程序计数器的当前值是`0H0200`，ASxxxx会保留从位置`0H0200`开始的三个字大小的存储空间。其中第一个值是十六进制常量`FFFE`，会被存储到位置`0H0200`。第二个由`.+4`表示的值将会被存储到位置`0H0202`，其值将会是`0H0206`（`= 0H0202 + 4`）。第三个由符号`QK`定义的值将会被放置到位置`0H0204`。

At the beginning of each assembly pass, ASxxxx resets the location counter. Normally, consecutive memory locations are assigned to each byte of object code generated. However, the value of the location counter can be changed through a direct assignment statement of the following form:

在每一遍汇编的开始，ASxxxx会重置位置计数器。一般来说，生成的目标代码中的每个字节会被分配连续的内存位置。不过，位置计数器的值可以通过下面这种形式的直接赋值语句改变：

```
        . = . + expression
```

The new location counter can only be specified relative to the current location counter. Neglecting to specify the current program counter along with the expression on the right side of the assignment operator will generate the `<.>` error. (Absolute program areas may use the `.org` directive to specify the absolute location of the current program counter.)

位置计数器的新值只能通过其当前值进行计算。在赋值运算符的右侧漏掉位置计数器的当前值，会导致`<.>`错误。（绝对定位的程序区域可以使用`.org`指示符指定当前程序计数器的绝对位置。）

The following coding illustrates the use of the current location counter:

下面的代码展示了当前位置计数器的使用：

```
                .area   CODE1   (ABS)   ;program area CODE1
                                        ;is ABSOLUTE

                                        ;程序区域CODE1是绝对定位的

                .org    0H100           ;set location to
                                        ;0H100 absolute

                                        ;设置绝对位置为0H100

        num1:   ldx     #.+0H10         ;The label num1 has
                                        ;the value 0H100.
                                        ;X is loaded with
                                        ;0H100 + 0H10

                                        ;标号num1的值为0H100。
                                        ;X载入值0H100 + 0H10

                .org    0H130           ;location counter
                                        ;set to 0H130

                                        ;位置计数器被设置为0H130

        num2:   ldy     #.              ;The label num2 has
                                        ;the value 0H130.
                                        ;Y is loaded with
                                        ;value 0H130.

                                        ;标号num2的值为0H130
                                        ;Y载入值0H130

                .area   CODE2   (REL)   ;program area CODE2
                                        ;is RELOCATABLE

                                        ;程序区域CODE2是可重定位的

                . = . + 0H20            ;Set location counter
                                        ;to relocatable 0H20 of
                                        ;the program section.

                                        ;将位置计数器设置为
                                        ;程序节偏移0H20的位置

        num3:   .word   0               ;The label num3 has
                                        ;the value
                                        ;of relocatable 0H20.

                                        ;标号num3的值是偏移0H20之后的值

                . = . + 0H40            ;will reserve 0H40
                                        ;bytes of storage as will

                                        ;将会保留0H40个存储字节

                .blkb   0H40            ;or  或这样
                .blkw   0H20
```

The `.blkb` and `.blkw` directives are the preferred methods of allocating space.

`.blkb`和`.blkw`指示符才是用于分配空间的推荐方式。

### <a id="1.3.5"></a>1.3.5 Numbers | 数字

ASxxxx assumes that all numbers in the source program are to be interpreted in decimal radix unless otherwise specified. The `.radix` directive may be used to specify the default as octal, decimal, or hexidecimal. Individual numbers can be designated as binary, octal, decimal, or hexidecimal through the temporary radix prefixes shown in table 6.

ASxxxx假设源程序中的所有数字都是十进制，除非特别指明。`.radix`指示符可以用于指定默认值为八进制、十进制或十六进制。用表6中列出的临时进制前缀，可以将单独的数字指定为二进制、八进制、十进制或十六进制。

Negative numbers must be preceeded by a minus sign; ASxxxx translates such numbers into two's complement form. Positive numbers may (but need not) be preceeded by a plus sign.

负数前面必须出现一个减号；ASxxxx将这样的数字解释为2的补码形式。整数前面可以（但不是必须）出现一个加号。

Numbers are always considered to be absolute values, therefor they are never relocatable.

数字总是被当作绝对值，因此永远不能是可重定位的。

### <a id="1.3.6"></a>1.3.6 Terms | 项

A term is a component of an expression and may be one of the following:

项是表达式中的一种组成部分，可以是下列之一：

1. A number.\
\
数字。

2. A symbol:\
\
符号：

    1. A period (`.`) specified in an expression causes the current location counter to be used.\
\
在表达式中是定的句点（`.`）导致使用了当前位置计数器。

    2. A User-defined symbol.\
\
用户定义的符号。

    3. An undefined symbol is assigned a value of zero and inserted in the User-Defined symbol table as an undefined symbol.\
\
未定义的符号被赋以零值并作为未定义符号插入用户符号表中。

3. A single quote followed by a single ascii character, or a double quote followed by two ascii characters.\
\
单引号后跟一个ASCII字符，或双引号后跟两个ASCII字符。

4. An expression enclosed in parenthesis. Any expression so enclosed is evaluated and reduced to a single term before the remainder of the expression in which it appears is evaluated. Parenthesis, for example, may be used to alter the left-to-right evaluation of expressions, (as in `A*B+C` versus `A*(B+C)`), or to apply a unary operator to an entire expression (as in `-(A+B)`).\
\
包围在括号中的表达式。在对其出现的表达式的其余部分进行求值时，括号包围的表达式会被求值并简化为一个单独项。括号可以用于改变自左至右的求值顺序（如`A*B+C`相对于`A*(B+C)`），或对整个表达式应用一元运算符（如`-(A+B)`）。

5. A unary operator followed by a symbol or number.\
\
一元运算符后跟符号或数字。

### <a id="1.3.7"></a>1.3.7 Expressions | 表达式

Expressions are combinations of terms joined together by binary operators. Expressions reduce to a value. The evaluation of an expression includes the determination of its attributes. A resultant expression value may be one of three types (as described later in this section): relocatable, absolute, and external.

表达式是通过二元运算符将项连接起来构成的组合。表达式会化简为一个值。对表达式的求值包括了确定其属性。作为结果的表达式值可以是三种类型（本小节稍后介绍）之一——可重定位的、绝对的和外部的。

Expressions are evaluate with an operand hierarchy as follows:

表达式按照下面的操作数层级进行求值：

```
        *       /       %       multiplication,
                                division, and
                                modulus first.

                                先做乘、除和取模运算.

        +       -               addition and
                                subtraction second.

                                然后是加法和减法。

        <<      >>              left shift and
                                right shift third.

                                左移和右移。

        ^                       exclusive or fourth.

                                异或排第四。

        &                       logical and fifth.

                                逻辑“与”第五。

        |                       logical or last

                                最后是逻辑“或”。

        except that unary operators take precedence over binary
        operators.

        一元运算符除外，它们比二元运算符优先。
```

A missing or illegal operator terminates the expression analysis, causing error codes `<o>` and/or `<q>` to be generated depending upon the context of the expression itself.

缺失运算符或有非法运算符会终止对表达式的分析，导致生成错误码`<o>`和/或`<q>`，取决于表达式自身的上下文。

At assembly time the value of an external (global) expression is equal to the value of the absolute part of that expression. For example, the expression `external+4`, where '`external`' is an external symbol, has the value of `4`. This expression, however, when evaluated at link time takes on the resolved value of the symbol '`external`', plus `4`.

汇编时，一个外部（全局）表达式的值等于该表达式绝对部分的值。例如，在表达式`external+4`中，`external`时一个外部符号，其值为`4`。而这个表达式在连接求值时，会取符号`external`解决之后的值，再加`4`。

Expressions, when evaluated by ASxxxx, are one of three types: relocatable, absolute, or external. The following distinctions are important:

ASxxxx中的表达式有三种类型——可重定位的、绝对定位的和外部的。下面的区别非常重要：

1. An expression is relocatable if its value is fixed relative to the base address of the program area in which it appears; it will have an offset value added at link time. Terms that contain labels defined in relocatable program areas will have a relocatable value; similarly, a period (`.`) in a relocatable program area, representing the value of the current program location counter, will also have a relocatable value.\
\
如果一个表达式的值与其出现的程序区域的基地址是固定相对的，则该表达式是可重定位的；在连接时将会为其加一个偏移量。包含标号的项，如果该标号定义在可重定位的程序区域，则该项具有可重定位值；类似的，可重定位程序区域中的句点（`.`）表示了当前程序位置计数器的值，也具有可重定位值。

2. An expression is absolute if its value is fixed. An expression whose terms are numbers and ascii characters will reduce to an absolute value. A relocatable expression or term minus a relocatable term, where both elements being evaluated belong to the same program area, is an absolute expression. This is because every term in a program area has the same relocation bias. When one term is subtracted from the other the relocation bias is zero.\
\
如果一个表达式的值是固定的，则该表达式是绝对的。如果表达式中的项是数字或ASCII字符，则它会被化简为绝对值。一个可重定位表达式或项，减去另一个可重定位项，如果两个元素经过求值都属于同一个程序区域，则它是一个绝对表达式。这是因为一个程序区域里的每个项都具有相同的重定位偏移。当一个项减去另一个时，最终的重定位偏移为零。

3. An expression is external (or global) if it contains a single global reference (plus or minus an absolute expression value) that is not defined within the current program. Thus, an external expression is only partially defined following assembly and must be resolved at link time.\
\
如果一个表达式包含了一个未在当前程序中定义的全局引用（加上或减去一个绝对表达式值），则该表达式时外部的（或全局的）。因此，外部表达式在汇编过程中只是部分定义的，必须在连接时进行解决。

#### <a id="1.4"></a>1.4 GENERAL ASSEMBLER DIRECTIVES | 通用汇编器指示符

An ASxxxx directive is placed in the operator field of the source line. Only one directive is allowed per source line. Each directive may have a blank operand field or one or more operands. Legal operands differ with each directive.

ASxxxx指示符可以放在源代码行的操作符字段里。每行只允许有一个指示符。每个指示符可以没有操作数字段，或有一个或多个操作数。每个指示符可以有不同的合法操作数。

#### <a id="1.4.1"></a>1.4.1 `.module` Directive | `.module`指示符

Format:

格式：

```
        .module name
```

The `.module` directive causes the `name` to be included in the assemblers output file as an identifier for this particular object module. The `name` may be from 1 to 79 characters in length. The `name` may not have any embedded white space (spaces or tabs). Only one identifier is allowed per assembled module. The main use of this directive is to allow the linker to report a modules' use of undefined symbols. At link time all undefined symbols are reported and the modules referencing them are listed.

`.module`指示符可以使`name`被包含到汇编器输出文件中，作为该特定目标模块的标识符。`name`的长度可以是1到79个字符。`name`不能包含任何内嵌的空白（空格或制表符）。每个汇编模块只能有一个标识符。该指示符的主要作用是允许连接器报告模块对未定义符号的使用。所有未定义符号都会在连接时被报告出来，同事引用了它们的模块也会被列出来。

#### <a id="1.4.2"></a>1.4.2 `.title` Directive | `.title`指示符

Format:

格式：

```
        .title  string
```

The `.title` directive provides a character string to be placed on the second line of each page during listing. The string begins with the first non white space character (after any space or tab) and ends with the end of the line.

`.title`指示符提供了一个字符串，在生成列表文件时，该字符串将被放置在每一页的第二行。该字符串从第一个非空白字符开始（任何空格或制表符之后），到行尾结束。

#### <a id="1.4.3"></a>1.4.3 `.sbttl` Directive | `.sbttl`指示符

Format:

格式：

```
        .sbttl  string
```

The `.sbttl` directive provides a character string to be placed on the third line of each page during listing. The string begins with the first non white space character (after any space or tab) and ends with the end of the line.

`.sbttl`指示符提供了一个字符串，在生成列表文件时，该字符串将被放置在每一页的第三行。该字符串从第一个非空白字符开始（任何空格或制表符之后），到行尾结束。

#### <a id="1.4.4"></a>1.4.4 `.list` and `.nlist` Directives | `.list`和`.nlist`指示符

Format:

格式：

```
        .list                           ;Basic .list
                                        ;基本的.list

        .list   expr                    ;with expression
                                        ;带表达式的

        .list   (arg1,arg2,...,argn)    ;with sublist options
                                        ;带子列表选项的

        .nlist                          ;Basic .nlist
                                        ;基本的.nlist

        .nlist  expr                    ;with expression
                                        ;带表达式的

        .nlist  (arg1,arg2,...,argn)    ;with sublist options
                                        ;带子列表选项的
```

The `.list` and `.nlist` directives control the listing output to the `.lst` file. The directives have the following sublist options:

`.list`和`.nlist`指示符控制着输出到`.lst`文件的列表。该指示符具有下列子列表选项：

```
        err - errors                          错误
        loc - program location                程序位置
        bin - binary output                   二进制输出
        eqt - symbol or .if evaluation        符号或.if求值
        cyc - opcode cycle count              操作码周期数
        lin - source line number              源代码行
        src - source line text                源代码文本
        pag - pagination                      分页
        lst - .list/.nlist line listing       .list/.nlist行
        md  - macro definition listing        宏定义
        me  - macro expansion listing         宏展开
        meb - macro expansion binary listing  宏展开的二进制

        !   - sets the listing mode to        在应用子列表选项之前
              !(.list) or !(.nlist) before    将列表模式设置为
              applying the sublist options    !(.list)或!(.nlist)
```

The 'normal' listing mode `.list` is the combination of `err`, `loc`, `bin`, `eqt`, `cyc`, `lin`, `src`, `pag`, `lst`, and `md` enabled with `me` and `meb` disabled. The 'normal' listing mode `.nlist` has all sublist items disabled. When specifying sublist options the option list must be enclosed within parenthesis and multiple options seperated by commas.

在“一般的”列表模式下，`.list`是`err`、`loc`、`bin`、`eqt`、`cyc`、`lin`、`src`、`pag`、`lst`的组合，同时`md`是启用的而`me`和`meb`是禁用的。在“一般的”列表模式下，`.nlist`会禁用所有的子列表选项。当需要指定子列表选项时，选项列表必须放在括号中，多个选项之间使用逗号分隔。

The NOT option, `!`, is used to set the listing mode to the opposite of the `.list` or `.nlist` directive before applying the sublist options. For example:

“取反”选项——`!`——用于在应用子列表选项之前，将列表模式设置为与`.list`或`.nlist`相反。例如：

```
        .nlist (!)  is equivalent to .list  and    等价于.list
        .list  (!)  is equivalent to .nlist        等价于.nlist
                    any additional options will    其他更多选项
                    be applied normally            正常应用
```

Normal `.list`/`.nlist` processing is disabled within false conditional blocks. However, the `.list`/`.nlist` with an expression can override this behavior if the expression has a non zero value.

在假条件块中，对`.list`/`.nlist`的处理是被禁用的。然而，带表达式的`.list`/`.nlist`如果其表达式具有非零值，则可以覆盖这一行为。

Examples of listing options:

列表选项示例：

```
        .list   (meb)   ; macro processing lists only
                        ; generated binary and location
                        ; 宏处理列表仅列出生成的二进制和位置

        .list   (me)    ; listing options are enabled
                        ; during macro processing
                        ; 在宏处理过程中启用列表选项

        .nlist  (src)   ; .nlist src lines not listed
                        ; 源代码行不被列出

        .nlist  (!,lst) ; list all except .nlist
                        ; 列出所有，除了.nlist这一行

        .nlist          ; combination lists only
        .list   (src)   ; the source line
                        ; 组合起来，只列源代码行

        .list   (!,src) ; list only the source line
                        ; 只列出源代码行

        .list   1       ; enable listing even within
                        ; a FALSE conditional block
                        ; 即使在“假”代码块也进行列表输出
```

#### <a id="1.4.5"></a>1.4.5 `.page` Directive | `.page`指示符

Format:

格式：

```
        .page
```

The `.page` directive causes a page ejection with a new heading to be printed. The new page occurs after the next line of the source program is processed, this allows an immediately following `.sbttl` directive to appear on the new page. The `.page` source line will not appear in the file listing. Paging may be disabled by invoking the `-p` directive or by using the directive:

`.page`指示符另起一页，并打印新的页头。新页从正在处理的下一行源程序开始，紧随其后的`.sbttl`指示符会出现在新页上。`.page`这一行不会出现在文件列表中。分页可以通过调用`-p`指示符（译注：命令行选项？）或者通过下面的提示符禁用：

```
        .nlist  (pag)
```

If the `.page` directive is followed by a non zero constant or an expression that evaluates to a non zero value then pagination will be enabled within a false condition range to allow extended textual information to be incorporated in the source program with out the need to use the comment delimiter (`;`):

如果`.page`指示符后跟了一个非零常量或者是一个求值结果为非零值的表达式，则即使是在“假”条件的范围里，分页也会被启用，这允许在源程序中包含文本信息而无需使用注释界定符（`;`）：

```
        .if     0

        .page   1       ;Enable pagination within 'if' block.
                        ;在if块中启用分页
        This text will be bypassed during assembly
        but appear in the listing file.
        这些文字在汇编时会被跳过，
        但会出现在列表文件中。
        .
        .
        .

        .endif
```

#### <a id="1.4.6"></a>1.4.6 `.msg` Directive | `.msg`指示符

Format:

格式：

```
        .msg     /string/               or
                                        或

        .msg    ^/string/
```

where:

其中：

```
        string  represents a text string. The string is printed
                to the console during the final assembly pass.

                表示一个文本串。在最后一遍汇编中，
                该字符串将被打印到控制台。

        /  /    represent the delimiting characters. These
                delimiters may be any paired printing
                characters, as long as the characters are not
                contained within the string itself. If the
                delimiting characters do not match, the .msg
                directive will give the <q> error.

                表示界定字符。此界定符可以使任何可打印的字符对，
                只要该字符没有包含在字符串自身中就可以。
                如果界定字符不匹配，.msg指示符会给出<q>错误。
```

The `.msg` directive is useful to report assembly status or other information during the assembly process.

`.msg`指示符可以用于汇报汇编状态或汇编过程中的其他信息。

#### <a id="1.4.7"></a>1.4.7 `.error` Directive | `.error`指示符

Format:

格式：

```
        .error exp
```

where:

其中：

```
        exp     represents an absolute expression. If the
                evaluation of the expression results in a non
                zero value then an <e> error is reported and the
                text line is listed in the generated error.

                表示一个绝对表达式。如果该表达式的求值结果是非零值，
                则会报告<e>错误，并且该文本行会列在产生的错误中。
```

The `.error` directive is useful to report configuration or value errors during the assembly process. (The `.error` directive is identical in function to the `.assume` directive, just perhaps more descriptive.)

`.error`指示符用于报告汇编过程中的配置或值错误。（`.error`指示符在功能上与`.assume`指示符是一样的，不过更具描述性。）

#### <a id="1.4.8"></a>1.4.8 `.byte`, `.db`, and `.fcb` Directives | `.byte`、`.db`和`.fcb`指示符

Format:

格式：

```
        .byte   exp             ;Stores the binary value
        .db     exp             ;of the expression in the
        .fcb    exp             ;next byte.

                                ;将表达式的二进制值
                                ;保存到接下来的字节中

        .byte   exp1,exp2,expn  ;Stores the binary values
        .db     exp1,exp2,expn  ;of the list of expressions
        .fcb    exp1,exp2,expn  ;in successive bytes.

                                ;将表达式列表的二进制值
                                ;存储到连续的字节中
```

where:

其中：

```
        exp,    represent expressions that will be
        exp1,   truncated to 8-bits of data.
        .       Each expression will be calculated,
        .       the high-order byte will be truncated.
        .       Multiple expressions must be
        expn    separated by commas.

                所表示的表达式将被截断到8位数据。
                每个表达式都会被计算，高位字节会被截断。
                多个表达式用逗号分隔。

```

The `.byte`, `.db`, or `.fcb` directives are used to generate successive bytes of binary data in the object module.

`.byte`、`.db`和`.fcb`指示符用于在目标模块中生成连续的二进制字节数据。（译注：其实和进制没关系。）

#### <a id="1.4.9"></a>1.4.9 `.word`, `.dw`, and `.fdb` Directives | `.word`、`.dw`和`.fdb`指示符

Format:

格式：

```
        .word   exp             ;Stores the binary value
        .dw     exp             ;of the expression in
        .fdb    exp             ;the next word.

                                ;将表达式的二进制值
                                ;保存到接下来的字中

        .word   exp1,exp2,expn  ;Stores the binary values
        .dw     exp1,exp2,expn  ;of the list of expressions
        .fdb    exp1,exp2,expn  ;in successive words.

                                ;将表达式列表的二进制值
                                ;存储到连续的字中
```

where:

其中：

```
        exp,    represent expressions that will occupy two
        exp1,   bytes of data. Each expression will be
        .       calculated as a 16-bit word expression.
        .       Multiple expressions must be
        expn    separated by commas.

                所表示的表达式将占据两个字节的数据。
                每个表达式都将被作为16位字进行计算。
                多个表达式用逗号分隔。
```

The `.word`, `.dw`, or `.fdb` directives are used to generate successive words of binary data in the object module.

`.word`、`.dw`和`.fdb`指示符用于在目标模块中生成连续的二进制字数据。

#### <a id="1.4.10"></a>1.4.10 `.3byte` and `.triple` Directives | `.3byte`和`.triple`指示符

Format:

格式：

```
        .3byte  exp             ;Stores the binary value
        .triple exp             ;of the expression in
                                ;the next triple (3 bytes).

                                ;将表达式的二进制值
                                ;保存到接下来的三元组（3个字节）中

        .3byte  exp1,exp2,expn  ;Stores the binary values
        .triple exp1,exp2,expn  ;of the list of expressions
                                ;in successive triples
                                ;(3 bytes).

                                ;将表达式列表的二进制值
                                ;存储到连续的三元组（3个字节）中
```

where:

其中：

```
        exp,    represent expressions that will occupy three
        exp1,   bytes of data. Each expression will be
        .       calculated as a 24-bit word expression.
        .       Multiple expressions must be
        expn    separated by commas.

                所表示的表达式将占据三个字节的数据。
                每个表达式都将被作为24位字进行计算。
                多个表达式用逗号分隔。
```

The `.3byte` or `.triple` directive is used to generate successive triples of binary data in the object module. (These directives are only available in assemblers supporting 24-bit addressing.)

`.3byte`和`.triple`指示符用于在目标模块中生成连续的三元组字数据。（这几个指示符只能用于支持24位地址的汇编器中。）

#### <a id="1.4.11"></a>1.4.11 `.4byte` and `.quad` Directive | `.4byte`和`.quad`指示符

格式：

```
        .4byte  exp             ;Stores the binary value
        .quad   exp             ;of the expression in
                                ;the next quad (4 bytes).

                                ;将表达式的二进制值
                                ;保存到接下来的四元组（4个字节）中

        .4byte  exp1,exp2,expn  ;Stores the binary values
        .quad   exp1,exp2,expn  ;of the list of expressions
                                ;in successive quads
                                ;(4 bytes).

                                ;将表达式列表的二进制值
                                ;存储到连续的四元组（4个字节）中
```

where:

其中：

```
        exp,    represent expressions that will occupy three
        exp1,   bytes of data. Each expression will be
        .       calculated as a 32-bit word expression.
        .       Multiple expressions must be
        expn    separated by commas.

                所表示的表达式将占据四个字节的数据。
                每个表达式都将被作为32位字进行计算。
                多个表达式用逗号分隔。
```

The `.4byte` or `.quad` directive is used to generate successive quads of binary data in the object module. (These directives are only available in assemblers supporting 32-bit addressing.)

`.4byte`和`.quad`指示符用于在目标模块中生成连续的四元组字数据。（这几个指示符只能用于支持32位地址的汇编器中。）

#### <a id="1.4.12"></a>1.4.12 `.blkb`, `.ds`, `.rmb`, and `.rs` Directives | `.blkb`、`.ds`、`.rmb`和`.rs`指示符

Format:

格式：

```
        .blkb   N       ;reserve N bytes of space
        .ds     N       ;reserve N bytes of space
        .rmb    N       ;reserve N bytes of space
        .rs     N       ;reserve N bytes of space

                        ;预留N字节的空间
```

The `.blkb`, `.ds`, `.rmb`, and `.rs` directives reserve byte blocks in the object module;

`.blkb`、`.ds`、`.rmb`和`.rs`指示符用于在目标模块中预留字节块。

#### <a id="1.4.13"></a>1.4.13 `.blkw`, `.blk3`, and `.blk4` Directives | `.blkw`、`.blk3`和`.blk4`指示符

Format:

格式：

```
        .blkw   N       ;reserve N words of space
                        ;预留N个字的空间

        .blk3   N       ;reserve N triples of space
                        ;预留N个三元组的空间

        .blk4   N       ;reserve N quads of space
                        ;预留N个四元组的空间
```

The `.blkw` directive reserves word blocks; the `.blk3` reserves 3 byte blocks(available in assemblers supporting 24-bit addressing); the `.blk4` reserves 4 byte blocks (available in assemblers supporting 32-bit addressing).

`.blkw`指示符用于预留字块；`.blk3`用于预留3字节块（只在支持24位寻址的汇编器中可用）；`.blk4`用于预留4字节块（只在支持32位寻址的汇编器中可用）。

#### <a id="1.4.14"></a>1.4.14 `.ascii`, `.str`, and `.fcc` Directives | `.ascii`、`.str`和`.fcc`指示符

Format:

格式：

```
        .ascii   /string/               or      或

        .ascii  ^/string/

        .fcc     /string/               or      或

        .fcc    ^/string/

        .str     /string/               or      或

        .str    ^/string/
```

where:

其中：

```
        string  is a string of printable ascii characters.

                是一个可打印的ASCII字符串。

        /  /    represent the delimiting characters. These
                delimiters may be any paired printing
                characters, as long as the characters are not
                contained within the string itself. If the
                delimiting characters do not match, the .ascii
                directive will give the <q> error.

                表示界定字符。该界定符可以是任何可打印的字符对，
                只要该字符未在字符串自身中出现即可。
                如果界定符不匹配，.ascii指示符会给出<q>错误。
```

The `.ascii`, `.fcc`, and `.str` directives place one binary byte of data for each character in the string into the object module.

`.ascii`、`.str`和`.fcc`指示符将每个字符的二进制字节数据放置到目标模块中。

#### <a id="1.4.15"></a>1.4.15 `.ascis` and `.strs` Directives | `.ascis`和`.strs`指示符

Format:

格式：

```
        .ascis   /string/               or      或

        .ascis  ^/string/

        .strs    /string/               or      或

        .strs   ^/string/
```

where:

其中：

```
        string  is a string of printable ascii characters.

                是一个可打印的ASCII字符串。

        /  /    represent the delimiting characters. These
                delimiters may be any paired printing
                characters, as long as the characters are not
                contained within the string itself. If the
                delimiting characters do not match, the .ascis
                and .strs directives will give the <q> error.

                表示界定字符。该界定符可以是任何可打印的字符对，
                只要该字符未在字符串自身中出现即可。
                如果界定符不匹配，.ascis和.strs指示符会给出<q>错误。
```

The `.ascis` and `.strs` directives place one binary byte of data for each character in the string into the object module. The last character in the string will have the high order bit set.

`.ascis`和`.strs`指示符将每个字符的二进制字节数据放置到目标模块中。串中的最后一个字符具有高位序。

#### <a id="1.4.16"></a>1.4.16 `.asciz` and `.strz` Directives | `.asciz`和`.strz`指示符

Format:

格式：

```
        .asciz   /string/               or      或

        .asciz  ^/string/

        .strz    /string/               or      或

        .strz   ^/string/
```

where:

其中：

```
        string  is a string of printable ascii characters.

                是一个可打印的ASCII字符串。

        /  /    represent the delimiting characters. These
                delimiters may be any paired printing
                characters, as long as the characters are not
                contained within the string itself. If the
                delimiting characters do not match, the .asciz
                and .strz directive will give the <q> error.

                表示界定字符。该界定符可以是任何可打印的字符对，
                只要该字符未在字符串自身中出现即可。
                如果界定符不匹配，.asciz和.strz指示符会给出<q>错误。
```

The `.asciz` and `.strz` directives place one binary byte of data for each character in the string into the object module. Following all the character data a zero byte is inserted to terminate the character string.


`.asciz`和`.strz`指示符将每个字符的二进制字节数据放置到目标模块中。所有字符数据之后会插入一个零，用于终止字符串。

#### <a id="1.4.17"></a>1.4.17 `.assume` Directive | `.assume`指示符

Format:

格式：

```
        .assume exp
```

where:

其中：

```
        exp     represents an absolute expression. If the
                evaluation of the expression results in a non
                zero value then an <e> error is reported and the
                text line is listed in the generated error.

                表示一个绝对表达式。如果该表达式的求值结果是非零值，
                则会报告<e>错误，并且该文本行会列在产生的错误中。
```

The `.assume` directive is useful to check assumptions about assembler values. (The `.assume` directive is identical in function to the `.error` directive, just perhaps more descriptive.)

`.assume`指示符用于检查对汇编器值做出的假设。（`.assume`指示符在功能上与`.error`指示符是一样的，不过更具描述性。）


#### <a id="1.4.18"></a>1.4.18 `.radix` Directive | `.radix`指示符

Format:

格式：

```
        .radix  character
```

where:

其中：

```
        character  represents a single character specifying the
        default radix to be used for succeeding numbers. The
        character may be any one of the following:

        character 表示一个单独的字符，用于在后续的数字中指定默认的进制。
        该字符可以是下列之一：

                B,b     Binary          二进制

                O,o     Octal           八进制
                Q,q

                D,d     Decimal         十进制
                'blank'                 “空”

                H,h     Hexidecimal     十六进制
                X,x
```

#### <a id="1.4.19"></a>1.4.19 `.even` Directive | `.even`指示符

Format:

格式：

```
        .even
```

The `.even` directive ensures that the current location counter contains an even boundary value by adding 1 if the current location is odd.

`.even`指示符确保当前位置计数器包含偶数边界值，如果当前位置是奇数，为其加1。

#### <a id="1.4.20"></a>1.4.20 `.odd` Directive | `.odd`指示符

Format:

格式：

```
        .odd
```

The `.odd` directive ensures that the current location counter contains an odd boundary value by adding one if the current location is even.

`.odd`指示符确保当前位置计数器包含奇数边界值，如果当前位置是偶数，为其加1。

#### <a id="1.4.21"></a>1.4.21 `.bndry` Directive | `.bndry`指示符

Format:

格式：

```
        .bndry  n
```

If the current location is not an integer multiple of `n` then the location counter is increased to the next integer multiple of `n`.

如果当前位置不是`n`的整数倍，则增加位置计数器的值到下一个`n`的整数倍。

As an example:

例如：

```
        .bndry  4
```

changes the current location to be at a multiple of `4`, a 4-byte boundary.

将当前位置设置为`4`的整数倍，一个4字节边界。

The boundary specifications are propagated to the linker as a boundary modulus, ie the smallest common boundary for all `.odd`, `.even`, and `.bndry` directives contained within the area. A boundary value of `1` is equivalent to `.odd` and a boundary value of `2` is equivalent to `.even`. Because areas are always assembled with an initial address of 0, an even address, both `.odd` and `.even` are modulus 2 boundaries.

边界规范会被作为边界模传递到连接器，即一个区域中所有的`.odd`、`.even`和`.bndry`指示符的最小公共边界。边界值`1`等价于`.odd`，边界值`2`等价于`.even`。因为区域总是被假设为从地址0起始，偶数地址、`.odd`和`.even`都是模2边界。

As an example, suppose there are two sections: a `CODE` section and a `DATA` section. The program code is written so that the data associated with this section of the program code follows immediately.

例如，假设这里有两个节——一个`CODE`节和一个`DATA`节。像下面这样编写程序代码，则与程序代码相关的数据可以与之紧密相连。

```
        .area   CODE

        ; Subroutine 1 Code
        ; Uses data having a boundary of 6

        ; 例程1的代码
        ; 使用边界为6的数据

        .area   DATA

        ; Subroutine 1 Data
        ; 例程1的数据

        .bndry  6
        .word   1, 2, 3

        ...

        .area   CODE

        ; Subroutine 2 Code
        ; Uses data having a boundary of 8

        ; 例程2的代码
        ; 使用边界为8的数据

        .area   DATA

        ; Subroutine 2 Data
        ; 例程2的数据

        .bndry  8
        .word   1, 2, 3, 4,
```

Since the `CODE` and `DATA` sections are assembled during a single assembly (also applies to include files) the the assembler compiles all `CODE` segments as a single area segment. The assembler also compiles all the `DATA` segments as a single area segment which has two `.bndry` directives and will have a boundary modulus of `24`. `24` is the smallest boundary divisible by `6` and `8` with no remainder. When the assembled file is linked the location of the data in the `DATA` area will be offset to an address which has a boundary modulus of 24.

由于`CODE`和`DATA`节是在第一遍（同时处理了包含文件）中进行汇编的，汇编器会将所有`CODE`段编译为一个单独的区域段。汇编器还会将所有的`DATA`段编译为一个单独的区域段，其中含有两个`.bndry`指示符，并且边界模是`24`。`24`是可以被边界值`6`和`8`除尽的最小值。当经过汇编的文件被连接时，`DATA`区域中数据的位置会被偏移到24的边界模上。

When multiple files containing the same area names (projects with multiple independently compiled files or library files) are linked together each area segment will be offset to match the segments boundary modulus.

当多个包含相同区域名字的文件（具有多个独立编译文件或二进制文件的项目）被连接到一起时，每个区域段都会被偏移，以匹配段边界模。

Boundary specifications will also be preserved when an area base address is specified with the `-b` linker option and/or the area is placed within a bank.

如果一个区域的基地址是通过`-b`连接器选项指定的，并且/或者被放置在一个阵列中，边界规范会被保留。

#### <a id="1.4.22"></a>1.4.22 `.area` Directive | `.area`指示符

Format:

格式：

```
        .area   name    [(options)]
```

where:

其中：

```
        name    represents the symbolic name of the program sec-
                tion. This name may be the same as any
                user-defined symbol or bank as the area names
                are independent of all symbols, labels, and
                banks.

                表示程序节的符号化名字。
                该名字可以与任何用户定义的符号或阵列的名字相同，
                因为区域的名字独立于所有符号、标号和阵列。

        options specify the type of program or data area:

                指定了程序或数据区域的类型：

                ABS     absolute (automatically invokes OVR)
                        绝对的（自动调用OVR）

                REL     relocatable
                        可重定位的

                OVR     overlay
                        覆盖的

                CON     concatenate
                        连续的

                NOPAG   non-paged area
                        未分页数据

                PAG     paged area
                        分页数据

        options specify a code or data segment:

                指定了代码或数据段：

                CSEG    Code segment
                        代码段

                DSEG    Data segment
                        数据段

        option  specifies the data area bank:

                指定数据区域阵列：

                BANK    Named collection of areas
                        区域的命名集合
```

The `.area` directive provides a means of defining and separating multiple programming and data sections. The `name` is the area label used by the assembler and the linker to collect code from various separately assembled modules into one section. The `name` may be from 1 to 79 characters in length.

`.area`指示符提供了一种方法，用以定义和分隔多个程序和数据节。`name`是区域的标号，由汇编器和连接器使用，用于从多个分离的汇编模块中将代码收集到一个节中。`name`的长度可以从1到79.

The `options` are specified within parenthesis and separated by commas as shown in the following example:

`options`在括号中指定，用逗号分隔，如下例所示：

```
        .area  TEST  (REL,CON)  ;This section is relocatable
                                ;and concatenated with other
                                ;sections of this program area.

                                ;该节是可重定位的，
                                ;并会与该程序区域中的其他节连接到一起

        .area  DATA  (REL,OVR)  ;This section is relocatable
                                ;and overlays other sections
                                ;of this program area.

                                ;该节是可重定位的，
                                ;并与该程序区域中的其他节重叠

        .area  SYS   (ABS,OVR)  ;(CON not allowed with ABS)
                                ;This section is defined as
                                ;absolute. Absolute sections
                                ;are always overlayed with
                                ;other sections of this program
                                ;area.

                                ;(CON不能与ABS一起使用)
                                ;该节被定义为绝对的。
                                ;绝对节总是与该程序区域中的其他节重叠

        .area  PAGE  (PAG)      ;This is a paged section. The
                                ;section must be on a 256 byte
                                ;boundary and its length is
                                ;checked by the linker to be
                                ;no larger than 256 bytes.
                                ;This is useful for direct page
                                ;areas.

                                ;该节是分页节。
                                ;该节必须在256字节边界上，并且
                                ;连接器会检查其长度不超过256字节。
                                ;该选项用于直接对区域进行分页。
```

The default area type is `REL|CON`; i.e. a relocatable section which is concatenated with other sections of code with the same area name. The `ABS` option indicates an absolute area. The `OVR` and `CON` options indicate if program sections of the same name will overlay each other (start at the same location) or be concatenated with each other (appended to each other).

默认的区域类型是`REL|CON`，即可重定位节，并连接到同名区域的其他节后。`ABS`选项指定绝对区域。`OVR`和`CON`选项指定程序节是与其他节重叠（起始于相同位置）还是连接（追加）到其他节之后。

The area can be specified as either a code segment, `CSEG`, or a data segment, `DSEG`. The `CSEG` and `DSEG` descriptors are useful when the microprocessor code and data unit allocations are unequal: e.g. the executable code uses an allocation of 2 bytes for each instruction and is addressed at an increment of 1 for every instruction, and the data uses an allocation of 1 byte for each element and is addressed at an increment of 1 for each data byte. The allocation units are defined by the architecture of the particular microprocessor.

区域可以指定为代码段`CSEG`或数据段`DSEG`。当微处理器的代码和数据单元是单独分配的时，`CSEG`和`DSEG`非常有用。例如，可执行代码可以为每条指令分配2字节，地址按照1指令递增。而对于数据，每个元素使用1字节，地址也是按1字节递增。单元的分配由特定微处理器的架构定义。

The `.area` directive also provides a means of specifying the bank this area is associated with. All areas associated with a particular bank are combined at link time into a block of code/data.

`.area`指示符还提供了一种为区域指定关联的阵列的方法。与一个特定阵列关联的所有区域，在连接时都会被合并到一个代码/数据块中。

The `CSEG`, `DSEG`, and `BANK` options are specified within the parenthesis as shown in the following examples:

`CSEG`、`DSEG`和`BANK`选项在括号中指定，如下例所示：

```
        .area   C_SEG   (CSEG,BANK=C1)
                                ;This is a code section
                                ;and is included in bank C1

                                ;代码节，包括在阵列C1中

        .area   D_SEG   (DSEG,BANK=D1)
                                ;This is a data section
                                ;and is included in bank D1.

                                ;数据节，包括在阵列D1中
```

Multiple invocations of the `.area` directive with the same name must specify the same options or leave the options field blank, this defaults to the previously specified options for this program area.

对`.area`的多次调用如果使用了相同的名字，则也必须指定相同的选项；或者在选项字段留空，这样会默认使用上一次指定的值。

The ASxxxx assemblers automatically provide two program sections:

ASxxxx汇编器自动提供两个程序节：

```
        '_CODE'         This is the default code/data area.
                        This program area is of type
                        (REL,CON,CSEG).

                        默认的代码/数据区域。
                        该程序区域的类型是 (REL,CON,CSEG)

        '_DATA'         This is the default optional data area.
                        This program area is of type
                        (REL,CON,DSEG).

                        默认的可选数据区域。
                        该程序区域的类型是 (REL,CON,DSEG)
```

The `.area` names and options are never case sensitive.

`.area`的名字和选项永远是大小写不敏感的。

#### <a id="1.4.23"></a>1.4.23 `.bank` Directive | `.bank`指示符

Format:

格式：

```
        .bank   name    [(options)]
```

where:

其中：

```
        name    represents the symbolic name of the bank sec-
                tion. This name may be the same as any
                user-defined symbol or area as the bank names
                are independent of all symbols, labels, and
                areas. The name may be from 1 to 79 characters
                in length.

                表示阵列的符号化名字。
                该名字可以与任何用户定义的符号或区域的名字相同，
                因为阵列的名字独立于所有符号、标号和区域。
                名字的长度是从1到79个字符。

        options specify the parameters of the bank:

                指定阵列的参数：

                BASE    base address of bank

                        阵列的基地址

                SIZE    maximum size of bank

                        阵列的最大大小
                
                FSFX    file suffix for this bank

                        该阵列的文件后缀

                MAP     NOICE mapping

                        NoICE映射
```

The `.bank` directive allows an arbitrary grouping of program and/or data areas to be communicated to the linker. The bank parameters are all optional and are described as follows:

`.bank`指示符允许和连接器通信，对程序和/或数据区域进行随意分组。阵列参数都是可选的，描述如下：

1. `BASE`, the starting address of the bank (default is 0) may be defined. This address can be overridden by using the linker `-b` option for the first area within the bank. The bank address is always specified in 'byte' addressing. A first area which is not 'byte' addressed (e.g. a processor addressed by a 'word' of 2 or more bytes) has the area address scaled to begin at the 'byte' address.\
\
`BASE`用于定义阵列的起始地址（默认是0）。如果为阵列中第一个区域使用了`-b`连接器选项，则该地址将被覆盖。阵列地址总是指定为“字节”寻址。如果其中某个区域不是“字节”寻址的（例如某处理器通过有2个或更多个字节的“字”进行寻址），其区域地址会被换算为“字节”寻址。

2. `SIZE`, the maximum length of the bank specified in bytes. The size is always specified in terms of bytes.\
\
`SIZE`指定了阵列的最大字节长度。这个大小总是按照字节指定。

3. `FSFX`, the file suffix to be used by the linker for this bank. The suffix may not contain embedded white space.\
\
`FSFX`是连接器为该阵列使用的文件后缀。改后缀不能包含任何嵌入的空白字符。

4. `MAP`, NOICE mapping parameter for this bank of code/data.\
\
`MAP`为该代码/数据阵列指定NoICE映射参数。

The `options` are specified within parenthesis and separated by commas as shown in the following example:

`options`在括号中指定，并用逗号进行分隔，如下例所示：

```
        .BANK  C1  (BASE=0x0100,SIZE=0x1000,FSFX=_C1)
                                ;This bank starts at 0x0100,
                                ;has a maximum size of 0x1000,
                                ;and is to be placed into
                                ;a file with a suffix of _C1

                                ;该阵列起始于0x0100，
                                ;最大大小0x1000，
                                ;并会被放到后缀为_C1的文件中
```

The parameters must be absolute (external symbols are not allowed.)

这些参数必须是绝对的（不允许外部符号）。

#### <a id="1.4.24"></a>1.4.24 `.org` Directive | `.org`指示符

Format:

格式：

```
        .org    exp
```

where:

其中：

```
        exp     is  an absolute expression that becomes the cur-
                rent location counter.

                是一个绝对表达式，将被作为当前位置计数器的值。
```

The `.org` directive is valid only in an absolute program section and will give a `<q>` error if used in a relocatable program area. The `.org` directive specifies that the current location counter is to become the specified absolute value.

`.org`指示符只在绝对程序区域中有效，如果用在可重定位程序区域中，就会给出`<q>`错误。`.org`指示符将当前位置计数器指定为特定的绝对值。

#### <a id="1.4.25"></a>1.4.25 `.globl` Directive | `.globl`指示符

Format:

格式：

```
        .globl  sym1,sym2,...,symn
```

where:

其中：

```
        sym1,           represent legal symbolic names.
        sym2,...        When multiple symbols are specified,
        symn            they are separated by commas.

                        表示合法的符号名字。
                        如果指定了多个符号，使用逗号进行分隔。
```

A `.globl` directive may also have a label field and/or a comment field.

`.globl`指示符也可以具有标号字段和/或注释字段。

The `.globl` directive is provided to export (and thus provide linkage to) symbols not otherwise defined as global symbols within a module. In exporting global symbols the directive `.globl J` is similar to:

```
        J == expression or J::
```

`.globl`指示符为模块中没有另外定义为全局符号的符号提供了导出（因此也提供了对该符号的连接）。在导出全局符号方面，指示符`.globl J`类似于：

```
        J == expression    或     J::
```

Because object modules are linked by global symbols, these symbols are vital to a program. All internal symbols appearing within a given program must be defined at the end of pass 1 or they will be considered undefined. The assembly directive (`-g`) can be invoked to make all undefined symbols global at the end of pass 1.

由于目标模块是通过全局符号连接的，所以这样的符号对于一个程序来说是至关重要的。一个程序中出现的所有内部符号，在第一遍结束时必须得到定义，否则就会被认为是未定义的。可以调用汇编提示符`-g`将所有未定义的符号在第一遍结束时变成全局符号。

The `.globl` directive and `==` construct can be overridden by a following `.local` directive.

`.globl`指示符和`==`结构可以被后面的`.local`指示符覆盖。


NOTE | 注意

The ASxxxx assemblers use the last occurring symbol specification in the source file(s) as the type shown in the symbol table and output to the `.rel` file.

ASxxxx汇编器使用最后一次在源文件中出现的符号规范作为符号表中显示的类型并输出到`.rel`文件中。

#### <a id="1.4.26"></a>1.4.26 `.local` Directive | `.local`指示符

Format:

格式：

```
        .local  sym1,sym2,...,symn
```

where:

其中：

```
        sym1,           represent legal symbolic names.
        sym2,...        When multiple symbols are specified,
        symn            they are separated by commas.

                        表示合法的符号名字。
                        如果指定了多个符号，使用逗号进行分隔。
```

A `.local` directive may also have a label field and/or a comment field.

`.local`指示符也可以具有标号字段和/或注释字段。

The `.local` directive is provided to define symbols that are local to the current assembly process. Local symbols are not effected by the assembler option `-a` (make all symbols global). In defining local symbols the directive `.local J` is similar to:

`.local`指示符用于定义局限于当前汇编过程中使用的符号。局部符号不会被汇编器选项`-a`（将所有符号定义为全局）影响。在定义局部符号方面，指示符`.local J`类似于：

```
      J =: expression
```

The `.local` directive and the `=:` construct are useful in defining symbols and constants within a header or definition file that contains many symbols specific to the current assembly process that should not be exported into the `.rel` output file. A typical usage is in the definition of SFRs (Special Function Registers) for a microprocessor.

当在头文件或def文件中定义符号和常量时，`.local`指示符和`=:`结构非常有用，这样的文件通常包含大量用于当前汇编过程中的符号，而这些符号不应该导出到`.rel`输出文件中。一个典型的应用就是为微处理器定义SFR（特殊功能寄存器）。

The `.local` directive and `=:` construct can be overridden by a following `.globl` directive.

`.local`指示符和`=:`结构可以被后来的`.globl`指示符覆盖。

NOTE | 注意

The ASxxxx assemblers use the last occurring symbol specification in the source file(s) as the type shown in the symbol table and output to the `.rel` file.

ASxxxx汇编器使用最后一次在源文件中出现的符号规范作为符号表中显示的类型并输出到`.rel`文件中。


### <a id="1.4.27"></a>1.4.27 `.equ`, `.gblequ`, and `.lclequ` Directives | `.equ`、`.gblequ`和`.lclequ`指示符

Format:

格式：

```
        sym1    .equ    expr    ; equivalent to sym1  = expr

                                ; 等价于sym1  = expr

        sym2    .gblequ expr    ; equivalent to sym2 == expr

                                ; 等价于sym2 == expr

        sym3    .lclequ expr    ; equivalent to sym3 =: expr

                                ; 等价于sym3 =: expr

        or      或

        .equ    sym1,   expr    ; equivalent to sym1  = expr

                                ; 等价于sym1  = expr

        .gblequ sym2,   expr    ; equivalent to sym2 == expr

                                ; 等价于sym2 == expr

        .lclequ sym3,   expr    ; equivalent to sym3 =: expr

                                ; 等价于sym3 =: expr

```

These alternate forms of equivalence are provided for user convenience.

这些等价的替补格式只是为了给用户提供方便。

### <a id="1.4.28"></a>1.4.28 `.if`, `.else`, and `.endif` Directives | `.if`、`.else`和`.endif`指示符

Format:

格式：

```
        .if     expr
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

条件汇编指示符允许你再汇编过程中，根据对测试条件的求值结果包括或排除一块源代码。

The range of true condition will be processed if the expression '`expr`' is not zero (i.e. true) and the range of false condition will be processed if the expression '`expr`' is zero (i.e false). The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.if`/`.else`/`.endif` constructions:

如果表达式`expr`不为零（即真），真条件范围会被处理。如果表达式`expr`为零（即假），假条件范围会被处理。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.if`/`.else`/`.endif`构造：

```
        .if     A-4             ;evaluate A-4
        .byte   1,2             ;insert bytes if A-4 is
        .endif                  ;not zero

                                ;对A-4进行求值
                                ;如果A-4非零，则插入两个字节

        .if     K+3             ;evaluate K+3
        .else
        .byte   3,4             ;insert bytes if K+3
        .endif                  ;is zero

                                ;对K+3进行求值
                                ;如果K+3为零，则插入两个字节

        .if     J&3             ;evaluate J masked by 3
        .byte   12              ;insert this byte if J&3
        .else                   ;is not zero
        .byte   13              ;insert this byte if J&3
        .endif                  ;is zero

                                ;求J对于3的掩码
                                ;如果J&3非零，插入字节12
                                ;如果J&3为零，插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.29"></a>1.4.29 `.iff`, `.ift`, and `.iftf` Directives | `.iff`、`.ift`和`.iftf`指示符

Format:

格式：

```
        .if     expr    ;'if' range Condition is
                        ;TRUE when expr is not zero

                        ; 如果expr非零，则if范围的条件为真

         .ift                                           ;}
         .      ;} range of true condition  | 真范围     ;}
         .iff                                           ;} if
         .      ;} range of false condition | 假范围     ;} block
         .iftf                                          ;}
         .      ;} unconditional range      | 无条件范围 ;} if块

        .else           ;'else' range Condition is
                        ;TRUE when expr is zero

                        ;如果expr为零，则else范围的条件为真

         .ift                                           ;}
         .      ;} range of true condition  | 真范围     ;}
         .iff                                            ;} else
         .      ;} range of false condition | 假范围     ;} block
         .iftf                                          ;}
         .      ;} unconditional range      | 无条件范围 ;} else块
        .endif
```

The subconditional assembly directives may be placed within conditional assembly blocks to indicate:

子条件汇编指示符可以放置在条件汇编块中，用于指出：

1. The assembly of an alternate body of code when the condition of the block tests false.\
\
当这一块测试为假时，汇编另一个替补代码块。

2. The assembly of non-contiguous body of code within the conditional assembly block, depending upon the result of the conditional test in entering the block.\
\
对一个条件汇编块中不相邻代码块的汇编，依赖于进入这个块似的条件测试结果。

3. The unconditional assembly of a body of code within a conditional assembly block.\
\
条件汇编块中的无条件汇编代码块。

The use of the `.iff`, `.ift`, and `.iftf` directives makes the use of the `.else` directive redundant.

对`.iff`、`.ift`和`.iftf`指示符的使用，使得对`.else`指示符的使用成为冗余。

Note that the implementation of the `.else` directive causes the `.if` tested condition to be complemented. The TRUE and FALSE conditions are determined by the `.if`/`.else` conditional state.

注意`.else`指示符的实现导致对`.if`条件的测试结果求补。真假条件由`.if`/`.else`条件的状态决定。

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of the `.iff`, `.ift`, or `.iftf` directives outside of a conditional block results in a `<i>` error code.

在条件块之外使用`.iff`、`.ift`或`.iftf`指示符会产生`<i>`错误码。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.30"></a>1.4.30 `.ifxx` Directives | `.ifxx`指示符

Additional conditional directives are available to test the value of an evaluated expression:

还有一些额外的条件指示符可以用于测试表达式的值：

```
        .ifne   expr    ; true if expr != 0  时，为真
        .ifeq   expr    ; true if expr == 0  时，为真
        .ifgt   expr    ; true if expr >  0  时，为真
        .iflt   expr    ; true if expr <  0  时，为真
        .ifge   expr    ; true if expr >= 0  时，为真
        .ifle   expr    ; true if expr <= 0  时，为真
```

Format:

格式：

```
        .ifxx   expr
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The range of true condition will be processed if the expression '`expr`' is not zero (i.e. true) and the range of false condition will be processed if the expression '`expr`' is zero (i.e false). The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifxx`/`.else`/`.endif` constructions:

如果表达式`expr`不为零（即真），真条件范围会被处理。如果表达式`expr`为零（即假），假条件范围会被处理。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifxx`/`.else`/`.endif`构造：

```
        .ifne   A-4             ;evaluate A-4
        .byte   1,2             ;insert bytes if A-4 is
        .endif                  ;not zero

                                ;对A-4进行求值
                                ;如果A-4非零，则插入两个字节

        .ifeq   K+3             ;evaluate K+3
        .byte   3,4             ;insert bytes if K+3
        .endif                  ;is zero

                                ;对K+3进行求值
                                ;如果K+3为零，则插入两个字节

        .ifne   J&3             ;evaluate J masked by 3
        .byte   12              ;insert this byte if J&3
        .else                   ;is not zero
        .byte   13              ;insert this byte if J&3
        .endif                  ;is zero

                                ;求J对于3的掩码
                                ;如果J&3非零，插入字节12
                                ;如果J&3为零，插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.31"></a>1.4.31 `.ifdef` Directive | `.ifdef`指示符

Format:

格式：

```
        .ifdef  sym
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The range of true condition will be processed if the symbol '`sym`' has been defined with a `.define` directive or '`sym`' is a variable with an assigned value else the false range will be processed. The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifdef`/`.else`/`.endif` constructions:

如果已经用`.define`指示符定义过了符号`sym`，或者`sym`是一个赋过值的变量，将会处理真条件范围，否则处理假条件范围。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifdef`/`.else`/`.endif`构造：

```
        .ifdef  sym$1           ;lookup symbol sym$1
        .byte   1,2             ;insert bytes if sym$1
        .endif                  ;is defined or
                                ;assigned a value

                                ;寻找符号sym$1
                                ;如果sym$1有定义或赋过值
                                ;则插入字节1和2

        .ifdef  sym$2           ;lookup symbol sym$2
        .else
        .byte   3,4             ;insert bytes if sym$1
        .endif                  ;is not defined and
                                ;not assigned a value

                                ;寻找符号sym$2
                                ;如果sym$2未定义或没有赋过值
                                ;则插入字节3和4

        .ifdef  sym$3           ;lookup symbol sym$3
        .byte   12              ;insert this byte if sym$3
        .else                   ;is defined/valued
        .byte   13              ;insert this byte if sym$3
        .endif                  ;is not defined/valued

                                ;寻找符号sym$3
                                ;如果sym$3有定义或赋过值
                                ;则插入字节12
                                ;如果sym$3未定义或没有赋过值
                                ;则插入字节13
```

Note that the default assembler configuration of case sensitive means the testing for a defined symbol is also case sensitive.

注意，汇编器的默认配置是区分大小写的，这意味着对符号定义的测试也是区分大小写的。

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.32"></a>1.4.32 `.ifndef` Directive | `.ifndef`指示符

Format:

格式：

```
        .ifndef sym
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the condition test.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The range of true condition will be processed if the symbol '`sym`' is not defined by a `.define` directive and a variable '`sym`' has not been assigned a value else the range of false condition will be processed. The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifndef`/`.else`/`.endif` constructions:

如果没有用`.define`指示符定义过符号`sym`，并且没有为变量`sym`赋过值，将会处理真条件范围，否则处理假条件范围。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifndef`/`.else`/`.endif`构造：

```
        .ifndef sym$1           ;lookup symbol sym$1
        .byte   1,2             ;insert bytes if sym$1 is
        .endif                  ;not defined and
                                ;not assigned a value

                                ;寻找符号sym$1
                                ;如果sym$1未定义且未赋过值
                                ;则插入字节1和2

        .ifndef sym$2           ;lookup symbol sym$2
        .else
        .byte   3,4             ;insert bytes if sym$1
        .endif                  ;is defined or
                                ;is assigned a value

                                ;寻找符号sym$2
                                ;如果sym$2有定义或赋过值
                                ;则插入字节3和4

        .ifndef sym$3           ;lookup symbol sym$3
        .byte   12              ;insert this byte if sym$3
        .else                   ;is not defined/valued
        .byte   13              ;insert this byte if sym$3
        .endif                  ;is defined/valued

                                ;寻找符号sym$3
                                ;如果sym$3未定义且未赋过值
                                ;则插入字节12
                                ;如果sym$3有定义或赋过值
                                ;则插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.33"></a>1.4.33 `.ifb` Directive | `.ifb`指示符

Format:

格式：

```
        .ifb    sym
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The conditional `.ifb` is most useful when used in macro definitions to determine if the argument is blank. The range of true condition will be processed if the symbol '`sym`' is blank. The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifb`/`.else`/`.endif` constructions:

`.ifb`指示符用于在宏定义中检测一个参数是否为空。如果符号`sym`为空，将会处理真条件范围，否则处理假条件范围。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifb`/`.else`/`.endif`构造：

```
        .ifb    sym$1           ;argument is not blank
        .byte   1,2             ;insert bytes if argument
        .endif                  ;is blank

                                ;检查参数是否为空
                                ;为空的话则插入字节1和2

        .ifb    sym$2           ;argument is not blank
        .else
        .byte   3,4             ;insert bytes if argument
        .endif                  ;is not blank

                                ;检查参数是否为空
                                ;不为空的话则插入字节3和4

        .ifb                    ;argument is blank
        .byte   12              ;insert this byte if
        .else                   ;argument is blank
        .byte   13              ;insert this byte if
        .endif                  ;argument not blank

                                ;检查参数是否为空
                                ;为空的话则插入字节12
                                ;不为空的话则插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.、

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.34"></a>1.4.34 `.ifnb` Directive | `.ifnb`指示符

Format:

格式：

```
                .ifnb   sym
                .                       ;}
                .                       ;} range of true condition
                .                       ;} 真条件范围
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;} 假条件范围
                .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The conditional `.ifnb` is most useful when used in macro definitions to determine if the argument is not blank. The range of true condition will be processed if the symbol '`sym`' is not blank. The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifnb`/`.else`/`.endif` constructions:

`.ifb`指示符用于在宏定义中检测一个参数是否非空。如果符号`sym`非空，将会处理真条件范围，否则处理假条件范围。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifnb`/`.else`/`.endif`构造：

```
        .ifnb   sym$1           ;argument is not blank
        .byte   1,2             ;insert bytes if argument
        .endif                  ;is not blank

                                ;检查参数是否非空
                                ;非空的话则插入字节1和2

        .ifnb   sym$2           ;argument is not blank
        .else
        .byte   3,4             ;insert bytes if argument
        .endif                  ;is blank

                                ;检查参数是否非空
                                ;为空的话则插入字节3和4

        .ifnb                   ;argument is blank
        .byte   12              ;insert this byte if
        .else                   ;argument is not blank
        .byte   13              ;insert this byte if
        .endif                  ;argument is blank

                                ;检查参数是否为空
                                ;非空的话则插入字节12
                                ;为空的话则插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.35"></a>1.4.35 `.ifidn` Directive | `.ifidn`指示符

Format:

格式：

```
        .ifidn  sym$1,sym$2
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The conditional `.ifidn` is most useful when used in macro definitions to determine if the arguments are identical. The range of true condition will be processed if the symbol '`sym$1`' is idendical to '`sym$2`' (i.e. the character strings for `sym$1` and `sym$2` are the same consistent with the case sensitivity flag). When this `if` statement occurs inside a macro where an argument substitution may be blank then an argument should be delimited with the form `/symbol/` for each symbol. The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifidn`/`.else`/`.endif` constructions:

`.ifidn`指示符用于在宏定义中检测一个两个参数是否相等。如果符号`sym$1`和`sym$1`相等（即考虑是否区分大小写的标志下，`sym$1`和`sym$2`的字符串具有相同的构造），将会处理真条件范围，否则处理假条件范围。当宏中出现这个`if`语句时，参数可能会被空白替换，因此参数参数中的每一个符号都应该用`/symbol/`的形式界定起来。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifidn`/`.else`/`.endif`构造：

```
        .ifidn  sym$1,sym$1     ;arguments are the same
        .byte   1,2             ;insert bytes if arguments
        .endif                  ;are the sane

                                ;检查参数是否相等
                                ;相等的话则插入字节1和2

        .ifidn  sym$1,sym$2     ;arguments are not the same
        .else
        .byte   3,4             ;insert bytes if arguments
        .endif                  ;are not the same

                                ;检查参数是否相等
                                ;不相等的话则插入字节3和4

        .ifidn  sym$3,sym$3     ;arguments are the same
        .byte   12              ;insert this byte if
        .else                   ;arguments are the same
        .byte   13              ;insert this byte if
        .endif                  ;arguments are not the same

                                ;检查参数是否相等
                                ;相等的话则插入字节12
                                ;不相等的话则插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.36"></a>1.4.36 `.ifdif` Directive | `.ifdif`指示符

Format:

格式：

```
        .ifdif  sym$1,sym$2
        .                       ;}
        .                       ;} range of true condition
        .                       ;} 真条件范围
        .else
        .                       ;}
        .                       ;} range of false condition
        .                       ;} 假条件范围
        .endif
```

The conditional assembly directives allow you to include or exclude blocks of source code during the assembly process, based on the evaluation of the test condition.

在汇编过程中，条件汇编指示符允许你根据测试条件的求值结果包含或排除源代码块。

The conditional `.ifdif` is most useful when used in macro definitions to determine if the arguments are different. The range of true condition will be processed if the symbol '`sym$1`' is different from '`sym$2`' (i.e. the character strings for `sym$1` and `sym$2` are the not the same consistent with the case sensitivity flag). When this `if` statement occurs inside a macro where an argument substitution may be blank then an argument should be delimited with the form `/symbol/` for each symbol. The range of true condition is optional as is the `.else` directive and the range of false condition. The following are all valid `.ifdif`/`.else`/`.endif` constructions:

`.ifdif`指示符用于在宏定义中检测一个两个参数是否不相等。如果符号`sym$1`和`sym$1`不相等（即考虑是否区分大小写的标志下，`sym$1`和`sym$2`的字符串具有不相同的构造），将会处理真条件范围，否则处理假条件范围。当宏中出现这个`if`语句时，参数可能会被空白替换，因此参数参数中的每一个符号都应该用`/symbol/`的形式界定起来。在这里真条件范围是可选的，因为出现了`.else`指示符和假条件范围。下面的例子都是合法的`.ifdif`/`.else`/`.endif`构造：

```
        .ifdif  sym$1,sym$2     ;arguments are different
        .byte   1,2             ;insert bytes if arguments
        .endif                  ;are different

                                ;检查参数是否不相等
                                ;不相等的话则插入字节1和2

        .ifdif  sym$1,sym$1     ;arguments are identical
        .else
        .byte   3,4             ;insert bytes if arguments
        .endif                  ;are different

                                ;检查参数是否不相等
                                ;相等的话则插入字节3和4

        .ifdif  sym$1,sym$3     ;arguments are different
        .byte   12              ;insert this byte if
        .else                   ;arguments are different
        .byte   13              ;insert this byte if
        .endif                  ;arguments are identical

                                ;检查参数是否不相等
                                ;不相等的话则插入字节12
                                ;相等的话则插入字节13
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.37"></a>1.4.37 Alternate `.if` Directive Forms | 其他`.if`指示符格式

Format:

格式：

```
        .if     cnd(,)  arg1(, arg2)
```

where the `cnd` (followed by an optional comma) may be any of the following:

其中`cnd`（后跟可选的逗号）可以下列任何一种：

```
        -------------------------------------------------------
         condition              Assemble
        (complement)    Args    Block if:
        -------------------------------------------------------
           条件                 编译该代码块，
        （逆条件）       参数    如果：
        -------------------------------------------------------
        eq   ( ne )     expr    equal to zero
                                (not equal to zero)

                                等于零 （不等于零）

        gt   ( le )     expr    greater than zero
                                (less than or equal to zero)

                                大于零 （小于或等于零）

        lt   ( ge )     expr    less than zero
                                (greater than or equal to zero)

                                小于零 （大于或等于零）

        def  ( ndef )   symbol  .define'd or user set
                                (not .define'd or user set)

                                有定义 （没有定义）

        b    ( nb )     macro   argument present
                        symbol  (argument not present)

                                参数出现了 （参数没出现）

        idn  ( dif )    macro   arguments identical
                        symbol  (arguments not identical)

                                参数相等 （参数不等）

        f    ( t )      -----   only within a .if/.else/.endif
                                conditional block

                                只用于.if/.else/.endif条件块里

        tf              -----   only within a .if/.else/.endif
                                conditional block

                                只用于.if/.else/.endif条件块里
```

All `.if`/`.else`/`.endif` directives are limited to a maximum nesting of 10 levels.

全部`.if`/`.else`/`.endif`指示符最多嵌套10层。

The use of a `.else` directive outside a `.if`/`.endif` block will generate an `<i>` error. Assemblies having unequal `.if` and `.endif` counts will cause an `<i>` error.

在`.if`/`.endif`块之外使用`.else`指示符会产生`<i>`错误。汇编程序具有不等量的`.if`和`.endif`会导致`<i>`错误。

### <a id="1.4.38"></a>1.4.38 Immediate Conditional Assembly Directives | 立即条件汇编指示符

The immediate conditional assembly directives allow a single line of code to be assembled without using a `.if`/`.else`/`.endif` construct. All of the previously described conditionals have immediate equivalents.

立即条件汇编指示符允许单独一行代码被汇编，而无需使用`.if`/`.else`/`.endif`构造。前面介绍的所有条件都有等价的立即形式。

Format:

格式：

```
        .iif            arg(,)          line_to_assemble
        .iifeq          arg(,)          line_to_assemble
        .iifne          arg(,)          line_to_assemble
        .iifgt          arg(,)          line_to_assemble
        .iifle          arg(,)          line_to_assemble
        .iifge          arg(,)          line_to_assemble
        .iiflt          arg(,)          line_to_assemble
        .iifdef         arg(,)          line_to_assemble
        .iifndef        arg(,)          line_to_assemble

        .iifb        (,)arg(,)          line_to_assemble
        .iifnb       (,)arg(,)          line_to_assemble
        .iifidn      (,)arg1,arg2(,)    line_to_assemble
        .iifdif      (,)arg1,arg2(,)    line_to_assemble

        .iiff                           line_to_assemble
        .iift                           line_to_assemble
        .iiftf                          line_to_assemble
```

Alternate Format:

其他格式：

```
        .iif            arg(,)          line_to_assemble
        .iif    eq      arg(,)          line_to_assemble
        .iif    ne      arg(,)          line_to_assemble
        .iif    gt      arg(,)          line_to_assemble
        .iif    le      arg(,)          line_to_assemble
        .iif    ge      arg(,)          line_to_assemble
        .iif    lt      arg(,)          line_to_assemble
        .iif    def     arg(,)          line_to_assemble
        .iif    ndef    arg(,)          line_to_assemble

        .iif    b    (,)arg(,)          line_to_assemble
        .iif    nb   (,)arg(,)          line_to_assemble
        .iif    idn  (,)arg1,arg2(,)    line_to_assemble
        .iif    dif  (,)arg1,arg2(,)    line_to_assemble

        .iiff                           line_to_assemble
        .iift                           line_to_assemble
        .iiftf                          line_to_assemble
```

The `(,)` indicates an optional comma.

`(,)`表示一个可选的逗号。

The `.iif` types `b`, `n`, `idn`, and `dif` require the commas if the argument(s) may be blank. These commas may be removed if the arguments are delimited with the form `^/symbol/` for each symbol.

在`.iif`类型`b`、`n`、`idn`和`dif`中，如果参数可能为空，则要求必须有逗号。如果参数中的每个符号都用`^/synbol/`的形式界定了，则可以移除逗号。

The immediate conditional directives donot change the `.if`/`.else`/`.endif` nesting level.

立即条件指示符不改变`.if`/`.else`/`.endif`的嵌套层级。

### <a id="1.4.39"></a>1.4.39 `.include` Directive | `.include`指示符

Format:

格式：

```
        .include         /string/               or   或

        .include        ^/string/
```

where:

其中：

```
        string  represents  a string that is the file specifica-
                tion of an ASxxxx source file.

                表示一个指定了ASxxxx源文件的字符串。

        /  /    represent the delimiting characters. These
                delimiters may be any paired printing
                characters, as long as the characters are not
                contained within the string itself. If the
                delimiting characters do not match, the .include
                directive will give the <q> error.

                表示界定字符。该界定符可以是任何的可打印字符对，
                只要没有包含在字符串自身中即可。
                如果界定字符不匹配，.include指示符会给出<q>错误。
```

The `.include` directive is used to insert a source file within the source file currently being assembled. When this directive is encountered, an implicit `.page` directive is issued. When the end of the specified source file is reached, an implicit `.page` directive is issued and input continues from the previous source file. The maximum nesting level of source files specified by a `.include` directive is five.

`.include`指示符用于向当前正在编译的源文件中插入另一个源文件。当遇到该指示符，会发出一个隐含的`.page`指示符。当遇到指定的源文件结尾时，有会发出另一个隐含的`.page指示符，然后从前一个源文件继续。`.include`指示符能指定的源文件嵌套层级最大为五。

The total number of separately specified `.include` files is unlimited as each `.include` file is opened and then closed during each pass made by the assembler.

独立指定的`.include`文件总数时没有限制的，因为汇编器在每一遍汇编中，会打开`.include`指定文件，处理完立即关闭。

The default directory path, if none is specified, for any `.include` file is the directory path of the current file. For example: if the current source file, `D:\proj\file1.asm`, includes a file specified as `"include1"` then the `file D:\proj\include1.asm` is opened.

`.include`文件的默认目录，如果为指定的话，就是当前文件的目录。例如，如果当前源文件为`D:\proj\file1.asm`，包含文件为`"include1"`，则文件`file D:\proj\include1.asm`会被打开。

### <a id="1.4.39.1"></a>1.4.39.1 Including Files In Windows/DOS | Windows/DOS上的包含文件

Graphical Illustration of Include File Locations for the following command line entry:

这是对下面的命令行关于包含文件位置的图形化演示：

```
        __> bin\ascheck -l -o -s  obj\prjct.rel   src\prjct\prjct.asm
```

```
                                  /-----------------------------------------------------------------------\
                                  |                                           (rooted)                    |
                      _____       |           _____                                                       |
                     |     |      |          |     |                                                      |
                  ---| inc |  <---/       ---| bin |                                                      |
                  |  |_____|              |  |_____|                                                      |
                  |     |                 |     |                                                         |
                  |     \___ inc4.asm     |     \___ ascheck.exe                                          |
                  |                       |                                                               |
                  |                       |                                                               |
          _____   |   _____       _____   |   _____       _____                                           |
         |     |  |  |     |     |     |  |  |     |     |     |      (in prjct.asm directory)            |
         | C:\ |-----| ..\ |-----| __> |--+--| src |-----|prjct|  <-------------------------------\       |
         |_____|     |_____|  |  |_____|  |  |_____|  |  |_____|                                  |       |
                              |           |           |     |                .include "inc1.asm" -/       |
                              |     ^     |           |     \___ prjct.asm   .include "C:\inc\inc4.asm" --/
                              |     |     |           |     \___ inc1.asm    .include "..\inc\inc3.asm" -------\
             Current          |     |     |           |   _____              .include "src\inc\inc2.asm" -\    |
             Working  ------> | >---/     |           |  |     |                                          |    |
            Directory         |           |           ---| inc |  <---------------------------------------/    |
                              |           |              |_____|   (relative to current working directory)     |
                              |           |                 |                                                  |
                              |           |                 \___ inc2.asm                                      |
                              |           |   _____                                                            |
                              |           |  |     |                                                           |
                              |           ---| obj |                                                           |
                              |              |_____|                                                           |
                              |                 |                                                              |
                              |                 \___ .REL, .SYM, .LST, .HLR                                    |
                              |                                                                                |
                              |   _____                                                                        |
                              |  |     |                          (relative to current working directory)      |
                              ---| inc |  <--------------------------------------------------------------------/
                                 |_____|
                                    |
                                    \___ inc3.asm
```

### <a id="1.4.39.2"></a>1.4.39.2 Including Files in Linux | Linux上的包含文件

Graphical Illustration of Include File Locations for the following command line entry:

这是对下面的命令行关于包含文件位置的图形化演示：

```
        __$ bin/ascheck -l -o -s  obj/prjct.rel   src/prjct/prjct.asm
```

```
                                  /-----------------------------------------------------------------------\
                                  |                                           (rooted)                    |
                      _____       |           _____                                                       |
                     |     |      |          |     |                                                      |
                  ---| inc |  <---/       ---| bin |                                                      |
                  |  |_____|              |  |_____|                                                      |
                  |     |                 |     |                                                         |
                  |     \___ inc4.asm     |     \___ ascheck                                              |
                  |                       |                                                               |
                  |                       |                                                               |
          _____   |   _____       _____   |   _____       _____                                           |
         |     |  |  |     |     |     |  |  |     |     |     |      (in prjct.asm directory)            |
         |  /  |-----| ../ |-----| __$ |--+--| src |-----|prjct|  <-------------------------------\       |
         |_____|     |_____|  |  |_____|  |  |_____|  |  |_____|                                  |       |
                              |           |           |     |                .include "inc1.asm" -/       |
                              |     ^     |           |     \___ prjct.asm   .include "/inc/inc4.asm" ----/
                              |     |     |           |     \___ inc1.asm    .include "../inc/inc3.asm" -------\
             Current          |     |     |           |   _____              .include "src/inc/inc2.asm" -\    |
             Working  ------> | >---/     |           |  |     |                                          |    |
            Directory         |           |           ---| inc |  <---------------------------------------/    |
                              |           |              |_____|   (relative to current working directory)     |
                              |           |                 |                                                  |
                              |           |                 \___ inc2.asm                                      |
                              |           |   _____                                                            |
                              |           |  |     |                                                           |
                              |           ---| obj |                                                           |
                              |              |_____|                                                           |
                              |                 |                                                              |
                              |                 \___ .REL, .SYM, .LST, .HLR                                    |
                              |                                                                                |
                              |   _____                                                                        |
                              |  |     |                          (relative to current working directory)      |
                              ---| inc |  <--------------------------------------------------------------------/
                                 |_____|
                                    |
                                    \___ inc3.asm
```


### <a id="1.4.40"></a>1.4.40 `.define` and `.undefine` Directives | `.define`和`.undefine`指示符

Format:

格式：

```
        .define        keyword  /string/        or    或

        .define        keyword ^/string/

        .undefine      keyword
```

where:

其中：

```
        keyword is the substitutable string which must start
                with a letter and may contain any combination of
                digits and letters.

                时待替换的字符串，必须以字母开头，
                可以包含字母和数字的组合

        string  represents a string that is substituted for the
                keyword. The string may contain any sequence of
                characters including white space.

                表示替换keyword的字符串。
                该字符串可以包含任何字符序列，包括空白字符。

        /  /    represent the delimiting characters. These
                delimiters may be any paired printing
                characters, as long as the characters are not
                contained within the string itself. If the
                delimiting characters do not match, the .define
                directive will give the <q> error.

                表示界定字符。界定符可以是任何可打印字符对，
                只要没有包含在字符串自身中即可。
                如果界定字符不匹配，.define指示符会给出<q>错误。
```

The `.define` directive specifies a user defined string which is substituted for the `keyword`. The substitution string may itself contain other keywords that are substitutable. The assembler resumes the parse of the line at the point the keyword was found. Care must be excersized to avoid any circular references within `.define` directives, otherwise the assembler may enter a 'recursion runaway' resulting in an `<s>` error.

`.define`指示符为`keyword`制定了替换字符串。替换字符串自身可能包含其他可以替换的关键字。汇编器找到关键字的定义后会恢复对这一行的分析。必须特别小心，要避免在`.define`指示符中出现循环引用，否则编译器会陷入“递归失控”状态，并产生`<s>`错误。

The `.undefine` directive removes the `keyword` as a substitutable string. No error is returned if the `keyword` was not defined.

`.undefine`指示符移除对`keyword`的字符串替换。如果`keyword`未定义，不会返回错误。

### <a id="1.4.41"></a>1.4.41 `.setdp` Directive | `.setdp`指示符

Format:

格式：

```
        .setdp [base [,area]]
```

The set direct page directive has a common format in all the assemblers supporting a paged mode. The `.setdp` directive is used to inform the assembler of the current direct page region and the offset address within the selected area. The normal invocation methods are:

在所有支持分页模式的汇编器中，设置直接分页指示符有通用的格式。`.setdp`指示符用于告知汇编器当前的直接分页区域极其在选中区域中的偏移地址。通常的调用方式为：

```
        .area   DIRECT  (PAG)
        .setdp

        or      或

        .setdp  0,DIRECT
```

for all the 68xx microprocessors (the 6804 has only the paged ram area). The commands specify that the direct page is in area `DIRECT` and its offset address is `0` (the only valid value for all but the 6809 microprocessor). Be sure to place the `DIRECT` area at address `0` during linking. When the base address and area are not specified, then zero and the current area are the defaults. If a `.setdp` directive is not issued the assembler defaults the direct page to the area "`_CODE`" at offset `0`.

此法适用于所有68xx微处理器（6804只有分页的ram区域）。该命令指定直接分页位于`DIRECT`区域，且其偏移地址为`0`（处6809微处理器之外唯一的有效值）。在连接时，要确保将`DIRECT`区域放置在地址`0`。如果没有指定基地址和区域，则默认值为0以及当前区域。如果没有使用`.setdp`指示符，则汇编器默认直接分页位于偏移量`0`处的`_CODE`区域。

The assembler verifies that any local variable used in a direct variable reference is located in this area. Local variable and constant value direct access addresses are checked to be within the address range from 0 to 255.

汇编器会验证直接变量引用中使用的局部变量都位于这一区域。局部变量和常量值直接访问地址也会被检查，确保位于0到255的地址范围。

External direct references are assumed by the assembler to be in the correct area and have valid offsets. The linker will check all direct page relocations to verify that they are within the correct area.

汇编器会假设外部直接引用位于正确的区域并且具有有效的偏移量。连接器会检查所有的直接分页重定位，验证它们位于正确的区域。

The 6809 microprocessor allows the selection of the direct page to be on any 256 byte boundary by loading the appropriate value into the `dp` register. Typically one would like to select the page boundary at link time, one method follows:

6809微处理器允许通过向`dp`寄存器加载适当的值，来选择位于任何256字节边界的直接分页。通常可以在连接时选择页边界，方法如下：

```
        .area   DIRECT  (PAG)   ; define the direct page
        .setdp
                                ; 定义直接分页
        .
        .
        .
        .area   PROGRAM
        .
        ldd     #DIRECT         ; load the direct page register
        tfr     a,dp            ; for access to the direct page

                                ; 记载直接分页寄存器
                                ; 用于访问直接分页
```

At link time specify the base and global equates to locate the direct page:

在连接时指定基地址和全局等式，以定位直接分页：

```
        -b DIRECT = 0x1000
        -g DIRECT = 0x1000
```

Both the area address and offset value must be specified (area and variable names are independent). The linker will verify that the relocated direct page accesses are within the direct page.

区域地址和偏移值都必须指定（区域和变量名是相互独立的）。连接器会验证对重定位的直接分页的访问是位于直接分页中的）。

The preceeding sequence could be repeated for multiple paged areas, however an alternate method is to define a non-paged area and use the `.setdp` directive to specify the offset value:

前面的过程可以重复应用于多个分页的区域，不过另一种可选的方法是，定义一个非分页区域，然后使用`.setdp`指示符指定偏移值：

```
        .area   DIRECT          ; define non-paged area
                                ; 定义非分页区域
        .
        .
        .
        .area   PROGRAM
        .
        .setdp  0,DIRECT        ; direct page area
        ldd     #DIRECT         ; load the direct page register
        tfr     a,dp            ; for access to the direct page

                                ; 直接分页区域
                                ; 加载直接分页寄存器
                                ; 用于访问直接分页
        .
        .
        .setdp  0x100,DIRECT    ; direct page area
        ldd     #DIRECT+0x100   ; load the direct page register
        tfr     a,dp            ; for access to the direct page

                                ; 直接分页区域
                                ; 加载直接分页寄存器
                                ; 用于访问直接分页
```

The linker will verify that subsequent direct page references are in the specified area and offset address range. It is the programmers responsibility to load the `dp` register with the correct page segment corresponding to the `.setdp` base address specified.

连接器会验证后续的直接分页引用位于指定的区域和编译地址范围中。而将与`.setdp`指定的基地址相对应的正确的分页段加载到`dp`寄存器，则是程序员的职责。

For those cases where a single piece of code must access a defined data structure within a direct page and there are many pages, define a dumby direct page linked at address 0. This dumby page is used only to define the variable labels. Then load the `dp` register with the real base address but donot use a `.setdp` directive. This method is equivalent to indexed addressing, where the `dp` register is the index register and the direct addressing is the offset.

当一段代码需要访问直接分页中定义的一个数据结构，并且有很多页时，可以定义一个哑元直接分页，连接到地址0。该哑元页只用于定义变量标号。然后将真实基地址加载到`dp`寄存器，但不使用`.setdp`指示符。这个方法等价于索引寻址，其中`dp`寄存器是索引寄存器，而直接地址是偏移量。

### <a id="1.4.42"></a>1.4.42 `.16bit`, `.24bit`, and `.32bit` Directives | `.16bit`、`.24bit`和`.32bit`指示符

Format:

格式：

```
        .16bit          ;specify 16-bit addressing
                        ;指定16位地址

        .24bit          ;specify 24-bit addressing
                        ;指定24位地址

        .32bit          ;specify 32-bit addressing
                        ;指定32位地址
```

The `.16bit`, `.24bit`, and `.32bit` directives are special directives for assembler configuration when default values are not used.

`.16bit`、`.24bit`和`.32bit`指示符是当没有使用默认值时位汇编器提供配置值的特殊指示符。

### <a id="1.4.43"></a>1.4.43 `.msb` Directive | `.msb`指示符

Format:

格式：

```
        .msb    n
```

The `.msb` directive is only available in selected assemblers which support 24 or 32-bit addressing.

`.msb`指示符只能用于支持24位或32位寻址的汇编器。

The assembler operator '`>`' selects the upper byte (MSB) when included in an assembler instruction. The default assembler mode is to select bits `<15:8>` as the MSB. The `.msb` directive allows the programmer to specify a particular byte as the 'MSB' when the address space is larger than 16-bits.

汇编操作符`>`用于选中一条汇编指令中包含的（数据）的高字节（MSB）。在汇编器的默认模式下会将`<15:8>`位选中为MSB。`.msb`指示符允许汇编器再地址空间大于16位的时候，选中特定的字节作为MSB。

The assembler directive `.msb n` configures the assembler to select a particular byte as MSB. Given a 32-bit address of `MNmn` (`M(3)` is `<31:24>`, `N(2)` is `<23:16>`, `m(1)` is `<15:8>`, and `n(0)` is `<7:0>`) the following examples show how to select a particular address byte:

汇编器指示符`.msb n`配置汇编器使其选择特定字节作为MSB。给定一个32位地址`MNmn`（`M(3)`为`<31:24>`，`N(2)`为`<23:16>`，`m(1)`为`<15:8>`，`n(0)`为`<7:0>`），下面的例子展示了如何选择特定的地址字节：

```
        .msb 1          ;select byte 1 of address
                        ;<M(3):N(2):m(1):n(0)>
        LD A,>MNmn      ;byte m <15:8> ==>> A
        ...
                        ;选择字节1，
                        ;字节 m <15:8> ==>> A

        .msb 2          ;select byte 2 of address
                        ;<M(3):N(2):m(1):n(0)>
        LD A,>MNmn      ;byte N <23:16> ==>> A
        ...
                        ;选择字节2，
                        ;字节 N <23:16> ==>> A

        .msb 3          ;select byte 3 of address
                        ;<M(3):N(2):m(1):n(0)>
        LD A,>MNmn      ;byte M <31:24> ==>> A
        ...
                        ;选择字节3，
                        ;字节 M <31:24> ==>> A
```

### <a id="1.4.44"></a>1.4.44 `.lohi` and `.hilo` Directives | `.lohi`和`.hilo`指示符

Format:

格式：

```
        .lohi           ;specify LSB first output
                        ;指定LSB先行输出

        .hilo           ;specify MSB first output
                        ;指定MSB先行输出
```

The `.lohi` and `.hilo` directives are special directives for assembler output configuration. These directives are currently only enabled in assembler 'ascheck'.

`.lohi`和`.hilo`指示符用于指定汇编器的输出。这两个指示符目前只在汇编器ascheck中可用。

An `<m>` error will be generated if the `.lohi` and `.hilo` directives are both used within the same assembly source file.

挡在同一个汇编源文件中同时使用了`.lohi`和`.hilo`指示符，会产生`<m>`错误。

### <a id="1.4.45"></a>1.4.45 `.end` Directive | `.end`指示符

Format:

格式：

```
        .end

        .end    exp
```

where:

其中：

```
        exp     represents any expression, including constants,
                symbols, or labels.

                表示任意表达式，包括常量、符号或标号。
```

The `.end` directive is used to specify a code entry point to be included in the linker output file. Review the I86 and S record formats described in the linker section for details.

`.end`指示符用于指定包含在连接器输出文件中的代码入口点。细节参见连接器章节中对I86和S记录格式的介绍。

The `.end` directive without an expression is ignored.

不带表达式的`.end`指示符会被忽略。

        1.5  INVOKING ASXXXX


           Starting  an  ASxxxx assembler without any arguments provides
        the following option list and then exits:

        Usage: [-Options] [-Option with arg] file
        Usage: [-Options] [-Option with arg] outfile file1 [file2 ...]
          -h   or NO ARGUMENTS  Show this help list
        Output:
          -l   Create list   file/outfile[.lst]
          -o   Create object file/outfile[.rel]
          -s   Create symbol file/outfile[.sym]
        Listing:
          -d   Decimal listing
          -q   Octal   listing
          -x   Hex     listing (default)
          -b   Display .define substitutions in listing
          -bb  and display without .define substitutions
          -c   Disable instruction cycle count in listing
          -f   Flag relocatable references by  `   in listing file
          -ff  Flag relocatable references by mode in listing file
          -p   Disable automatic listing pagination
          -u   Disable .list/.nlist processing
          -w   Wide listing format for symbol table
        Assembly:
          -i   Insert assembler line before input file(s)
          -v   Enable out of range signed / unsigned errors
        Symbols:
          -a   All user symbols made global
          -g   Undefined symbols made global
          -z   Disable case sensitivity for symbols
        "Debugging:
          -j   Enable NoICE Debug Symbols
          -y   Enable SDCC  Debug Symbols



           The ASxxxx assemblers are command line oriented.  Most sytems
        require the option(s) and file(s) arguments to follow the ASxxxx
        assembler name:

          as6809 -[Options] file

          as6809 [-Options] outfile file1 [file2 ...]


        Some  systems  may  request the arguments after the assembler is
        started at a system specific prompt:


        THE ASSEMBLER                                          PAGE 1-58
        INVOKING ASXXXX



          as6809
          argv: -[Options] file

          as6809
          argv: [-Options] outfile file1 [file2 ...]


        The ASxxxx options in some more detail:

                -h      List the ASxxxx options

            Output:
                -l      create list   output (out)file.lst

                        If -s (symbol table output) is not
                        specified the symbol table is included
                        at the end of the listing file.

                -o      create object output (out)file.rel
                -s      create symbol output (out)file.sym

            Listing:
                -d      decimal listing
                -q      octal   listing
                -x      hex     listing (default)

                        The listing radix affects the
                        .lst, .rel, .hlr, and .sym files.

                -b      display .define substitutions in listing

                        If a .define substitution has been applied
                        to an assembler source line the source
                        line is printed with the substitution.

                -bb     and display without .define substitutions

                        If a .define substitution has been applied
                        to an assembler source line the source
                        line is first printed without substitution
                        followed by the line with the substitution.

                -c      Disable instruction cycle count in listing

                        This option overrides the listing option
                        'cyc' in the .list and .nlist directives.
                        Instruction cycle counts cannot be enabled
                        if the -c option is specified.


        THE ASSEMBLER                                          PAGE 1-59
        INVOKING ASXXXX



                -f      by  `   in the listing file
                -ff     by mode in the listing file

                        Relocatable modes are flagged by byte
                        position (LSB, Byte 2, Byte 3, MSB)
                        *nMN    paged,
                        uvUV    unsigned,
                        rsRS    signed,
                        pqPQ    program counter relative.

                -p      disable listing pagination

                        This option inhibits the generation
                        of a form-feed character and its
                        associated page header in the
                        assembler listing.

                -u      disable .list/.nlist processing

                        This option disables all .list and
                        .nlist directives. The listing mode
                        is .list with the options err, loc,
                        bin, eqt, cyc, lin, src, pag, lst,
                        and md. The options cyc and pag are
                        overridden by the -c and -p command
                        line options.

                -w      wide listing format for symbol table

            Assembly:
                -i      Insert assembler line before input file(s)

                        This option inserts an assembly source
                        line before the first file to be assembled.
                        e.g.:  -i BUILD=2
                        If the insert contains white space then
                        delimit the insert.  Inserted lines are
                        by default not listed. To list an inserted
                        line preceed the insert with a .list insert.
                        e.g.:   -i .list -i BUILD=2

                -v      Enable out of range signed / unsigned errors

                        This option enables checking for out of
                        range signed / unsigned values in symbol
                        equates and arithmetic operations.  This
                        option has some ambiguities as internally
                        the assemblers use unsigned arithmetic


        THE ASSEMBLER                                          PAGE 1-60
        INVOKING ASXXXX


                        for calculations. (e.g. for a 2-byte machine
                        -32768 and 32768 are both represented as 0x8000)

            Symbols:
                -a      all user symbols made global

                        All defined (not local or external)
                        variables and symbols are flagged
                        as global.

                -g      undefined symbols made global

                        Unresolved (external) variables
                        and symbols are flagged as global.

                -z      disable case sensitivity for symbols

            Debugging:
                -j      enable NOICE debug symbols
                -y      enable SDCC debug symbols

           The file name for the .lst, .rel, .hlr, and .sym files is the
        first file name specified in the command line.  All output files
        are ascii text files which may be edited, copied, etc.  The out-
        put files are the concatenation of all the input files, if files
        are  to be assembled independently invoke the assembler for each
        file.

           The  .rel  file contains a radix directive so that the linker
        will use the proper conversion for this file.  Linked files  may
        have different radices.

           The  ASxxxx  assemblers  also  have  several 'hidden' options
        which are not shown in the usage message.  These are:

                -r      Include assembler line numbers
                        in the .hlr hint file
                -rr     Also include non listed line
                        numbers in the .hlr hint file

                -t      Show Include File and Macro Expansion
                        levels and memory allocations for
                        the assembler and macro processor




        THE ASSEMBLER                                          PAGE 1-61
        ERRORS


        1.6  ERRORS


           The  ASxxxx assemblers provide limited diagnostic error codes
        during the assembly process, these errors will be noted  in  the
        listing file and printed on the stderr device.

           The assembler reports the errors on the stderr device as

                ?ASxxxx-Error-<*> in line nnn of filename

        where  * is the error code, nnn is the line number, and filename
        is the source/include file.

           The errors are:

              <.>   This  error  is caused by an absolute direct assign-
                    ment of the current location counter
                          . = expression (incorrect)
                    rather than the correct
                          . = . + expression

              <a>   Indicates  a machine specific addressing or address-
                    ing mode error.

              <b>   Indicates a direct page boundary error.

              <c>   Indicates modulus of .bndry directives to large.

              <d>   Indicates a direct page addressing error.

              <e>   Caused by a .error or .assume directive.

              <i>   Caused  by  an  .include file error or an .if/.endif
                    mismatch.

              <m>   Multiple  definitions  of  the  same label, multiple
                    .module directives, multiple conflicting  attributes
                    in  an  .area or .bank directive or the use of .hilo
                    and lohi within the same assembly.

              <n>   An  .mexit,  .endm,  or .narg directive outside of a
                    macro, repeat block or indefinite repeat block.

              <o>   Directive  or  mnemonic error or the use of the .org
                    directive in a relocatable area.

              <p>   Phase error:  label location changing between passes
                    2 and 3.  Normally caused by having  more  than  one


        THE ASSEMBLER                                          PAGE 1-62
        ERRORS


                    level of forward referencing.

              <q>   Questionable syntax:  missing or improper operators,
                    terminators, or delimiters.

              <r>   Relocation  error:   logic  operation attempted on a
                    relocatable term, addition of two relocatable terms,
                    subtraction  of two relocatable terms not within the
                    same programming area or external symbols.

              <s>   String Substitution / recursion error.

              <u>   Undefined symbol encountered during assembly.

              <z>   Divide by 0 or Modulus by 0 error:  result is 0.


        1.7  LISTING FILE


           The  (-l) option produces an ascii output listing file.  Each
        page of output contains a five line header:


             1.  The ASxxxx program name and page number

             2.  Assembler Radix and Address Bits

             3.  Title from a .title directive (if any)

             4.  Subtitle from a .sbttl directive (if any)

             5.  Blank line



        Each succeeding line contains six fields:


             1.  Error field (first two characters of line)

             2.  Current location counter

             3.  Generated code in byte format

             4.  Opcode cycles count

             5.  Source text line number



        THE ASSEMBLER                                          PAGE 1-63
        LISTING FILE


             6.  Source text


           The error field may contain upto 2 error flags indicating any
        errors encountered while assembling this line of source code.

           The  current  location  counter  field  displays  the 16-bit,
        24-bit, or 32-bit program position.  This field will be  in  the
        selected radix.

           The generated code follows the program location.  The listing
        radix determines the number of bytes that will be  displayed  in
        this field.  Hexidecimal listing allows six bytes of data within
        the field, decimal and octal allow four bytes within the  field.
        If more than one field of data is generated from the assembly of
        a single line of source code, then the data field is repeated on
        successive lines.

           The  opcode cycles count is printed within the delimiters [ ]
        on the line with the source text.  This reduces  the  number  of
        generated code bytes displayed on the line with the source list-
        ing by one.  (The -c option disables all opcode cycle listing.)

           The source text line number is printed in decimal and is fol-
        lowed by the source text.  A Source line with a .page  directive
        is never listed.  (The -u option overrides this behavior.)

           Two  additional options are available for printing the source
        line text.  If the -b option is specified then the listed source
        line  contains all the .define substitutions.  If the -bb option
        is specified then the original source line is printed before the
        source line with substitutions.

           Two  data  field  options  are  available to flag those bytes
        which will be relocated by the linker.   If  the  -f  option  is
        specified  then  each  byte to be relocated will be preceeded by
        the '`' character.  If the -ff option  is  specified  then  each
        byte  to  be relocated will be preceeded by one of the following
        characters:

             1.  *   paged relocation

             2.  u   low  byte of unsigned word or unsigned byte

             3.  v   high byte of unsigned word

             4.  p   PCR low  byte of word relocation or PCR byte



        THE ASSEMBLER                                          PAGE 1-64
        LISTING FILE


             5.  q   PCR high byte of word relocation

             6.  r   low  byte relocation or byte relocation

             7.  s   high byte relocation


           Assemblers  which  use 24-bit or 32-bit addressing use an ex-
        tended flagging mode:

             1.  *   paged relocation

             2.  u   1st  byte of unsigned value

             3.  v   2nd  byte of unsigned value

             4.  U   3rd  byte of unsigned value

             5.  V   4th  byte of unsigned value

             6.  p   PCR 1st  byte of relocation value or PCR byte

             7.  q   PCR 2nd  byte of relocation value

             8.  P   PCR 3rd  byte of relocation value

             9.  Q   PCR 4th  byte of relocation value

            10.  r   1st  byte of relocation value or byte relocation

            11.  s   2nd  byte of relocation value

            12.  R   3rd  byte of relocation value

            13.  S   4th  byte of relocation value





        THE ASSEMBLER                                          PAGE 1-65
        SYMBOL TABLE FILE


        1.8  SYMBOL TABLE FILE


           The symbol table has two parts:

             1.  The alphabetically sorted list of symbols and/or labels
                 defined or referenced in the source program.

             2.  A  list of the program areas defined during assembly of
                 the source program.


           The sorted list of symbols and/or labels contains the follow-
        ing information:

             1.  Program  area  number (none if absolute value or exter-
                 nal)

             2.  The symbol or label

             3.  Directly assigned symbol is denoted with an (=) sign

             4.  The  value of a symbol, location of a label relative to
                 the program area base address (=0), or a ****  indicat-
                 ing the symbol or label is undefined.

             5.  The       characters:       G - global,      L - local,
                 R - relocatable, and X - external.


           The list of program areas provides the correspondence between
        the program area numbers and the defined program areas, the size
        of the program areas, and the area flags (attributes).


        1.9  OBJECT FILE


           The  object  file is an ascii file containing the information
        needed by the linker to bind multiple object modules into a com-
        plete  loadable  memory  image.   The object module contains the
        following designators:

                [XDQ][HL][234]
                        X       Hexidecimal radix
                        D       Decimal radix
                        Q       Octal radix

                        H       Most significant byte first


        THE ASSEMBLER                                          PAGE 1-66
        OBJECT FILE


                        L       Least significant byte first

                        2       16-Bit Addressing
                        3       24-Bit Addressing
                        4       32-Bit Addressing

                H       Header
                M       Module
                G       Merge Mode
                B       Bank
                A       Area
                S       Symbol
                T       Object code
                R       Relocation information
                P       Paging information

           Refer to the linker for a detailed description of each of the
        designators and the format of the information contained  in  the
        object file.


        1.10  HINT FILE


           The  hint file is an ascii file containing information needed
        by the linker to convert the listing file into a relocated list-
        ing  file.   Each  line  in the .hlr file coresponds to a single
        line in the listing file.  The text line usually contains 3 or 4
        parameters  in  the radix selected for the assembler as shown in
        the following table:

        Line Position:  123456789012
                        ------------
        Octal:           111 222 333
        Decimal:         111 222 333
        Hex:             11 22 33


        Parameter 1 specifies the parameters listed in the line.
        A bit is set for each listing option enabled during the
        assembly of the line.

                BIT 0   - LIST_ERR      Error Code(s)
                BIT 1   - LIST_LOC      Location
                BIT 2   - LIST_BIN      Generated Binary Value(s)
                BIT 3   - LIST_EQT      Assembler Equate Value
                BIT 4   - LIST_CYC      Opcode Cycles
                BIT 5   - LIST_LIN      Line Numbers
                BIT 6   - LIST_SRC      Assembler Source Code


        THE ASSEMBLER                                          PAGE 1-67
        HINT FILE


                BIT 7   - HLR_NLST      Listing Inhibited

        Parameter 2 is the internal assembler listing mode
        value specified for this line during the assembly process:

                0 - NLIST       No listing
                1 - SLIST       Source only
                2 - ALIST       Address only
                3 - BLIST       Address only with allocation
                4 - CLIST       Code
                5 - ELIST       Equate only
                6 - ILIST       IF conditional evaluation

        Parameter 3 is the number of output bytes listed
        for this line.

           The  4th  parameter  is only output if an equate references a
        value in a different area.  The area name is output in the  fol-
        lowing format following the 3 parameters described above:

        Line Position:  123456789012
                        ------------
        Area Name:       equatearea

           When  the  line number is output to the .hlr file (-r option)
        the line number is prepended to the 3 or 4 parameters  described
        above.   The  line  number is always in decimal in the following
        format:

        Line Position:  1234567
                        -------
        Decimal:          LLLLL

           Thus the four formats (for each radix) that may be present in
        a .hlr file are:

        Line Position:  123456789012345678901234567890
                        ------------------------------
                         11 22 33
                         11 22 33 equatearea
                          LLLLL 11 22 33
                          LLLLL 11 22 33 equatearea

           The  linker understands these formats without any user inter-
        action.
