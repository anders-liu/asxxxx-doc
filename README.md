# asxxxx-doc

> 翻译工作正在进行中，最新进展请参见[[working](https://github.com/anders-liu/asxxxx-doc/tree/working)]分支。

ASxxxx 是 Alan R. Baldwin 主持开发的一款跨平台交叉汇编器，该汇编器本身可运行于 Linux、
Windows 以及 MS-DOS 操作系统，并可针对包括 8051 在内的超过 40 种微处理器进行汇编开发。

ASxxxx 基于 GPL 协议，可免费使用。

本库用于提供对 ASxxxx 文档的中文翻译，但仅包括其中与汇编代码开发相关的部分，也就是第一至三章（但不包括 3.6-3.13）。

[asmlnk-v5.30.txt](asmlnk-v5.30.txt)是对来自 ASxxxx 网站 TXT 格式文档的备份。

ASxxxx 官方网站：http://shop-pdp.net/ashtml/asxxxx.htm

文档页链接：http://shop-pdp.net/ashtml/asxdoc.htm

当前 TXT 格式文档链接：http://shop-pdp.net/ashtml/asmlnk.txt

**目录**

- [CHAPTER 1 THE ASSEMBLER | 第一章 汇编器](CHAPTER1.md)

  - [1.1 THE ASXXXX ASSEMBLERS | ASXXXX汇编器](CHAPTER1.md#user-content-1.1)

    - [1.1.1 Assembly Pass 1 | 第一遍汇编](CHAPTER1.md#user-content-1.1.1)
    - [1.1.2 Assembly Pass 2 | 第二遍汇编](CHAPTER1.md#user-content-1.1.2)
    - [1.1.3 Assembly Pass 3 | 第三遍汇编](CHAPTER1.md#user-content-1.1.3)

  - [1.2 SOURCE PROGRAM FORMAT | 源程序格式](CHAPTER1.md#user-content-1.2)

    - [1.2.1 Statement Format | 语句格式](CHAPTER1.md#user-content-1.2.1)
      - [1.2.1.1 Label Field | 标号字段](CHAPTER1.md#user-content-1.2.1.1)
      - [1.2.1.2 Operator Field | 操作符字段](CHAPTER1.md#user-content-1.2.1.2)
      - [1.2.1.3 Operand Field | 操作数字段](CHAPTER1.md#user-content-1.2.1.3)
      - [1.2.1.4 Comment Field | 注释字段](CHAPTER1.md#user-content-1.2.1.4)

  - [1.3 SYMBOLS AND EXPRESSIONS | 符号和表达式](CHAPTER1.md#user-content-1.3)

    - [1.3.1 Character Set | 字符集](CHAPTER1.md#user-content-1.3.1)
    - [1.3.2 User-Defined Symbols | 用户定义的符号](CHAPTER1.md#user-content-1.3.2)
    - [1.3.3 Reusable Symbols | 可重用符号](CHAPTER1.md#user-content-1.3.3)
    - [1.3.4 Current Location Counter | 当前位置计数器](CHAPTER1.md#user-content-1.3.4)
    - [1.3.5 Numbers | 数字](CHAPTER1.md#user-content-1.3.5)
    - [1.3.6 Terms | 项](CHAPTER1.md#user-content-1.3.6)
    - [1.3.7 Expressions | 表达式](CHAPTER1.md#user-content-1.3.7)

  - [1.4 GENERAL ASSEMBLER DIRECTIVES | 通用汇编器指示符](CHAPTER1.md#user-content-1.4)

    - [1.4.1 `.module` Directive | `.module`指示符](CHAPTER1.md#user-content-1.4.1)
    - [1.4.2 `.title` Directive | `.title`指示符](CHAPTER1.md#user-content-1.4.2)
    - [1.4.3 `.sbttl` Directive | `.sbttl`指示符](CHAPTER1.md#user-content-1.4.3)
    - [1.4.4 `.list` and `.nlist` Directives | `.list`和`.nlist`指示符](CHAPTER1.md#user-content-1.4.4)
    - [1.4.5 `.page` Directive | `.page`指示符](CHAPTER1.md#user-content-1.4.5)
    - [1.4.6 `.msg` Directive | `.msg`指示符](CHAPTER1.md#user-content-1.4.6)
    - [1.4.7 `.error` Directive | `.error`指示符](CHAPTER1.md#user-content-1.4.7)
    - [1.4.8 `.byte`, `.db`, and `.fcb` Directives | `.byte`、`.db`和`.fcb`指示符](CHAPTER1.md#user-content-1.4.8)
    - [1.4.9 `.word`, `.dw`, and `.fdb` Directives | `.word`、`.dw`和`.fdb`指示符](CHAPTER1.md#user-content-1.4.9)
    - [1.4.10 `.3byte` and `.triple` Directives | `.3byte`和`.triple`指示符](CHAPTER1.md#user-content-1.4.10)
    - [1.4.11 `.4byte` and `.quad` Directive | `.4byte`和`.quad`指示符](CHAPTER1.md#user-content-1.4.11)
    - [1.4.12 `.blkb`, `.ds`, `.rmb`, and `.rs` Directives | `.blkb`、`.ds`、`.rmb`和`.rs`指示符](CHAPTER1.md#user-content-1.4.12)
    - [1.4.13 `.blkw`, `.blk3`, and `.blk4` Directives | `.blkw`、`.blk3`和`.blk4`指示符](CHAPTER1.md#user-content-1.4.13)
    - [1.4.14 `.ascii`, `.str`, and `.fcc` Directives | `.ascii`、`.str`和`.fcc`指示符](CHAPTER1.md#user-content-1.4.14)
    - [1.4.15 `.ascis` and `.strs` Directives | `.ascis`和`.strs`指示符](CHAPTER1.md#user-content-1.4.15)
    - [1.4.16 `.asciz` and `.strz` Directives | `.asciz`和`.strz`指示符](CHAPTER1.md#user-content-1.4.16)
    - [1.4.17 `.assume` Directive | `.assume`指示符](CHAPTER1.md#user-content-1.4.17)
    - [1.4.18 `.radix` Directive | `.radix`指示符](CHAPTER1.md#user-content-1.4.18)
    - [1.4.19 `.even` Directive | `.even`指示符](CHAPTER1.md#user-content-1.4.19)
    - [1.4.20 `.odd` Directive | `.odd`指示符](CHAPTER1.md#user-content-1.4.20)
    - [1.4.21 `.bndry` Directive | `.bndry`指示符](CHAPTER1.md#user-content-1.4.21)
    - [1.4.22 `.area` Directive | `.area`指示符](CHAPTER1.md#user-content-1.4.22)
    - [1.4.23 `.bank` Directive | `.bank`指示符](CHAPTER1.md#user-content-1.4.23)
    - [1.4.24 `.org` Directive | `.org`指示符](CHAPTER1.md#user-content-1.4.24)
    - [1.4.25 `.globl` Directive | `.globl`指示符](CHAPTER1.md#user-content-1.4.25)
    - [1.4.26 `.local` Directive | `.local`指示符](CHAPTER1.md#user-content-1.4.26)
    - [1.4.27 `.equ`, `.gblequ`, and `.lclequ` Directives | `.equ`、`.gblequ`和`.lclequ`指示符](CHAPTER1.md#user-content-1.4.27)
    - [1.4.28 `.if`, `.else`, and `.endif` Directives | `.if`、`.else`和`.endif`指示符](CHAPTER1.md#user-content-1.4.28)
    - [1.4.29 `.iff`, `.ift`, and `.iftf` Directives | `.iff`、`.ift`和`.iftf`指示符](CHAPTER1.md#user-content-1.4.29)
    - [1.4.30 `.ifxx` Directives | `.ifxx`指示符](CHAPTER1.md#user-content-1.4.30)
    - [1.4.31 `.ifdef` Directive | `.ifdef`指示符](CHAPTER1.md#user-content-1.4.31)
    - [1.4.32 `.ifndef` Directive | `.ifndef`指示符](CHAPTER1.md#user-content-1.4.32)
    - [1.4.33 `.ifb` Directive | `.ifb`指示符](CHAPTER1.md#user-content-1.4.33)
    - [1.4.34 `.ifnb` Directive | `.ifnb`指示符](CHAPTER1.md#user-content-1.4.34)
    - [1.4.35 `.ifidn` Directive | `.ifidn`指示符](CHAPTER1.md#user-content-1.4.35)
    - [1.4.36 `.ifdif` Directive | `.ifdif`指示符](CHAPTER1.md#user-content-1.4.36)
    - 1.4.37 Alternate `.if` Directive Forms
    - 1.4.38 Immediate Conditional Assembly Directives
    - 1.4.39 `.include` Directive
        - 1.4.39.1 Including Files In Windows/DOS
        - 1.4.39.2 Including Files in Linux
    - 1.4.40 `.define` and `.undefine` Directives
    - 1.4.41 `.setdp` Directive
    - 1.4.42 `.16bit`, `.24bit`, and `.32bit` Directives
    - 1.4.43 `.msb` Directive
    - 1.4.44 `.lohi` and `.hilo` Directives
    - 1.4.45 `.end` Directive

  - 1.5 INVOKING ASXXXX
  - 1.6 ERRORS
  - 1.7 LISTING FILE
  - 1.8 SYMBOL TABLE FILE
  - 1.9 OBJECT FILE
  - 1.10 HINT FILE

- CHAPTER 2 THE MACRO PROCESSOR

  - 2.1 DEFINING MACROS

    - 2.1.1 `.macro` Directive
    - 2.1.2 `.endm` Directive
    - 2.1.3 `.mexit` Directive

  - 2.2 CALLING MACROS

  - 2.3 ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS

    - 2.3.1 Macro Nesting
    - 2.3.2 Special Characters in Macro Arguments
    - 2.3.3 Passing Numerical Arguments as Symbols
    - 2.3.4 Number of Arguments in Macro Calls
    - 2.3.5 Creating Local Symbols Automatically
    - 2.3.6 Concatenation of Macro Arguments

  - 2.4 MACRO ATTRIBUTE DIRECTIVES

    - 2.4.1 `.narg` Directive
    - 2.4.2 `.nchr` Directive
    - 2.4.3 `.ntyp` Directive
    - 2.4.4 `.nval` Directive

  - 2.5 INDEFINITE REPEAT BLOCK DIRECTIVES

    - 2.5.1 `.irp` Directive
    - 2.5.2 `.irpc` Directive

  - 2.6 REPEAT BLOCK DIRECTIVE

    - 2.6.1 `.rept` Directive

  - 2.7 MACRO DELETION DIRECTIVE

    - 2.7.1 `.mdelete` Directive

  - 2.8 MACRO INVOCATION DETAILS

  - 2.9 CONTROLLING MACRO LISTINGS

  - 2.10 BUILDING A MACRO LIBRARY

    - 2.10.1 `.mlib` Macro Directive
    - 2.10.2 `.mcall` Macro Directive

  - 2.11 EXAMPLE MACRO CROSS ASSEMBLERS

- CHAPTER 3 THE LINKER

  - 3.1 ASLINK RELOCATING LINKER
  - 3.2 INVOKING ASLINK
  - 3.3 LIBRARY PATH(S) AND FILE(S)
  - 3.4 ASLINK PROCESSING

  - 3.5 ASXXXX VERSION 5.XX (4.XX) LINKING

    - 3.5.1 Object Module Format
    - 3.5.2 Header Line
    - 3.5.3 Module Line
    - 3.5.4 Merge Mode Line
    - 3.5.5 Bank Line
    - 3.5.6 Area Line
    - 3.5.7 Symbol Line
    - 3.5.8 T Line
    - 3.5.9 R Line
    - 3.5.10 P Line
    - 3.5.11 24-Bit and 32-Bit Addressing
    - 3.5.12 ASlink V5.xx (V4.xx) Error Messages
