# CHAPTER 1 THE ASSEMBLER | 第一章 汇编器
## <a id="1.1"></a>1.1  THE ASXXXX ASSEMBLERS | ASXXXX汇编器

The ASxxxx assemblers are a series of microprocessor assemblers written in the C programming language. Each assembler has a device specific section which includes:

ASxxxx汇编器（集合）是一系列用C语言编写的微处理器汇编器。每种汇编器都有一个特定于设备的节，其中包含了：

1. device description, byte order, and file extension information 

    设备描述、字节顺序和文件扩展名信息

2. a table of the assembler general directives, special device directives, assembler mnemonics and associated operation codes 

    由汇编通用指示符、特殊设备指示符、汇编助记符和相关操作码构成的表格

3. machine specific code for processing the device mnemonics, addressing modes, and special directives 

    特定于机器的代码，用于处理设备助记符、地址模式和特殊指示符

The device specific information is detailed in the appendices.

特定于设备的详细信息请参见附录（译注：未翻译）。

The assemblers have a common device independent section which handles the details of file input/output, symbol table generation, program/data areas, expression analysis, and assembler directive processing.

这些汇编器有一个公用的设备无关节，用于处理与文件输入/输出、符号表生成、程序数据分区、表达式分析、以及汇编指示符处理相关的细节。

The assemblers provide the following features:

这些汇编器提供了下列特性：

1. Command string control of assembly functions 

    汇编功能的命令字符串控制

2. Alphabetized, formatted symbol table listing 

    按字母排序的、格式化过的符号表列表

3. Relocatable object modules 

    可重定位的目标模块

4. Global symbols for linking object modules 

    用于连接目标模块的全局符号

5. Conditional assembly directives 

    条件汇编指示符

6. Program sectioning directives 

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

 The relocatable object file is an ascii file containing sym- bol references and definitions, program area definitions, and the relocatable assembled code, the linker ASLINK will use this information to generate an absolute load file (Intel, Motorola or Tandy CoCo Disk Basic formats).

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

#### <a id="1.2.1.1"></a>1.2.1.1  Label Field | 标号字段

A label is a user-defined symbol which is assigned the value of the current location counter and entered into the user de- fined symbol table. The current location counter is used by ASxxxx to assign memory addresses to the source program state- ments as they are encountered during the assembly process. Thus a label is a means of symbolically referring to a specific statement. 

标号是一个用户定义的符号，具有当前位置计数器的值，并会被放置到用户定义符号表中。当ASxxxx在汇编过程中遇到原程序语句，它会使用当前位置计数器为该语句赋予内存地址。所以标号的意思就是对一条特定语句的符号化引用。

           When  a program section is absolute, the value of the current
        location counter is absolute;  its value references an  absolute
        memory  address.   Similarly, when a program section is relocat-
        able, the value of the current location counter is  relocatable.
        A  relocation  bias  calculated at link time is added to the ap-
        parent value of the current location counter  to  establish  its
        effective  absolute  address  at  execution time.  (The user can
        also force the linker to relocate sections defined as  absolute.
        This may be required under special circumstances.) 

           If  present,  a  label  must  be  the first field in a source
        statement and must be terminated by a colon (:).   For  example,
        if  the  value  of  the  current  location  counter  is absolute
        01F0(H), the statement:  

              abcd:     nop 

        assigns  the  value  01F0(H) to the label abcd.  If the location
        counter value were relocatable, the final value of abcd would be
        01F0(H)+K, where K represents the relocation bias of the program
        section, as calculated by the linker at link time.  

           More  than  one label may appear within a single label field.
        Each label so specified is assigned the same address value.  For
        example,  if  the  value  of  the  current  location  counter is
        1FF0(H), the multiple labels in the following statement are each
        assigned the value 1FF0(H):  

              abcd:     aq:     $abc:   nop 

           Multiple labels may also appear on successive lines.  For ex-
        ample, the statements 

              abcd:  
              aq:  
              $abc:     nop 

        likewise  cause  the  same value to be assigned to all three la-
        bels.  


        THE ASSEMBLER                                           PAGE 1-5
        SOURCE PROGRAM FORMAT


           A  double  colon  (::)  defines the label as a global symbol.
        For example, the statement 

              abcd::    nop 

        establishes the label abcd as a global symbol.  The distinguish-
        ing attribute of a global symbol is that it  can  be  referenced
        from  within an object module other than the module in which the
        symbol is defined.  References to this label  in  other  modules
        are  resolved when the modules are linked as a composite execut-
        able image.  

        The legal characters for defining labels are:  

                A through Z 
                a through z 
                0 through 9 
                . (Period) 
                $ (Dollar sign) 
                _ (underscore) 

           A  label  may  be  any  length,  however  only  the  first 79
        characters are significant and, therefore must be  unique  among
        all  labels in the source program (not necessarily among separa-
        tely compiled modules).  An error code(s) (<m> or <p>)  will  be
        generated  in the assembly listing if the first 79 characters in
        two or more labels are the same.  The <m> code is caused by  the
        redeclaration  of  the symbol or its reference by another state-
        ment.  The <p> code is generated because the symbols location is
        changing on each pass through the source file.  

           The  label  must  not  start with the characters 0-9, as this
        designates a reusable symbol with special  attributes  described
        in a later section.  

           The  label  must  not  start  with  the  sequence $$, as this
        represents the temporary radix 16 for constants.  




        THE ASSEMBLER                                           PAGE 1-6
        SOURCE PROGRAM FORMAT


        1.2.1.2  Operator Field  - 

           The  operator field specifies the action to be performed.  It
        may consist of an instruction mnemonic (op code) or an assembler
        directive.  

           When  the  operator is an instruction mnemonic, a machine in-
        struction is generated and the assembler evaluates the addresses
        of  the operands which follow.  When the operator is a directive
        ASxxxx performs certain control actions or processing operations
        during assembly of the source program.  

           Leading  and  trailing  spaces  or tabs in the operator field
        have no significance;  such characters serve  only  to  separate
        the operator field from the preceeding and following fields.  

           An operator is terminated by a space, tab or end of line.  


        1.2.1.3  Operand Field  - 

           When  the  operator is an instruction mnemonic (op code), the
        operand  field  contains  program  variables  that  are  to   be
        evaluated/manipulated by the operator.  

           Operands  may  be  expressions  or  symbols, depending on the
        operator.  Multiple expressions used in the operand  fields  may
        be  separated  by a comma.  An operand should be preceeded by an
        operator field;  if it is not, the statement will give an  error
        (<q> or <o>).   All operands following instruction mnemonics are
        treated as expressions.  

           The operand field is terminated by a semicolon when the field
        is followed  by  a  comment.   For  example,  in  the  following
        statement:  

              label:    lda     abcd,x          ;Comment field 

        the  tab  between lda and abcd terminates the operator field and
        defines the beginning of the operand field;  a  comma  separates
        the operands abcd and x;  and a semicolon terminates the operand
        field and defines the beginning of the comment field.   When  no
        comment  field  follows,  the operand field is terminated by the
        end of the source line.  




        THE ASSEMBLER                                           PAGE 1-7
        SOURCE PROGRAM FORMAT


        1.2.1.4  Comment Field  - 

           The comment field begins with a semicolon and extends through
        the end of the line.  This field is optional and may contain any
        7-bit ascii character except null.  

           Comments  do not affect assembly processing or program execu-
        tion.  


        1.3  SYMBOLS AND EXPRESSIONS 


           This  section  describes the generic components of the ASxxxx
        assemblers:  the character set, the conventions observed in con-
        structing  symbols,  and  the use of numbers, operators, and ex-
        pressions.  


        1.3.1  Character Set 


           The following characters are legal in ASxxxx source programs: 

             1.  The  letters  A  through Z.  Both upper- and lower-case
                 letters are acceptable.  The  assemblers,  by  default,
                 are  case  sensitive,  i.e.   ABCD and abcd are not the
                 same symbols.  (The assemblers can be made case  insen-
                 sitive by using the -z command line option.) 

             2.  The digits 0 through 9 

             3.  The  characters . (period), $ (dollar sign), and _ (un-
                 derscore).  

             4.  The special characters listed in Tables 1 through 6.  


           Tables  1  through  6  describe  the various ASxxxx label and
        field terminators, assignment operators, operand separators, as-
        sembly, unary, binary, and radix operators.  


        THE ASSEMBLER                                           PAGE 1-8
        SYMBOLS AND EXPRESSIONS


        Table 1         Label Terminators and Assignment Operators 
        ---------------------------------------------------------------- 

                :   Colon               Label terminator.  

                ::  Double colon        Label  Terminator;   defines the
                                        label as a global label.  

                =   Equal sign          Direct assignment operator.  

                ==  Global equal        Direct assignment operator;  de-
                                        fines the  symbol  as  a  global
                                        symbol.  

                =:  Local equal         Direct assignment operator;  de-
                                        fines the symbol as a local sym-
                                        bol.  

        ---------------------------------------------------------------- 





        Table 2         Field Terminators and Operand Separators 
        ---------------------------------------------------------------- 

                    Tab                 Item or field terminator.  

                    Space               Item or field terminator.  

                ,   Comma               Operand field separator.  

                ;   Semicolon           Comment field indicator.  

        ---------------------------------------------------------------- 







        THE ASSEMBLER                                           PAGE 1-9
        SYMBOLS AND EXPRESSIONS


        Table 3         Assembler Operators 
        ---------------------------------------------------------------- 

                #   Number sign         Immediate expression indicator. 

                .   Period              Current location counter.  

                (   Left parenthesis    Expression delimiter.  

                )   Right parenthesis   Expression delimeter.  

        ---------------------------------------------------------------- 





        Table 4         Unary Operators 
        ---------------------------------------------------------------- 

                <   Left bracket        <FEDC   Produces  the lower byte
                                                value of the expression.
                                                (DC) 

                >   Right bracket       >FEDC   Produces  the upper byte
                                                value of the expression.
                                                (FE) 

                +   Plus sign           +A      Positive value of A 

                -   Minus sign          -A      Produces   the  negative
                                                (2's complement) of A.  

                ~   Tilde               ~A      Produces the 1's comple-
                                                ment of A.  

                '   Single quote        'D      Produces  the  value  of
                                                the character D.  

                "   Double quote        "AB     Produces the double byte
                                                value for AB.  

                \   Backslash           '\n     Unix style characters 
                                                \b, \f, \n, \r, \t 
                                     or '\001   or octal byte values.  

        ---------------------------------------------------------------- 


        THE ASSEMBLER                                          PAGE 1-10
        SYMBOLS AND EXPRESSIONS







        Table 5         Binary Operators 
        ---------------------------------------------------------------- 

                <<  Double          0800 << 4   Produces the 4 bit 
                    Left bracket                left-shifted   value  of
                                                0800.  (8000) 

                >>  Double          0800 >> 4   Produces the 4 bit 
                    Right bracket               right-shifted  value  of
                                                0800.  (0080) 

                +   Plus sign       A + B       Arithmetic      Addition
                                                operator.  

                -   Minus sign      A - B       Arithmetic   Subtraction
                                                operator.  

                *   Asterisk        A * B       Arithmetic   Multiplica-
                                                tion operator.  

                /   Slash           A / B       Arithmetic      Division
                                                operator.  

                &   Ampersand       A & B       Logical AND operator.  

                |   Bar             A | B       Logical OR operator.  

                %   Percent sign    A % B       Modulus operator.  

                ^   Up arrow or     A ^ B       EXCLUSIVE OR operator.  
                    circumflex 

        ---------------------------------------------------------------- 







        THE ASSEMBLER                                          PAGE 1-11
        SYMBOLS AND EXPRESSIONS


        Table 6         Temporary Radix Operators 
        ---------------------------------------------------------------- 

                $%,   0b, 0B            Binary radix operator.  

                $&,   0o, 0O, 0q, 0Q    Octal radix operator.  

                $#,   0d, 0D            Decimal radix operator.  

                $$,   0h, 0H, 0x, 0X    Hexidecimal radix operator.  


                Potential  ambiguities arising from the use of 0b and 0d
                as temporary radix operators may be circumvented by pre-
                ceding  all  non-prefixed  hexidecimal  numbers with 00.
                Leading 0's are required in any  case  where  the  first
                hexidecimal  digit is abcdef as the assembler will treat
                the letter sequence as a label.  

        ---------------------------------------------------------------- 







        1.3.2  User-Defined Symbols 


           User-defined  symbols are those symbols that are equated to a
        specific value through a direct assignment statement  or  appear
        as  labels.  These symbols are added to the User Symbol Table as
        they are encountered during assembly.  

        The following rules govern the creation of user-defined symbols: 

             1.  Symbols  can  be  composed  of alphanumeric characters,
                 dollar signs ($),  periods  (.),  and  underscores  (_)
                 only.  

             2.  The  first  character  of a symbol must not be a number
                 (except in the case of reusable symbols).  

             3.  The  first 79 characters of a symbol must be unique.  A
                 symbol  can  be  written  with  more  than   79   legal
                 characters,  but the 80th and subsequent characters are
                 ignored.  



        THE ASSEMBLER                                          PAGE 1-12
        SYMBOLS AND EXPRESSIONS


             4.  Spaces and Tabs must not be embedded within a symbol.  



        1.3.3  Reusable Symbols 


           Reusable  symbols are specially formatted symbols used as la-
        bels within a block of coding that has been delimited as a reus-
        able symbol block.  Reusable symbols are of the form n$, where n
        is a decimal integer from 0 to 65535,  inclusive.   Examples  of
        reusable symbols are:  

              1$ 
              27$ 
              138$ 
              244$ 

           The range of a reusable symbol block consists of those state-
        ments between two normally constructed  symbolic  labels.   Note
        that a statement of the form:  

              ALPHA = EXPRESSION 

        is a direct assignment statement but does not create a label and
        thus does not delimit the range of a reusable symbol block.  

           Note  that  the  range  of a reusable symbol block may extend
        across program areas.  

           Reusable symbols provide a convenient means of generating la-
        bels for branch instructions and other  such  references  within
        reusable symbol blocks.  Using reusable symbols reduces the pos-
        sibility of symbols with multiple definitions appearing within a
        user  program.   In  addition,  the use of reusable symbols dif-
        ferentiates entry-point labels from other labels, since reusable
        labels cannot be referenced from outside their respective symbol
        blocks.  Thus, reusable symbols of the same name can  appear  in
        other  symbol blocks without conflict.  Reusable symbols require
        less symbol table space  than  normal  symbols.   Their  use  is
        recommended.  

           The  use  of  the  same reusable symbol within a symbol block
        will generate one or both of the <m> or <p> errors.  


        THE ASSEMBLER                                          PAGE 1-13
        SYMBOLS AND EXPRESSIONS


        Example of reusable symbols:  

                a:      ldx     #atable ;get table address
                        lda     #0d48   ;table length
                1$:     clr     ,x+     ;clear
                        deca
                        bne     1$
                        
                b:      ldx     #btable ;get table address
                        lda     #0d48   ;table length
                1$:     clr     ,x+     ;clear
                        deca
                        bne     1$


        1.3.4  Current Location Counter 


           The  period  (.) is the symbol for the current location coun-
        ter.  When used in the operand  field  of  an  instruction,  the
        period   represents  the  address  of  the  first  byte  of  the
        instruction:  

                AS:     ldx     #.      ;The period (.) refers to
                                        ;the address of the ldx
                                        ;instruction.

           When  used  in  the  operand field of an ASxxxx directive, it
        represents the address of the current byte or word:  

                QK = 0
        
                .word   0xFFFE,.+4,QK   ;The operand .+4 in the .word
                                        ;directive represents a value
                                        ;stored in the second of the
                                        ;three words during assembly.

           If  we  assume  the  current  value of the program counter is
        0H0200, then during assembly, ASxxxx  reserves  three  words  of
        storage  starting  at  location 0H0200.  The first value, a hex-
        idecimal constant FFFE, will be stored at location 0H0200.   The
        second  value  represented  by  .+4  will  be stored at location
        0H0202, its value will be 0H0206 ( = 0H0202  +  4).   The  third
        value  defined  by  the  symbol  QK  will  be placed at location
        0H0204.  

           At the beginning of each assembly pass, ASxxxx resets the lo-
        cation counter.   Normally,  consecutive  memory  locations  are
        assigned  to  each  byte of object code generated.  However, the


        THE ASSEMBLER                                          PAGE 1-14
        SYMBOLS AND EXPRESSIONS


        value of the location counter can be changed  through  a  direct
        assignment statement of the following form:  

              . = . + expression 


           The  new  location  counter can only be specified relative to
        the current location counter.  Neglecting to specify the current
        program  counter  along with the expression on the right side of
        the assignment operator will generate the <.> error.   (Absolute
        program areas may use the .org directive to specify the absolute
        location of the current program counter.) 

        The following coding illustrates the use of the current location
        counter:  

                .area   CODE1   (ABS)   ;program area CODE1
                                        ;is ABSOLUTE
        
                .org    0H100           ;set location to
                                        ;0H100 absolute
        
        num1:   ldx     #.+0H10         ;The label num1 has
                                        ;the value 0H100.
                                        ;X is loaded with
                                        ;0H100 + 0H10
        
                .org    0H130           ;location counter
                                        ;set to 0H130
        
        num2:   ldy     #.              ;The label num2 has
                                        ;the value 0H130.
                                        ;Y is loaded with
                                        ;value 0H130.
        
        
                .area   CODE2   (REL)   ;program area CODE2
                                        ;is RELOCATABLE
        
                . = . + 0H20            ;Set location counter
                                        ;to relocatable 0H20 of
                                        ;the program section.
        
        num3:   .word   0               ;The label num3 has
                                        ;the value
                                        ;of relocatable 0H20.
        
                . = . + 0H40            ;will reserve 0H40
                                        ;bytes of storage as will


        THE ASSEMBLER                                          PAGE 1-15
        SYMBOLS AND EXPRESSIONS


                .blkb   0H40            ;or
                .blkw   0H20

           The  .blkb  and .blkw directives are the preferred methods of
        allocating space.  


        1.3.5  Numbers 


           ASxxxx  assumes that all numbers in the source program are to
        be interpreted in decimal radix unless otherwise specified.  The
        .radix  directive  may  be used to specify the default as octal,
        decimal, or hexidecimal.  Individual numbers can  be  designated
        as  binary, octal, decimal, or hexidecimal through the temporary
        radix prefixes shown in table 6.  

           Negative  numbers  must be preceeded by a minus sign;  ASxxxx
        translates such numbers into two's  complement  form.   Positive
        numbers may (but need not) be preceeded by a plus sign.  

           Numbers are always considered to be absolute values, therefor
        they are never relocatable.  


        1.3.6  Terms 


           A  term is a component of an expression and may be one of the
        following:  


             1.  A number.  

             2.  A symbol:  
                 1.  A  period (.) specified in an expression causes the
                     current location counter to be used.  
                 2.  A User-defined symbol.  
                 3.  An undefined symbol is assigned a value of zero and
                     inserted in the User-Defined symbol table as an un-
                     defined symbol.  

             3.  A single quote followed by a single ascii character, or
                 a double quote followed by two ascii characters.  

             4.  An  expression enclosed in parenthesis.  Any expression
                 so enclosed is evaluated and reduced to a  single  term
                 before  the  remainder  of  the  expression in which it
                 appears is evaluated.  Parenthesis, for example, may be


        THE ASSEMBLER                                          PAGE 1-16
        SYMBOLS AND EXPRESSIONS


                 used  to  alter the left-to-right evaluation of expres-
                 sions, (as in A*B+C versus A*(B+C)), or to apply a  un-
                 ary operator to an entire expression (as in -(A+B)).  

             5.  A unary operator followed by a symbol or number.  



        1.3.7  Expressions 


           Expressions  are  combinations  of  terms  joined together by
        binary operators.  Expressions reduce to a value.   The  evalua-
        tion  of  an expression includes the determination of its attri-
        butes.  A resultant expression value may be one of  three  types
        (as  described  later  in this section):  relocatable, absolute,
        and external.  

        Expressions are evaluate with an operand hierarchy as follows:  

                *       /       %       multiplication,
                                        division, and
                                        modulus first.
        
                +       -               addition and
                                        subtraction second.
        
                <<      >>              left shift and
                                        right shift third.
        
                ^                       exclusive or fourth.
        
                &                       logical and fifth.
        
                |                       logical or last
        
                except that unary operators take precedence over binary
                operators.


           A  missing  or  illegal  operator  terminates  the expression
        analysis, causing error codes <o> and/or  <q>  to  be  generated
        depending upon the context of the expression itself.  

           At assembly time the value of an external (global) expression
        is equal to the value of the absolute part of  that  expression.
        For  example,  the expression external+4, where 'external' is an
        external symbol, has the value of 4.  This expression,  however,


        THE ASSEMBLER                                          PAGE 1-17
        SYMBOLS AND EXPRESSIONS


        when  evaluated  at link time takes on the resolved value of the
        symbol 'external', plus 4.  

           Expressions,  when  evaluated  by  ASxxxx,  are  one of three
        types:  relocatable, absolute, or external.  The following  dis-
        tinctions are important:  

             1.  An  expression is relocatable if its value is fixed re-
                 lative to the base address of the program area in which
                 it appears;  it will have an offset value added at link
                 time.  Terms that contain labels defined in relocatable
                 program  areas  will  have  a relocatable value;  simi-
                 larly, a period (.)  in  a  relocatable  program  area,
                 representing  the value of the current program location
                 counter, will also have a relocatable value.  

             2.  An  expression  is  absolute if its value is fixed.  An
                 expression whose terms are numbers and ascii characters
                 will  reduce  to  an absolute value.  A relocatable ex-
                 pression or term minus a relocatable term,  where  both
                 elements  being  evaluated  belong  to the same program
                 area, is an absolute expression.  This is because every
                 term  in  a  program area has the same relocation bias.
                 When one term is subtracted from the other the  reloca-
                 tion bias is zero.  

             3.  An  expression is external (or global) if it contains a
                 single global reference (plus or minus an absolute  ex-
                 pression  value) that is not defined within the current
                 program.  Thus, an external  expression  is  only  par-
                 tially  defined following assembly and must be resolved
                 at link time.  



        1.4  GENERAL ASSEMBLER DIRECTIVES 


           An  ASxxxx  directive  is placed in the operator field of the
        source line.  Only one directive is  allowed  per  source  line.
        Each  directive  may  have  a blank operand field or one or more
        operands.  Legal operands differ with each directive.  




        THE ASSEMBLER                                          PAGE 1-18
        GENERAL ASSEMBLER DIRECTIVES


        1.4.1  .module Directive 

        Format:  

                .module name 

           The  .module  directive causes the name to be included in the
        assemblers output file as an identifier for this particular  ob-
        ject module.  The name may be from 1 to 79 characters in length.
        The name may not have any embedded white space (spaces or tabs).
        Only  one  identifier is allowed per assembled module.  The main
        use of this directive  is  to  allow  the  linker  to  report  a
        modules'  use  of undefined symbols.  At link time all undefined
        symbols are  reported  and  the  modules  referencing  them  are
        listed.  


        1.4.2  .title Directive 

        Format:  

                .title  string 

           The .title directive provides a character string to be placed
        on the second line of each page during listing.  The string  be-
        gins  with  the first non white space character (after any space
        or tab) and ends with the end of the line.  


        1.4.3  .sbttl Directive 

        Format:  

                .sbttl  string 

           The .sbttl directive provides a character string to be placed
        on the third line of each page during listing.  The  string  be-
        gins  with  the first non white space character (after any space
        or tab) and ends with the end of the line.  




        THE ASSEMBLER                                          PAGE 1-19
        GENERAL ASSEMBLER DIRECTIVES


        1.4.4  .list and .nlist Directives 

        Format:  

                .list                           ;Basic .list 

                .list   expr                    ;with expression 

                .list   (arg1,arg2,...,argn)    ;with sublist options 

                .nlist                          ;Basic .nlist 

                .nlist  expr                    ;with expression 

                .nlist  (arg1,arg2,...,argn)    ;with sublist options 


        The  .list  and  .nlist directives control the listing output to
        the .lst  file.   The  directives  have  the  following  sublist
        options:  

                err     -       errors
                loc     -       program location
                bin     -       binary output
                eqt     -       symbol or .if evaluation
                cyc     -       opcode cycle count
                lin     -       source line number
                src     -       source line text
                pag     -       pagination
                lst     -       .list/.nlist line listing
                md      -       macro definition listing
                me      -       macro expansion listing
                meb     -       macro expansion binary listing
        
                !       -       sets the listing mode to
                                !(.list) or !(.nlist) before
                                applying the sublist options


        The  'normal' listing mode .list is the combination of err, loc,
        bin, eqt, cyc, lin, src, pag, lst, and md enabled  with  me  and
        meb  disabled.  The 'normal' listing mode .nlist has all sublist
        items disabled.  When specifying sublist options the option list
        must   be  enclosed  within  parenthesis  and  multiple  options
        seperated by commas.  

           The NOT option, !, is used to set the listing mode to the op-
        posite of the .list or  .nlist  directive  before  applying  the
        sublist options.  For example:  


        THE ASSEMBLER                                          PAGE 1-20
        GENERAL ASSEMBLER DIRECTIVES



                .nlist  (!)     is equivalent to .list  and
                .list   (!)     is equivalent to .nlist
                                any additional options will
                                be applied normally


           Normal  .list/.nlist processing is disabled within false con-
        ditional blocks.  However, the .list/.nlist with  an  expression
        can  override  this  behavior  if  the expression has a non zero
        value.  

           Examples of listing options:  

                .list   (meb)   ; macro processing lists only
                                ; generated binary and location
        
                .list   (me)    ; listing options are enabled
                                ; during macro processing
        
                .nlist  (src)   ; .nlist src lines not listed
        
                .nlist  (!,lst) ; list all except .nlist
        
                .nlist          ; combination lists only
                .list   (src)   ; the source line
        
                .list   (!,src) ; list only the source line
        
                .list   1       ; enable listing even within
                                ; a FALSE conditional block


        1.4.5  .page Directive 

        Format:  

                .page 

           The .page directive causes a page ejection with a new heading
        to be printed.  The new page occurs after the next line  of  the
        source  program is processed, this allows an immediately follow-
        ing .sbttl directive to appear  on  the  new  page.   The  .page
        source  line will not appear in the file listing.  Paging may be
        disabled by invoking the -p directive or by using the directive: 

                .nlist  (pag)




        THE ASSEMBLER                                          PAGE 1-21
        GENERAL ASSEMBLER DIRECTIVES


           If  the .page directive is followed by a non zero constant or
        an expression that evaluates to a non zero value then pagination
        will be enabled within a false condition range to allow extended
        textual information to be incorporated  in  the  source  program
        with out the need to use the comment delimiter (;):  

                .if     0
        
                .page   1       ;Enable pagination within 'if' block.
                This text will be bypassed during assembly
                but appear in the listing file.
                .
                .
                .
        
                .endif


        1.4.6  .msg Directive 

        Format:  

                .msg     /string/               or 

                .msg    ^/string/ 


        where:  string  represents a text string.  The string is printed
                        to the console during the final assembly pass.  

                /  /    represent   the  delimiting  characters.   These
                        delimiters   may   be   any   paired    printing
                        characters,  as  long  as the characters are not
                        contained within  the  string  itself.   If  the
                        delimiting  characters  do  not  match, the .msg
                        directive will give the <q> error.  


           The  .msg  directive  is  useful to report assembly status or
        other information during the assembly process.  




        THE ASSEMBLER                                          PAGE 1-22
        GENERAL ASSEMBLER DIRECTIVES


        1.4.7  .error Directive 

        Format:  

                .error exp 

        where:  exp     represents   an  absolute  expression.   If  the
                        evaluation of the expression results  in  a  non
                        zero value then an <e> error is reported and the
                        text line is listed in the generated error.  


           The  .error  directive  is  useful to report configuration or
        value errors during the assembly process.  (The .error directive
        is  identical in function to the .assume directive, just perhaps
        more descriptive.) 


        1.4.8  .byte, .db, and .fcb Directives 

        Format:  

                .byte   exp             ;Stores the binary value
                .db     exp             ;of the expression in the
                .fcb    exp             ;next byte.
        
                .byte   exp1,exp2,expn  ;Stores the binary values
                .db     exp1,exp2,expn  ;of the list of expressions
                .fcb    exp1,exp2,expn  ;in successive bytes.
        
        where:  exp,    represent expressions that will be
                exp1,   truncated to 8-bits of data.
                .       Each expression will be calculated,
                .       the high-order byte will be truncated.
                .       Multiple expressions must be
                expn    separated by commas.

           The  .byte, .db, or .fcb directives are used to generate suc-
        cessive bytes of binary data in the object module.  




        THE ASSEMBLER                                          PAGE 1-23
        GENERAL ASSEMBLER DIRECTIVES


        1.4.9  .word, .dw, and .fdb Directives 

        Format:  

                .word   exp             ;Stores the binary value
                .dw     exp             ;of the expression in
                .fdb    exp             ;the next word.
        
                .word   exp1,exp2,expn  ;Stores the binary values
                .dw     exp1,exp2,expn  ;of the list of expressions
                .fdb    exp1,exp2,expn  ;in successive words.
        
        where:  exp,    represent expressions that will occupy two
                exp1,   bytes of data. Each expression will be
                .       calculated as a 16-bit word expression.
                .       Multiple expressions must be
                expn    separated by commas.

           The  .word, .dw, or .fdb directives are used to generate suc-
        cessive words of binary data in the object module.  


        1.4.10  .3byte and .triple Directives 

        Format:  

                .3byte  exp             ;Stores the binary value
                .triple exp             ;of the expression in
                                        ;the next triple (3 bytes).
        
                .3byte  exp1,exp2,expn  ;Stores the binary values
                .triple exp1,exp2,expn  ;of the list of expressions
                                        ;in successive triples
                                        ;(3 bytes).
        
        where:  exp,    represent expressions that will occupy three
                exp1,   bytes of data. Each expression will be
                .       calculated as a 24-bit word expression.
                .       Multiple expressions must be
                expn    separated by commas.

           The  .3byte  or .triple directive is used to generate succes-
        sive triples of binary data in the object module.  (These direc-
        tives   are  only  available  in  assemblers  supporting  24-bit
        addressing.) 




        THE ASSEMBLER                                          PAGE 1-24
        GENERAL ASSEMBLER DIRECTIVES


        1.4.11  .4byte and .quad Directive 

        Format:  

                .4byte  exp             ;Stores the binary value
                .quad   exp             ;of the expression in
                                        ;the next quad (4 bytes).
        
                .4byte  exp1,exp2,expn  ;Stores the binary values
                .quad   exp1,exp2,expn  ;of the list of expressions
                                        ;in successive quads
                                        ;(4 bytes).
        
        where:  exp,    represent expressions that will occupy three
                exp1,   bytes of data. Each expression will be
                .       calculated as a 32-bit word expression.
                .       Multiple expressions must be
                expn    separated by commas.

           The  .4byte or .quad directive is used to generate successive
        quads of binary data in the object  module.   (These  directives
        are only available in assemblers supporting 32-bit addressing.) 


        1.4.12  .blkb, .ds, .rmb, and .rs Directives 

        Format:  

                .blkb   N       ;reserve N bytes of space
                .ds     N       ;reserve N bytes of space
                .rmb    N       ;reserve N bytes of space
                .rs     N       ;reserve N bytes of space

           The  .blkb, .ds, .rmb, and .rs directives reserve byte blocks
        in the object module;  


        1.4.13  .blkw, .blk3, and .blk4 Directives 

        Format:  

                .blkw   N       ;reserve N words of space
                .blk3   N       ;reserve N triples of space
                .blk4   N       ;reserve N quads of space

           The .blkw directive reserves word blocks;  the .blk3 reserves
        3  byte  blocks(available  in   assemblers   supporting   24-bit
        addressing);   the  .blk4  reserves  4 byte blocks (available in
        assemblers supporting 32-bit addressing).  


        THE ASSEMBLER                                          PAGE 1-25
        GENERAL ASSEMBLER DIRECTIVES


        1.4.14  .ascii, .str, and .fcc Directives 

        Format:  

                .ascii   /string/               or 

                .ascii  ^/string/ 

                .fcc     /string/               or 

                .fcc    ^/string/ 

                .str     /string/               or 

                .str    ^/string/ 


        where:  string  is a string of printable ascii characters.  

                /  /    represent   the  delimiting  characters.   These
                        delimiters   may   be   any   paired    printing
                        characters,  as  long  as the characters are not
                        contained within  the  string  itself.   If  the
                        delimiting  characters  do not match, the .ascii
                        directive will give the <q> error.  

        The  .ascii,  .fcc, and .str directives place one binary byte of
        data for each character in the string into the object module.  


        1.4.15  .ascis and .strs Directives 

        Format:  

                .ascis   /string/               or 

                .ascis  ^/string/ 

                .strs    /string/               or 

                .strs   ^/string/ 


        where:  string  is a string of printable ascii characters.  

                /  /    represent   the  delimiting  characters.   These
                        delimiters   may   be   any   paired    printing
                        characters,  as  long  as the characters are not
                        contained within  the  string  itself.   If  the


        THE ASSEMBLER                                          PAGE 1-26
        GENERAL ASSEMBLER DIRECTIVES


                        delimiting  characters  do not match, the .ascis
                        and .strs directives will give the <q> error.  


           The .ascis and .strs directives place one binary byte of data
        for each character in the string into the  object  module.   The
        last character in the string will have the high order bit set.  


        1.4.16  .asciz and .strz Directives 

        Format:  

                .asciz   /string/               or 

                .asciz  ^/string/ 

                .strz    /string/               or 

                .strz   ^/string/ 


        where:  string  is a string of printable ascii characters.  

                /  /    represent   the  delimiting  characters.   These
                        delimiters   may   be   any   paired    printing
                        characters,  as  long  as the characters are not
                        contained within  the  string  itself.   If  the
                        delimiting  characters  do not match, the .asciz
                        and .strz directive will give the <q> error.  


           The .asciz and .strz directives place one binary byte of data
        for each character in the string into the object  module.   Fol-
        lowing  all  the  character data a zero byte is inserted to ter-
        minate the character string.  




        THE ASSEMBLER                                          PAGE 1-27
        GENERAL ASSEMBLER DIRECTIVES


        1.4.17  .assume Directive 

        Format:  

                .assume exp 

        where:  exp     represents   an  absolute  expression.   If  the
                        evaluation of the expression results  in  a  non
                        zero value then an <e> error is reported and the
                        text line is listed in the generated error.  


           The  .assume  directive  is useful to check assumptions about
        assembler values.  (The .assume directive is identical in  func-
        tion to the .error directive, just perhaps more descriptive.) 


        1.4.18  .radix Directive 

        Format:  

                .radix  character 

        where:  character  represents  a single character specifying the
                default radix to be used for  succeeding  numbers.   The
                character may be any one of the following:  

                        B,b     Binary
        
                        O,o     Octal
                        Q,q
        
                        D,d     Decimal
                        'blank'
        
                        H,h     Hexidecimal
                        X,x




        THE ASSEMBLER                                          PAGE 1-28
        GENERAL ASSEMBLER DIRECTIVES


        1.4.19  .even Directive 

        Format:  

                .even 

           The .even directive ensures that the current location counter
        contains an even boundary value by adding 1 if the current loca-
        tion is odd.  


        1.4.20  .odd Directive 

        Format:  

                .odd 

           The  .odd directive ensures that the current location counter
        contains an odd boundary value by adding one if the current  lo-
        cation is even.  


        1.4.21  .bndry Directive 

        Format:  

                .bndry  n 

           If  the current location is not an integer multiple of n then
        the location counter is increased to the next  integer  multiple
        of n.  

           As an example:  

                .bndry  4 

        changes  the current location to be at a multiple of 4, a 4-byte
        boundary.  

           The boundary specifications are propagated to the linker as a
        boundary modulus, ie the smallest common boundary for all  .odd,
        .even, and .bndry directives contained within the area.  A boun-
        dary value of 1 is equivalent to .odd and a boundary value of  2
        is equivalent to .even.  Because areas are always assembled with
        an initial address of 0, an even address, both  .odd  and  .even
        are modulus 2 boundaries.  

           As  an  example, suppose there are two sections:  a CODE sec-
        tion and a DATA section.  The program code is  written  so  that


        THE ASSEMBLER                                          PAGE 1-29
        GENERAL ASSEMBLER DIRECTIVES


        the  data  associated with this section of the program code fol-
        lows immediately.  

                        .area   CODE
                        ; Subroutine 1 Code
                        ; Uses data having a boundary of 6
        
                        .area   DATA
                        ; Subroutine 1 Data
                        .bndry  6
                        .word   1, 2, 3
        
                        ...
        
                        .area   CODE
                        ; Subroutine 2 Code
                        ; Uses data having a boundary of 8
        
                        .area   DATA
                        ; Subroutine 2 Data
                        .bndry  8
                        .word   1, 2, 3, 4,


           Since  the CODE and DATA sections are assembled during a sin-
        gle assembly (also applies to include files) the  the  assembler
        compiles all CODE segments as a single area segment.  The assem-
        bler also compiles all the DATA segments as a single  area  seg-
        ment  which  has  two .bndry directives and will have a boundary
        modulus of 24.  24 is the smallest boundary divisible by 6 and 8
        with  no remainder.  When the assembled file is linked the loca-
        tion of the data in the DATA area will be offset to  an  address
        which has a boundary modulus of 24.  

           When  multiple files containing the same area names (projects
        with multiple independently compiled files or library files) are
        linked  together  each  area segment will be offset to match the
        segments boundary modulus.  

           Boundary  specifications  will also be preserved when an area
        base address is specified with the -b linker option  and/or  the
        area is placed within a bank.  




        THE ASSEMBLER                                          PAGE 1-30
        GENERAL ASSEMBLER DIRECTIVES


        1.4.22  .area Directive 

        Format:  

                .area   name    [(options)] 

        where:  name    represents the symbolic name of the program sec-
                        tion.   This  name  may  be  the  same  as   any
                        user-defined  symbol  or  bank as the area names
                        are independent  of  all  symbols,  labels,  and
                        banks.  

                options specify the type of program or data area:  
                        ABS     absolute (automatically invokes OVR) 
                        REL     relocatable 
                        OVR     overlay 
                        CON     concatenate 
                        NOPAG   non-paged area 
                        PAG     paged area 

                options specify a code or data segment:  
                        CSEG    Code segment 
                        DSEG    Data segment 

                option  specifies the data area bank:  
                        BANK    Named collection of areas 


           The .area directive provides a means of defining and separat-
        ing multiple programming and data sections.   The  name  is  the
        area  label used by the assembler and the linker to collect code
        from various separately assembled modules into one section.  The
        name may be from 1 to 79 characters in length.  

           The options are specified within parenthesis and separated by
        commas as shown in the following example:  

                .area  TEST  (REL,CON)  ;This section is relocatable
                                        ;and concatenated with other
                                        ;sections of this program area.
        
                .area  DATA  (REL,OVR)  ;This section is relocatable
                                        ;and overlays other sections
                                        ;of this program area.
        
                .area  SYS   (ABS,OVR)  ;(CON not allowed with ABS)
                                        ;This section is defined as
                                        ;absolute. Absolute sections
                                        ;are always overlayed with


        THE ASSEMBLER                                          PAGE 1-31
        GENERAL ASSEMBLER DIRECTIVES


                                        ;other sections of this program
                                        ;area.
        
                .area  PAGE  (PAG)      ;This is a paged section. The
                                        ;section must be on a 256 byte
                                        ;boundary and its length is
                                        ;checked by the linker to be
                                        ;no larger than 256 bytes.
                                        ;This is useful for direct page
                                        ;areas.

           The  default  area type is REL|CON;  i.e.  a relocatable sec-
        tion which is concatenated with other sections of code with  the
        same area name.  The ABS option indicates an absolute area.  The
        OVR and CON options indicate if program  sections  of  the  same
        name  will overlay each other (start at the same location) or be
        concatenated with each other (appended to each other).  

           The  area can be specified as either a code segment, CSEG, or
        a data segment, DSEG.  The CSEG and DSEG descriptors are  useful
        when  the  microprocessor  code  and  data  unit allocations are
        unequal:  e.g.  the executable code  uses  an  allocation  of  2
        bytes for each instruction and is addressed at an increment of 1
        for every instruction, and the data uses an allocation of 1 byte
        for  each element and is addressed at an increment of 1 for each
        data byte.  The allocation units are defined by the architecture
        of the particular microprocessor.  

           The  .area  directive also provides a means of specifying the
        bank this area is associated with.  All areas associated with  a
        particular  bank  are  combined  at  link  time  into a block of
        code/data.  

           The  CSEG,  DSEG,  and  BANK options are specified within the
        parenthesis as shown in the following examples:  

                .area   C_SEG   (CSEG,BANK=C1)
                                        ;This is a code section
                                        ;and is included in bank C1
                .area   D_SEG   (DSEG,BANK=D1)
                                        ;This is a data section
                                        ;and is included in bank D1.

           Multiple  invocations  of  the  .area directive with the same
        name must specify the same options or leave  the  options  field
        blank,  this  defaults  to  the previously specified options for
        this program area.  



        THE ASSEMBLER                                          PAGE 1-32
        GENERAL ASSEMBLER DIRECTIVES


        The   ASxxxx   assemblers   automatically  provide  two  program
        sections:  


                '_CODE'         This  is  the  default  code/data  area.
                                This   program   area   is    of    type
                                (REL,CON,CSEG).  

                '_DATA'         This  is the default optional data area.
                                This   program   area   is    of    type
                                (REL,CON,DSEG).  

        The .area names and options are never case sensitive.  


        1.4.23  .bank Directive 

        Format:  

                .bank   name    [(options)] 

        where:  name    represents  the  symbolic  name of the bank sec-
                        tion.   This  name  may  be  the  same  as   any
                        user-defined  symbol  or  area as the bank names
                        are independent  of  all  symbols,  labels,  and
                        areas.   The name may be from 1 to 79 characters
                        in length.  

                options specify the parameters of the bank:  
                        BASE    base address of bank 
                        SIZE    maximum size of bank 
                        FSFX    file suffix for this bank 
                        MAP     NOICE mapping 


           The  .bank  directive allows an arbitrary grouping of program
        and/or data areas to be communicated to the  linker.   The  bank
        parameters are all optional and are described as follows:  

             1.  BASE, the  starting  address of the bank (default is 0)
                 may be defined.  This address can be overridden by  us-
                 ing  the linker -b option for the first area within the
                 bank.  The bank address is always specified  in  'byte'
                 addressing.  A first area which is not 'byte' addressed
                 (e.g.  a processor addressed by a 'word' of 2  or  more
                 bytes)  has  the  area  address  scaled to begin at the
                 'byte' address.  



        THE ASSEMBLER                                          PAGE 1-33
        GENERAL ASSEMBLER DIRECTIVES


             2.  SIZE, the  maximum  length  of  the  bank  specified in
                 bytes.  The size is always specified in terms of bytes. 

             3.  FSFX, the file suffix to be used by the linker for this
                 bank.  The suffix may not contain embedded white space. 

             4.  MAP,  NOICE   mapping   parameter   for  this  bank  of
                 code/data.  


           The options are specified within parenthesis and separated by
        commas as shown in the following example:  

                .BANK  C1  (BASE=0x0100,SIZE=0x1000,FSFX=_C1)
                                        ;This bank starts at 0x0100,
                                        ;has a maximum size of 0x1000,
                                        ;and is to be placed into
                                        ;a file with a suffix of _C1

           The parameters must be absolute (external symbols are not al-
        lowed.) 


        1.4.24  .org Directive 

        Format:  

                .org    exp 

        where:  exp     is  an absolute expression that becomes the cur-
                        rent location counter.  

        The  .org directive is valid only in an absolute program section
        and will give a <q> error if used in a relocatable program area.
        The  .org  directive specifies that the current location counter
        is to become the specified absolute value.  




        THE ASSEMBLER                                          PAGE 1-34
        GENERAL ASSEMBLER DIRECTIVES


        1.4.25  .globl Directive 

        Format:  

                .globl  sym1,sym2,...,symn 

        where:  sym1,           represent legal symbolic names.
                sym2,...        When multiple symbols are specified,
                symn            they are separated by commas.

           A  .globl directive may also have a label field and/or a com-
        ment field.  

           The  .globl directive is provided to export (and thus provide
        linkage to) symbols not  otherwise  defined  as  global  symbols
        within  a  module.   In  exporting  global symbols the directive
        .globl J is similar to:  

              J == expression or J::  

           Because  object  modules  are linked by global symbols, these
        symbols are vital to a program.  All internal symbols  appearing
        within  a  given program must be defined at the end of pass 1 or
        they will be considered undefined.  The assembly directive  (-g)
        can  be  invoked to make all undefined symbols global at the end
        of pass 1.  

           The  .globl directive and == construct can be overridden by a
        following .local directive.  


                                      NOTE

             The  ASxxxx  assemblers  use the last occurring symbol
             specification in the source file(s) as the type  shown
             in the symbol table and output to the .rel file.  






        THE ASSEMBLER                                          PAGE 1-35
        GENERAL ASSEMBLER DIRECTIVES


        1.4.26  .local Directive 

        Format:  

                .local  sym1,sym2,...,symn 

        where:  sym1,           represent legal symbolic names.
                sym2,...        When multiple symbols are specified,
                symn            they are separated by commas.

           A  .local directive may also have a label field and/or a com-
        ment field.  

           The  .local  directive is provided to define symbols that are
        local to the current assembly process.  Local  symbols  are  not
        effected  by  the assembler option -a (make all symbols global).
        In defining local symbols the directive .local J is similar to: 

              J =: expression 

           The  .local directive and the =:  construct are useful in de-
        fining symbols and constants within a header or definition  file
        that contains many symbols specific to the current assembly pro-
        cess that should not be exported into the .rel output  file.   A
        typical  usage  is  in  the definition of SFRs (Special Function
        Registers) for a microprocessor.  

           The .local directive and =:  construct can be overridden by a
        following .globl directive.  


                                      NOTE

             The  ASxxxx  assemblers  use the last occurring symbol
             specification in the source file(s) as the type  shown
             in the symbol table and output to the .rel file.  






        THE ASSEMBLER                                          PAGE 1-36
        GENERAL ASSEMBLER DIRECTIVES


        1.4.27  .equ, .gblequ, and .lclequ Directives 

        Format:  

                sym1    .equ    expr    ; equivalent to sym1  = expr
                sym2    .gblequ expr    ; equivalent to sym2 == expr
                sym3    .lclequ expr    ; equivalent to sym3 =: expr
        
                or
        
                .equ    sym1,   expr    ; equivalent to sym1  = expr
                .gblequ sym2,   expr    ; equivalent to sym2 == expr
                .lclequ sym3,   expr    ; equivalent to sym3 =: expr

           These  alternate  forms  of equivalence are provided for user
        convenience.  


        1.4.28  .if, .else, and .endif Directives 

        Format:  

                .if     expr
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif

           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  range of true condition will be processed if the expres-
        sion 'expr' is not zero (i.e.  true) and the range of false con-
        dition  will  be processed if the expression 'expr' is zero (i.e
        false).  The range of true condition is optional as is the .else
        directive  and  the range of false condition.  The following are
        all valid .if/.else/.endif constructions:  

                .if     A-4             ;evaluate A-4
                .byte   1,2             ;insert bytes if A-4 is
                .endif                  ;not zero
        
                .if     K+3             ;evaluate K+3
                .else


        THE ASSEMBLER                                          PAGE 1-37
        GENERAL ASSEMBLER DIRECTIVES


                .byte   3,4             ;insert bytes if K+3
                .endif                  ;is zero
        
                .if     J&3             ;evaluate J masked by 3
                .byte   12              ;insert this byte if J&3
                .else                   ;is not zero
                .byte   13              ;insert this byte if J&3
                .endif                  ;is zero


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        1.4.29  .iff, .ift, and .iftf Directives 

        Format:  

                .if     expr    ;'if' range Condition is
                                ;TRUE when expr is not zero
                 .ift                                   ;}
                 .      ;} range of true condition      ;}
                 .iff                                   ;} if
                 .      ;} range of false condition     ;} block
                 .iftf                                  ;}
                 .      ;} unconditional range          ;}
                .else           ;'else' range Condition is
                                ;TRUE when expr is zero
                 .ift                                   ;}
                 .      ;} range of true condition      ;}
                 .iff                                   ;} else
                 .      ;} range of false condition     ;} block
                 .iftf                                  ;}
                 .      ;} unconditional range          ;}
                .endif

           The  subconditional  assembly directives may be placed within
        conditional assembly blocks to indicate:  

                1. The assembly of an alternate body of code when
                   the condition of the block tests false.
        
                2. The assembly of non-contiguous body of code
                   within the conditional assembly block,
                   depending upon the result of the conditional


        THE ASSEMBLER                                          PAGE 1-38
        GENERAL ASSEMBLER DIRECTIVES


                   test in entering the block.
        
                3. The unconditional assembly of a body of code
                   within a conditional assembly block.


        The use of the .iff, .ift, and .iftf directives makes the use of
        the .else directive redundant.  

           Note  that  the  implementation of the .else directive causes
        the .if tested condition to be complemented.  The TRUE and FALSE
        conditions are determined by the .if/.else conditional state.  

           All  .if/.else/.endif  directives  are  limited  to a maximum
        nesting of 10 levels.  

           The  use  of the .iff, .ift, or .iftf directives outside of a
        conditional block results in a <i> error code.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        1.4.30  .ifxx Directives 


           Additional  conditional  directives are available to test the
        value of an evaluated expression:  

                .ifne   expr            ; true if expr != 0
                .ifeq   expr            ; true if expr == 0
                .ifgt   expr            ; true if expr >  0
                .iflt   expr            ; true if expr <  0
                .ifge   expr            ; true if expr >= 0
                .ifle   expr            ; true if expr <= 0

           Format:  

                .ifxx   expr
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif



        THE ASSEMBLER                                          PAGE 1-39
        GENERAL ASSEMBLER DIRECTIVES


           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  range of true condition will be processed if the expres-
        sion 'expr' is not zero (i.e.  true) and the range of false con-
        dition  will  be processed if the expression 'expr' is zero (i.e
        false).  The range of true condition is optional as is the .else
        directive  and  the range of false condition.  The following are
        all valid .ifxx/.else/.endif constructions:  

                .ifne   A-4             ;evaluate A-4
                .byte   1,2             ;insert bytes if A-4 is
                .endif                  ;not zero
        
                .ifeq   K+3             ;evaluate K+3
                .byte   3,4             ;insert bytes if K+3
                .endif                  ;is zero
        
                .ifne   J&3             ;evaluate J masked by 3
                .byte   12              ;insert this byte if J&3
                .else                   ;is not zero
                .byte   13              ;insert this byte if J&3
                .endif                  ;is zero


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        1.4.31  .ifdef Directive 

        Format:  

                .ifdef  sym
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif



        THE ASSEMBLER                                          PAGE 1-40
        GENERAL ASSEMBLER DIRECTIVES


           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  range  of true condition will be processed if the symbol
        'sym' has been defined with a .define directive or  'sym'  is  a
        variable  with  an  assigned  value else the false range will be
        processed.  The range of true condition is optional  as  is  the
        .else directive and the range of false condition.  The following
        are all valid .ifdef/.else/.endif constructions:  

                .ifdef  sym$1           ;lookup symbol sym$1
                .byte   1,2             ;insert bytes if sym$1
                .endif                  ;is defined or
                                        ;assigned a value
        
                .ifdef  sym$2           ;lookup symbol sym$2
                .else
                .byte   3,4             ;insert bytes if sym$1
                .endif                  ;is not defined and
                                        ;not assigned a value
        
                .ifdef  sym$3           ;lookup symbol sym$3
                .byte   12              ;insert this byte if sym$3
                .else                   ;is defined/valued
                .byte   13              ;insert this byte if sym$3
                .endif                  ;is not defined/valued


        Note  that the default assembler configuration of case sensitive
        means the testing for a defined symbol is also case sensitive.  

           All  .if/.else/.endif  directives  are  limited  to a maximum
        nesting of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  




        THE ASSEMBLER                                          PAGE 1-41
        GENERAL ASSEMBLER DIRECTIVES


        1.4.32  .ifndef Directive 

        Format:  

                .ifndef sym
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif

           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the condition test.  

           The  range  of true condition will be processed if the symbol
        'sym' is not defined by a .define directive and a variable 'sym'
        has  not been assigned a value else the range of false condition
        will be processed.  The range of true condition is  optional  as
        is  the  .else  directive and the range of false condition.  The
        following are all valid .ifndef/.else/.endif constructions:  

                .ifndef sym$1           ;lookup symbol sym$1
                .byte   1,2             ;insert bytes if sym$1 is
                .endif                  ;not defined and
                                        ;not assigned a value
        
                .ifndef sym$2           ;lookup symbol sym$2
                .else
                .byte   3,4             ;insert bytes if sym$1
                .endif                  ;is defined or
                                        ;is assigned a value
        
                .ifndef sym$3           ;lookup symbol sym$3
                .byte   12              ;insert this byte if sym$3
                .else                   ;is not defined/valued
                .byte   13              ;insert this byte if sym$3
                .endif                  ;is defined/valued


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        THE ASSEMBLER                                          PAGE 1-42
        GENERAL ASSEMBLER DIRECTIVES


        1.4.33  .ifb Directive 

        Format:  

                .ifb    sym
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif

           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  conditional  .ifb  is most useful when used in macro de-
        finitions to determine if the argument is blank.  The  range  of
        true  condition  will be processed if the symbol 'sym' is blank.
        The range of true condition is optional as is the  .else  direc-
        tive  and  the  range of false condition.  The following are all
        valid .ifb/.else/.endif constructions:  

                .ifb    sym$1           ;argument is not blank
                .byte   1,2             ;insert bytes if argument
                .endif                  ;is blank
        
                .ifb    sym$2           ;argument is not blank
                .else
                .byte   3,4             ;insert bytes if argument
                .endif                  ;is not blank
        
                .ifb                    ;argument is blank
                .byte   12              ;insert this byte if
                .else                   ;argument is blank
                .byte   13              ;insert this byte if
                .endif                  ;argument not blank


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  




        THE ASSEMBLER                                          PAGE 1-43
        GENERAL ASSEMBLER DIRECTIVES


        1.4.34  .ifnb Directive 

        Format:  

                .ifnb   sym
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif

           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  conditional  .ifnb is most useful when used in macro de-
        finitions to determine if the argument is not blank.  The  range
        of  true  condition will be processed if the symbol 'sym' is not
        blank.  The range of true condition is optional as is the  .else
        directive  and  the range of false condition.  The following are
        all valid .ifnb/.else/.endif constructions:  

                .ifnb   sym$1           ;argument is not blank
                .byte   1,2             ;insert bytes if argument
                .endif                  ;is not blank
        
                .ifnb   sym$2           ;argument is not blank
                .else
                .byte   3,4             ;insert bytes if argument
                .endif                  ;is blank
        
                .ifnb                   ;argument is blank
                .byte   12              ;insert this byte if
                .else                   ;argument is not blank
                .byte   13              ;insert this byte if
                .endif                  ;argument is blank


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  




        THE ASSEMBLER                                          PAGE 1-44
        GENERAL ASSEMBLER DIRECTIVES


        1.4.35  .ifidn Directive 

        Format:  

                .ifidn  sym$1,sym$2
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif

           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  conditional .ifidn is most useful when used in macro de-
        finitions to determine if  the  arguments  are  identical.   The
        range  of true condition will be processed if the symbol 'sym$1'
        is idendical to 'sym$2' (i.e.  the character strings  for  sym$1
        and  sym$2  are  the  same  consistent with the case sensitivity
        flag).  When this if statement occurs inside a  macro  where  an
        argument  substitution  may  be blank then an argument should be
        delimited with the form /symbol/ for each symbol.  The range  of
        true  condition  is  optional  as is the .else directive and the
        range  of  false  condition.   The  following  are   all   valid
        .ifidn/.else/.endif constructions:  

                .ifidn  sym$1,sym$1     ;arguments are the same
                .byte   1,2             ;insert bytes if arguments
                .endif                  ;are the sane
        
                .ifidn  sym$1,sym$2     ;arguments are not the same
                .else
                .byte   3,4             ;insert bytes if arguments
                .endif                  ;are not the same
        
                .ifidn  sym$3,sym$3     ;arguments are the same
                .byte   12              ;insert this byte if
                .else                   ;arguments are the same
                .byte   13              ;insert this byte if
                .endif                  ;arguments are not the same


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  



        THE ASSEMBLER                                          PAGE 1-45
        GENERAL ASSEMBLER DIRECTIVES


           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        1.4.36  .ifdif Directive 

        Format:  

                .ifdif  sym$1,sym$2
                .                       ;}
                .                       ;} range of true condition
                .                       ;}
                .else
                .                       ;}
                .                       ;} range of false condition
                .                       ;}
                .endif

           The  conditional  assembly directives allow you to include or
        exclude blocks of source code during the assembly process, based
        on the evaluation of the test condition.  

           The  conditional .ifdif is most useful when used in macro de-
        finitions to determine if  the  arguments  are  different.   The
        range  of true condition will be processed if the symbol 'sym$1'
        is different from 'sym$2' (i.e.  the character strings for sym$1
        and  sym$2  are the not the same consistent with the case sensi-
        tivity flag).  When this if  statement  occurs  inside  a  macro
        where  an  argument  substitution  may be blank then an argument
        should be delimited with the form /symbol/ for each symbol.  The
        range  of  true  condition is optional as is the .else directive
        and the range of false condition.  The following are  all  valid
        .ifdif/.else/.endif constructions:  

                .ifdif  sym$1,sym$2     ;arguments are different
                .byte   1,2             ;insert bytes if arguments
                .endif                  ;are different
        
                .ifdif  sym$1,sym$1     ;arguments are identical
                .else
                .byte   3,4             ;insert bytes if arguments
                .endif                  ;are different
        
                .ifdif  sym$1,sym$3     ;arguments are different
                .byte   12              ;insert this byte if
                .else                   ;arguments are different
                .byte   13              ;insert this byte if
                .endif                  ;arguments are identical


        THE ASSEMBLER                                          PAGE 1-46
        GENERAL ASSEMBLER DIRECTIVES




        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        1.4.37  Alternate .if Directive Forms 


           Format:  

                .if     cnd(,)  arg1(, arg2)

           where  the  cnd (followed by an optional comma) may be any of
        the following:  

                -------------------------------------------------------
                 condition              Assemble
                (complement)    Args    Block if:
                -------------------------------------------------------
                eq   ( ne )     expr    equal to zero
                                        (not equal to zero)
        
                gt   ( le )     expr    greater than zero
                                        (less than or equal to zero)
        
                lt   ( ge )     expr    less than zero
                                        (greater than or equal to zero)
        
                def  ( ndef )   symbol  .define'd or user set
                                        (not .define'd or user set)
        
                b    ( nb )     macro   argument present
                                symbol  (argument not present)
        
                idn  ( dif )    macro   arguments identical
                                symbol  (arguments not identical)
        
                f    ( t )      -----   only within a .if/.else/.endif
                                        conditional block
        
                tf              -----   only within a .if/.else/.endif
                                        conditional block




        THE ASSEMBLER                                          PAGE 1-47
        GENERAL ASSEMBLER DIRECTIVES


        All .if/.else/.endif directives are limited to a maximum nesting
        of 10 levels.  

           The  use of a .else directive outside a .if/.endif block will
        generate an <i> error.  Assemblies having unequal .if and .endif
        counts will cause an <i> error.  


        1.4.38  Immediate Conditional Assembly Directives 


           The  immediate conditional assembly directives allow a single
        line of code to be assembled without  using  a  .if/.else/.endif
        construct.   All  of  the previously described conditionals have
        immediate equivalents.  

           Format:  

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


        Alternate Format:  

                .iif            arg(,)          line_to_assemble
                .iif    eq      arg(,)          line_to_assemble
                .iif    ne      arg(,)          line_to_assemble
                .iif    gt      arg(,)          line_to_assemble
                .iif    le      arg(,)          line_to_assemble
                .iif    ge      arg(,)          line_to_assemble
                .iif    lt      arg(,)          line_to_assemble
                .iif    def     arg(,)          line_to_assemble
                .iif    ndef    arg(,)          line_to_assemble


        THE ASSEMBLER                                          PAGE 1-48
        GENERAL ASSEMBLER DIRECTIVES


        
                .iif    b    (,)arg(,)          line_to_assemble
                .iif    nb   (,)arg(,)          line_to_assemble
                .iif    idn  (,)arg1,arg2(,)    line_to_assemble
                .iif    dif  (,)arg1,arg2(,)    line_to_assemble
        
                .iiff                           line_to_assemble
                .iift                           line_to_assemble
                .iiftf                          line_to_assemble


        The (,) indicates an optional comma.  

           The  .iif  types b, n, idn, and dif require the commas if the
        argument(s) may be blank.  These commas may be  removed  if  the
        arguments are delimited with the form ^/symbol/ for each symbol. 

           The   immediate   conditional  directives  donot  change  the
        .if/.else/.endif nesting level.  


        1.4.39  .include Directive 

        Format:  

                .include         /string/               or 

                .include        ^/string/ 


        where:  string  represents  a string that is the file specifica-
                        tion of an ASxxxx source file.  

                /  /    represent   the  delimiting  characters.   These
                        delimiters   may   be   any   paired    printing
                        characters,  as  long  as the characters are not
                        contained within  the  string  itself.   If  the
                        delimiting characters do not match, the .include
                        directive will give the <q> error.  

           The .include directive is used to insert a source file within
        the source file currently being assembled.  When this  directive
        is encountered, an implicit .page directive is issued.  When the
        end of the specified source file is reached, an  implicit  .page
        directive is issued and input continues from the previous source
        file.  The maximum nesting level of source files specified by  a
        .include directive is five.  



        THE ASSEMBLER                                          PAGE 1-49
        GENERAL ASSEMBLER DIRECTIVES


           The  total  number  of separately specified .include files is
        unlimited as each .include file is opened and then closed during
        each pass made by the assembler.  

           The  default  directory  path,  if none is specified, for any
        .include file is the directory path of the  current  file.   For
        example:   if  the  current  source file, D:\proj\file1.asm, in-
        cludes  a  file  specified   as   "include1"   then   the   file
        D:\proj\include1.asm is opened.  


        THE ASSEMBLER                                                                                          PAGE 1-50
        GENERAL ASSEMBLER DIRECTIVES


        1.4.39.1  Including Files In Windows/DOS  - 
            Graphical Illustration of Include File Locations
            for the following command line entry:
        
                __> bin\ascheck -l -o -s  obj\prjct.rel   src\prjct\prjct.asm
        
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
        


        THE ASSEMBLER                                                                                          PAGE 1-51
        GENERAL ASSEMBLER DIRECTIVES


        1.4.39.2  Including Files in Linux  - 
            Graphical Illustration of Include File Locations
            for the following command line entry:
        
                __$ bin/ascheck -l -o -s  obj/prjct.rel   src/prjct/prjct.asm
        
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
        


        THE ASSEMBLER                                          PAGE 1-52
        GENERAL ASSEMBLER DIRECTIVES


        1.4.40  .define and .undefine Directives 

        Format:  

                .define        keyword  /string/        or 

                .define        keyword ^/string/ 

                .undefine      keyword 


        where:  keyword is  the  substitutable  string  which must start
                        with a letter and may contain any combination of
                        digits and letters.  

        where:  string  represents  a string that is substituted for the
                        keyword.  The string may contain any sequence of
                        characters including white space.  

                /  /    represent   the  delimiting  characters.   These
                        delimiters   may   be   any   paired    printing
                        characters,  as  long  as the characters are not
                        contained within  the  string  itself.   If  the
                        delimiting  characters do not match, the .define
                        directive will give the <q> error.  

           The  .define  directive specifies a user defined string which
        is substituted for the keyword.  The substitution string may it-
        self  contain other keywords that are substitutable.  The assem-
        bler resumes the parse of the line at the point the keyword  was
        found.  Care must be excersized to avoid any circular references
        within .define directives, otherwise the assembler may  enter  a
        'recursion runaway' resulting in an <s> error.  

           The  .undefine  directive removes the keyword as a substitut-
        able string.  No error is returned if the keyword  was  not  de-
        fined.  




        THE ASSEMBLER                                          PAGE 1-53
        GENERAL ASSEMBLER DIRECTIVES


        1.4.41  .setdp Directive 

        Format:  

                .setdp [base [,area]] 

        The set direct page directive has a common format in all the as-
        semblers supporting a paged mode.  The .setdp directive is  used
        to  inform  the  assembler of the current direct page region and
        the offset address within the selected area.  The normal invoca-
        tion methods are:  

                .area   DIRECT  (PAG)
                .setdp
        
                or
        
                .setdp  0,DIRECT

        for  all  the  68xx microprocessors (the 6804 has only the paged
        ram area).  The commands specify that the direct page is in area
        DIRECT and its offset address is 0 (the only valid value for all
        but the 6809 microprocessor).  Be sure to place the DIRECT  area
        at address 0 during linking.  When the base address and area are
        not specified, then zero and the current area are the  defaults.
        If  a  .setdp directive is not issued the assembler defaults the
        direct page to the area "_CODE" at offset 0.  

           The  assembler  verifies  that  any  local variable used in a
        direct variable reference is located in this area.  Local  vari-
        able  and  constant value direct access addresses are checked to
        be within the address range from 0 to 255.  

           External direct references are assumed by the assembler to be
        in the correct area and have valid  offsets.   The  linker  will
        check all direct page relocations to verify that they are within
        the correct area.  

           The  6809  microprocessor  allows the selection of the direct
        page to be on any 256 byte boundary by loading  the  appropriate
        value  into the dp register.  Typically one would like to select
        the page boundary at link time, one method follows:  


        THE ASSEMBLER                                          PAGE 1-54
        GENERAL ASSEMBLER DIRECTIVES


                .area   DIRECT  (PAG)   ; define the direct page
                .setdp
                .
                .
                .
                .area   PROGRAM
                .
                ldd     #DIRECT         ; load the direct page register
                tfr     a,dp            ; for access to the direct page

        At  link  time specify the base and global equates to locate the
        direct page:  

                -b DIRECT = 0x1000
                -g DIRECT = 0x1000

        Both  the  area address and offset value must be specified (area
        and variable names are independent).   The  linker  will  verify
        that  the  relocated  direct page accesses are within the direct
        page.  

        The  preceeding  sequence  could  be repeated for multiple paged
        areas, however an alternate method is to define a non-paged area
        and use the .setdp directive to specify the offset value:  

                .area   DIRECT          ; define non-paged area
                .
                .
                .
                .area   PROGRAM
                .
                .setdp  0,DIRECT        ; direct page area
                ldd     #DIRECT         ; load the direct page register
                tfr     a,dp            ; for access to the direct page
                .
                .
                .setdp  0x100,DIRECT    ; direct page area
                ldd     #DIRECT+0x100   ; load the direct page register
                tfr     a,dp            ; for access to the direct page

        The  linker  will  verify that subsequent direct page references
        are in the specified area and offset address range.  It  is  the
        programmers responsibility to load the dp register with the cor-
        rect page segment  corresponding  to  the  .setdp  base  address
        specified.  

           For  those  cases  where a single piece of code must access a
        defined data structure within a direct page and there  are  many
        pages,  define  a  dumby  direct page linked at address 0.  This


        THE ASSEMBLER                                          PAGE 1-55
        GENERAL ASSEMBLER DIRECTIVES


        dumby page is used only to define  the  variable  labels.   Then
        load  the dp register with the real base address but donot use a
        .setdp directive.  This method is equivalent to indexed address-
        ing,  where the dp register is the index register and the direct
        addressing is the offset.  


        1.4.42  .16bit, .24bit, and .32bit Directives 

        Format:  

                .16bit          ;specify 16-bit addressing
                .24bit          ;specify 24-bit addressing
                .32bit          ;specify 32-bit addressing


           The  .16bit, .24bit, and .32bit directives are special direc-
        tives for assembler configuration when default  values  are  not
        used.  


        1.4.43  .msb Directive 

        Format:  

                .msb    n 


           The  .msb  directive is only available in selected assemblers
        which support 24 or 32-bit addressing.  

           The  assembler operator '>' selects the upper byte (MSB) when
        included in an assembler  instruction.   The  default  assembler
        mode  is  to  select bits <15:8> as the MSB.  The .msb directive
        allows the programmer to specify a particular byte as the  'MSB'
        when the address space is larger than 16-bits.  

           The assembler directive   .msb n  configures the assembler to
        select a particular byte as MSB.  Given a 32-bit address of MNmn
        (M(3)  is  <31:24>, N(2) is <23:16>, m(1) is <15:8>, and n(0) is
        <7:0>) the following examples show how to  select  a  particular
        address byte:  

                .msb 1          ;select byte 1 of address
                                ;<M(3):N(2):m(1):n(0)>
                LD A,>MNmn      ;byte m <15:8> ==>> A
                ...
        
                .msb 2          ;select byte 2 of address


        THE ASSEMBLER                                          PAGE 1-56
        GENERAL ASSEMBLER DIRECTIVES


                                ;<M(3):N(2):m(1):n(0)>
                LD A,>MNmn      ;byte N <23:16> ==>> A
                ...
        
                .msb 3          ;select byte 3 of address
                                ;<M(3):N(2):m(1):n(0)>
                LD A,>MNmn      ;byte M <31:24> ==>> A
                ...


        1.4.44  .lohi and .hilo Directives 

        Format:  

                .lohi           ;specify LSB first output
                .hilo           ;specify MSB first output


           The .lohi and .hilo directives are special directives for as-
        sembler output configuration.  These  directives  are  currently
        only enabled in assembler 'ascheck'.  

           An  <m> error will be generated if the .lohi and .hilo direc-
        tives are both used within the same assembly source file.  


        1.4.45  .end Directive 

        Format:  

                .end 

                .end    exp 

        where:  exp     represents  any expression, including constants,
                        symbols, or labels.  


           The  .end  directive is used to specify a code entry point to
        be included in the linker output file.  Review  the  I86  and  S
        record formats described in the linker section for details.  

           The .end directive without an expression is ignored.  


        THE ASSEMBLER                                          PAGE 1-57
        GENERAL ASSEMBLER DIRECTIVES


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
