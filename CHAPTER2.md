                                    CHAPTER 2

                               THE MACRO PROCESSOR





        2.1  DEFINING MACROS 


           By  using macros a programmer can use a single line to insert
        a sequence of lines into a source program.  

           A  macro  definition is headed by a .macro directive followed
        by the source lines.  The source lines  may  optionally  contain
        dummy arguments.  If such arguments are used, each one is listed
        in the .macro directive.  

           A  macro call is the statement used by the programmer to call
        the macro source program.  It consists of the  macro  name  fol-
        lowed  by  the  real arguments needed to replace the dummy argu-
        ments used in the macro.  

           Macro  expansion  is  the insertion of the macro source lines
        into the main  program.   Included  in  this  insertion  is  the
        replacement of the dummy arguments by the real arguments.  

           Macro  directives provide a means to manipulate the macro ex-
        pansions.  Only one directive is allowed per source line.   Each
        directive  may  have  a  blank  operand  field  or  one  or more
        operands.  Legal  operands  differ  with  each  directive.   The
        macros  and  their  associated  directives  are detailed in this
        chapter.  

           Macro  directives  can replace any machine dependent mnemonic
        associated with a specific assembler.  However, the basic assem-
        bler directives cannot be replaced with a macro.  


        THE MACRO PROCESSOR                                     PAGE 2-2
        DEFINING MACROS


        2.1.1  .macro Directive 


        Format:
        
        [label:]        .macro  name, dummy argument list
        
        where:  label   represents an optional statement label.
        
                name    represents the user-assigned symbolic
                        name of the macro.  This name  may be
                        any legal symbol and may be used as a
                        label elsewhere in the program.  The
                        macro name is not case sensitive,
                        name, NAME, or nAmE all refer to the
                        same macro.
        
                ,       represents a legal macro separator
                        (comma, space, and/or tab).
        
                dummy   represents a number of legal symbols
               argument that may appear anywhere in the body of
                list    the macro definition,  even as a label.
                        These dummy symbols can be used elsewhere
                        in the program with no conflict of
                        definition.  Multiple dummy arguments
                        specified in this directive may be
                        separated by any legal separator.  The
                        detection of a duplicate or an illegal
                        symbol in a dummy argument list
                        terminates the scan and causes a <q>
                        error to be generated.


        A  comment may follow the dummy argument list in a .macro direc-
        tive, as shown below:  

                .macro  abs     a,b     ;Defines macro abs


        The  first  statement  of  a  macro  definition must be a .macro
        directive.  Defining a macro with the same name as  an  existing
        macro will generate an <m> error.  The .mdelete directive should
        be used to delete the previous macro definition before  redefin-
        ing a macro.  


        THE MACRO PROCESSOR                                     PAGE 2-3
        DEFINING MACROS


        2.1.2  .endm Directive 


        Format:
        
                .endm


        The .endm directive should not have a label.  Because the direc-
        tives .irp, .irpc, and .rept may repeat more than once the label
        will  be  defined multiple times resulting in <m> and/or <p> er-
        rors.  

           The  .endm  directive  may be followed by a comment field, as
        shown below:  

                .endm           ;end of macro

           A  comment  may  follow  the  dummy argument list in a .macro
        directive, as shown below:  

                .macro  typemsg message ;Type a message.
                jsr     typemsg
                .word   message
                .endm                   ;End of typemsg


        The  final  statement  of every macro definition must be a .endm
        directive.  The .endm directive is also used to terminate  inde-
        finite  repeat  blocks and repeat blocks.  A .endm directive en-
        countered outside a macro definition  is  flagged  with  an  <n>
        error.  


        2.1.3  .mexit Directive 


        Format:
        
                .mexit


        The  .mexit directive may be used to terminate a macro expansion
        before the end of the macro is encountered.  This  directive  is
        also  legal  within  repeat blocks.  It is most useful in nested
        macros.  The .mexit directive terminates the  current  macro  as
        though a .endm directive had been encountered.  Using the .mexit
        directive bypasses the complexities of nested conditional direc-
        tives  and  alternate  assembly paths, as shown in the following


        THE MACRO PROCESSOR                                     PAGE 2-4
        DEFINING MACROS


        example:  

                .macro  altr    N,A,B
                  .
                  .
                  .
                .if eq,N        ;Start conditional Block
                  .
                  .
                  .
                .mexit          ;Terminate macro expansion
                .endif          ;End of conditional block
                  .
                  .
                  .
                .endm           ;Normal end of macro


        In  an  assembly  where  the  symbol  N is replaced by zero, the
        .mexit directive would assemble the conditional block  and  ter-
        minate  the  macro  expansion.   When macros ar nested, a .mexit
        directive causes an exit to the next higher level of  macro  ex-
        pansion.  A .mexit directive encountered outside a macro defini-
        tion is flagged with an <n> error.  


        2.2  CALLING MACROS 


        Format:
        
                [label:]  name  real arguments
        
        where:  label   represents an optional statement label.
        
                name    represents the name of the macro, as
                        specified in the macro definition.
        
                real    represent symbolic arguments which
             arguments  replace the dummy arguments listed
                        in the .macro definition.  When
                        multiple arguments occur, they are
                        separated by any legal separator.
                        Arguments to the macro call are
                        treated as character strings, their
                        usage is determined by the macro
                        definition.

        A  macro  definition  must be established by means of the .macro


        THE MACRO PROCESSOR                                     PAGE 2-5
        CALLING MACROS


        directive before the macro can be called and expanded within the
        source program.  

           When a macro name is the same as a user label, the appearance
        of the symbol in the operator field designates the symbol  as  a
        macro  call;   the appearance of the symbol in the operand field
        designates it as a label, as shown below:  

        LESS:   mov     @r0,r1          ;LESS is a label
                 .
                 .
                 .
                bra     LESS            ;LESS is considered a label
                 .
                 .
                 .
                LESS    sym1,sym2       ;LESS is a macro call


        2.3  ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS 


           Multiple arguments within a macro must be separated by one of
        the legal separating characters (comma, space, and/or tab).  

           Macro  definition  arguments (dummy) and macro call arguments
        (real) maintain a strict positional relationship.  That is,  the
        first  real  argument in a macro call corresponds with the first
        dummy argument in the macro definition.  

           For  example,  the  following  macro definition and its asso-
        ciated macro call contain multiple arguments:  

                .macro  new     a,b,c
                  .
                  .
                  .
        
                new     phi,sig,^/C1,C2/


        Arguments which themselves contain separating characters must be
        enclosed  within  the  delimiter  construct  ^/   /  where   the
        character  '/'  may be any character not in the argument string.
        For example, the macro call:  

                new     ^/exg    x,y/,#44,ij

        causes the entire expression 


        THE MACRO PROCESSOR                                     PAGE 2-6
        ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS



                exg    x,y

        to  replace all occurrances of the symbol a in the macro defini-
        tion.  Real arguments with a macro call  are  considered  to  be
        character  strings  and  are  treated  as a single entity during
        macro expansion.  

           The  up-arrow  (^)  construction also allows another up-arrow
        costruction to be passed as part of  the  argument.   This  con-
        struction,  for example, could have been used in the above macro
        call, as follows:  

                new     ^!^/exg    x,y/!,#44,ij

        causing the entire string ^/exg    x,y/ to be passed as an argu-
        ment.  


        2.3.1  Macro Nesting 


           Macro  nesting  occurs  where  the expansion of one macro in-
        cludes a call to another macro.  The depth of nesting  is  arbi-
        trarily limited to 20.  

           To  pass  an argument containing legal argument delimiters to
        nested macros, enclose the  argument  in  the  macro  definition
        within  an up-arrow construction, as shown in the coding example
        below.  This extra set of delimiters for each level  of  nesting
        is required in the macro definition, not the in the macro call. 

                .macro  level1  dum1,dum2
                level2  ^/dum1/
                level2  ^/dum2/
                .endm
        
                .macro  level2  dum3
                dum3
                add     #10,z
                push    z
                .endm

        A call to the level1 macro, as shown below, for example:  

                level1  ^/leaz    0,x/,^/tfr     x,z/

        causes the following macro expansion to occur:  



        THE MACRO PROCESSOR                                     PAGE 2-7
        ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS


                leaz    0,x
                add     #10,z
                push    z
                tfr     x,z
                add     #10,z
                push    z

        When  macro  definitions are nested, the inner definition cannot
        be called until the outer macro has been  called  and  expanded.
        For example, in the following code:  

                .macro  lv1     a,b
                  .
                  .
                  .
                .macro  lv2     c
                  .
                  .
                  .
                .endm
                .endm

        the  lv2 macro cannot be called and expanded until the lv1 macro
        has been expanded.  Likewise, any macro defined within  the  lv2
        macro  definition  cannot  be  called and expanded until lv2 has
        also been expanded.  


        2.3.2  Special Characters in Macro Arguments 


           If  an  argument  does not contain spaces, tabs, or commas it
        may include special  characters  without  enclosing  them  in  a
        delimited construction.  For example:  

                .macro  push    arg
                mov     arg,-(sp)
                .endm
        
        
                push    x+3(%2)

        causes the following code to be generated:  

                mov     x+3(%2),-(sp)




        THE MACRO PROCESSOR                                     PAGE 2-8
        ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS


        2.3.3  Passing Numerical Arguments as Symbols 


           If the unary operator backslash (\) precedes an argument, the
        macro treats the argument as a numeric value in the current pro-
        gram  radix.   The  ascii characters representing this value are
        inserted in the macro expansion, and their function  is  defined
        in  the context of the resulting code, as shown in the following
        example:  

                .macro  inc     a,b
                con     a,\b
                b = b + 1
                .endm
        
                .macro  con     a,b
        a'b:    .word   4
                .endm
        
                ...
        
                c = 0           ;Initialize
        
                inc     x,c

        The above macro call (inc) would thus expand to:  

                x0:     .word   4

        In  this  expanded  code,  the  label x0:  results from the con-
        catenation  of  two  real  arguments.   The  single  quote   (')
        character  in  the  label a'b:  concatenates the real argument x
        and 0 as they are passed during  the  expansion  of  the  macro.
        This type of argument construction is descibed in more detail in
        a following section.  

           A  subsequent  call to the same macro would generate the fol-
        lowing code:  

                x1:     .word   4

        and  so  on,  for  later  calls.   The two macro definitions are
        necessary because the symbol associated with the dummy  argument
        b (that is, symbol c) cannot be updated in the con macro defini-
        tion, because the character 0 has replaced  c  in  the  argument
        string  (inc  x,c).   In  the  con  macro definition, the number
        passed is treated as a string argument.  (Where the value of the
        real  argument  is 0, only a single 0 character is passed to the
        macro expansion.  


        THE MACRO PROCESSOR                                     PAGE 2-9
        ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS


        2.3.4  Number of Arguments in Macro Calls 


           A  macro  can  be defined with or without arguments.  If more
        arguments appear in the macro call than in the macro definition,
        a  <q>  error  is  generated.   If fewer arguments appear in the
        macro call than in the macro definition, missing  arguments  are
        assumed to be null values.  The conditional directives .if b and
        .if nb can be used within the macro to detect missing arguments.
        The number of arguments can be determined using the .narg direc-
        tive.  


        2.3.5  Creating Local Symbols Automatically 


           A  label is often required in an expanded macro.  In the con-
        ventional macro facilituies thus far described, a label must  be
        explicitly  specified  as an argument with each macro call.  The
        user must be careful in issuing subsequent  calls  to  the  same
        macro  in  order  avoid duplicating labels.  This concern can be
        eliminated through a feature of the ASxxxx macro  facility  that
        creates a unique symbol where a label is required in an expanded
        macro.  

           ASxxxx  allows temporary symbols of the form n$, where n is a
        decimal integer.  Automatically created symbols are  created  in
        numerical order beginning at 10000$.  

           The  automatic generation of local symbols is invoked on each
        call of a macro whose definition contains a dummy argument  pre-
        ceded  by the question mark (?) character, as shown in the macro
        definition below:  

                .macro  beta    a,?b    ;dummy argument b with ?
                tst     a
                beq     b
                add     #5,a
        b:
                .endm


           A  local symbol is created automatically only when a real ar-
        gument of the macro call is either null or missing, as shown  in
        Example 1 below.  If the real argument is specified in the macro
        call, however, generation of the local symbol is  inhibited  and
        normal argument replacement occurs, as shown in Example 2 below.
        (Examples 1 and 2 are both expansions of the beta macro  defined
        above.) 


        THE MACRO PROCESSOR                                    PAGE 2-10
        ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS



        Example 1:  Create a Local Symbol for the Missing Argument
        
                beta    flag    ;Second argument is missing.
                tst     flag
                beq     10000$  ;Local symbol is created.
                add     #5,flag
        10000$:
        
        Example 2:  Do Not Create a Local Symbol
        
                beta    r3,xyz
                tst     r3
                beq     xyz
                add     #5,r3
        xyz:


           Automatically created local symbols resulting from the expan-
        sion of a macro, as described above, do not  establish  a  local
        symbol block in their own right.  

           When  a  macro  has several arguments earmarked for automatic
        local symbol generation, substituting a specific label  for  one
        such  argument  risks  assembly errors because the arguments are
        constructed at the point of macro invocation.  Therefor, the ap-
        pearance of a label in the macro expansion will create a new lo-
        cal symbol block.  The new local symbol block could leave  local
        symbol references in the previous block and their symbol defini-
        tions in the new one, causing error codes in the assembly  list-
        ing.   Furthermore  a  later  macro expansion that creates local
        symbols in the new block may duplicate one  of  the  symbols  in
        question,  causing  an additional error code <p> in the assembly
        listing.  


        2.3.6  Concatenation of Macro Arguments 


           The  apostrophe  or  single quote character (') operates as a
        legal delimiting character in macro definitions.  A single quote
        that precedes and/or follows a dummy argument in a macro defini-
        tion is removed, and the substitution of the real  argument  oc-
        curs at that point.  For example, in the following statements:  



        THE MACRO PROCESSOR                                    PAGE 2-11
        ARGUMENTS IN MACRO DEFINITIONS AND MACRO CALLS


                .macro  def     A,B,C
        A'B:    asciz   "C"
                .byte   ''A,''B
                .endm

        when the macro def is called through the statement:  

                def     x,y,^/V05.00/

        it is expanded, as follows:  

        xy:     asciz   "V05.00"
                .byte   'x,'y

        In  expanding  the  first  line, the scan for the first argument
        terminates upon finding  the  first  apostrophe  (')  character.
        Since  A is a dummy argument, the apostrphe (') is removed.  The
        scan then resumes with B;  B is also noted as another dummy  ar-
        gument.   The  two  real  arguments x and y are then concated to
        form the label xy:.  The third dummy argument is  noted  in  the
        operand field of the .asciz directive, causing the real argument
        V05.00 to be substituted in this field.  

           When  evaluating  the arguments of the .byte directive during
        expansion of the second line, the scan  begins  with  the  first
        apostrophe (') character.  Since it is neither preceded nor fol-
        lowed by a dummy argument, this apostrophe remains in the  macro
        expansion.   The  scan  then  encounters  the second apostrophe,
        which is followed by a dummy argument and is therefor discarded.
        The scan of argument A is terminated upon encountering the comma
        (,).  The third apostrophe is neither preceded nor followed by a
        dummy  argument  and  again remains in the macro expansion.  The
        fourth (and last) apostrophe is followed by another dummy  argu-
        ment and is likewise discarded.  (Four apostrophe (') characters
        were necessary in the macro definition  to  generate  two  apos-
        trophe (') characters in the macro expansion.) 




        THE MACRO PROCESSOR                                    PAGE 2-12
        MACRO ATTRIBUTE DIRECTIVES


        2.4  MACRO ATTRIBUTE DIRECTIVES 


           The  ASxxxx  assemblers  have  four directives that allow the
        user to determine certain attributes of macro arguments:  .narg,
        .nchr,  .ntyp,  and  .nval.  The use of these directives permits
        selective modifications of a macro expansion, depending  on  the
        nature  of  the  arguments  being  passed.  These directives are
        described below.  


        2.4.1  .narg Directive 


        Format:
        
        [label:]        .narg   symbol
        
        where:  label   represents an optional statement label.
        
                symbol  represents any legal symbol. This symbol
                        is equated to the number of arguments in
                        the macro call currently being expanded.
                        If a symbol is not specified, the .narg
                        directive is flagged with a <q> error.

        The .narg directive is used to determine the number of arguments
        in the macro call currently being expanded.   Hence,  the  .narg
        directive  can appear only within a macro definition;  if it ap-
        pears elsewhere, an <n> error is generated.  

           The  argument  count  includes null arguments as shown in the
        following:  

                .macro  pack    A,B,C
                .narg   cnt
                  .
                  .
                  .
                .endm
        
                pack    arg1,,arg3
                pack    arg1

        When  the  first macro pack is invoked .narg will assign a value
        of three (3) to the number of arguments cnt, which includes  the
        empty  argument.  The second invocation of macro pack has only a
        single argument specified and .narg will assign a value  of  one
        (1) to cnt.  


        THE MACRO PROCESSOR                                    PAGE 2-13
        MACRO ATTRIBUTE DIRECTIVES


        2.4.2  .nchr Directive 


        Format:
        
        [label:]        .nchr   symbol,string
        
        where:  label   represents an optional statement label.
        
                symbol  represents any legal symbol. This symbol
                        is equated to the number of characters in
                        the string of the macro call currently
                        being expanded.  If a symbol is not
                        specified, the .nchr  directive is
                        flagged with a <q> error.
        
                ,       represents any legal separator (comma,
                        space, and/or tab).
        
                string  represents a string of printable 7-bit
                        ascii characters.  If the character
                        string contains a legal separator
                        (comma, space and/or tab) the whole
                        string must be delimited using the
                        up-arrow (^) construct ^/    /.
                        If the delimiting characters do not
                        match or if the ending delimiter
                        cannot be detected because of a
                        syntactical error in the character
                        string, the .nchr directive reports
                        a <q> error.

        The .nchr directive, which can appear anywhere in an ASxxxx pro-
        gram, is used to determine the number of characters in a  speci-
        fied  character string.  This directive is useful in calculating
        the length of macro arguments.  


        THE MACRO PROCESSOR                                    PAGE 2-14
        MACRO ATTRIBUTE DIRECTIVES


        2.4.3  .ntyp Directive 


        Format:
        
        [label:]        .ntyp   symbol,arg
        
        where:  label   represents an optional statement label.
        
                symbol  represents any legal symbol. The symbol
                        is made absolute and equated to 0 if
                        arg is an absolute value or a non
                        relocatable symbol.  The symbol is made
                        absolute and equated to 1 if arg is a
                        relocatable symbol.  If a symbol is not
                        specified then the .ntyp directive is
                        flagged with a <q> error.
        
                ,       represents any legal separator (comma,
                        space, and/or tab).
        
                arg     represents any legal expression or
                        symbol. If arg is not specified
                        then the .ntyp directive is flagged
                        with a <q> error.

        The .ntyp directive, which can appear anywhere in an ASxxxx pro-
        gram, is used to determine the symbol or expression type as  ab-
        solute (0) or relocatable (1).  


        THE MACRO PROCESSOR                                    PAGE 2-15
        MACRO ATTRIBUTE DIRECTIVES


        2.4.4  .nval Directive 


        Format:
        
        [label:]        .nval   symbol,arg
        
        where:  label   represents an optional statement label.
        
                symbol  represents any legal symbol. The symbol
                        is equated to the value of arg and made
                        absolute.  If a symbol is not specified
                        then the .nval directive is flagged
                        with a <q> error.
        
                ,       represents any legal separator (comma,
                        space, and/or tab).
        
                arg     represents any legal expression or
                        symbol. If arg is not specified
                        then the .nval directive is flagged
                        with a <q> error.

        The .nval directive, which can appear anywhere in an ASxxxx pro-
        gram, is used to determine the value of arg and make the  result
        an absolute value.  


        2.5  INDEFINITE REPEAT BLOCK DIRECTIVES 


           An  indefinite  repeat block is similar to a macro definition
        with only one dummy argument.  At each expansion  of  the  inde-
        finite  repeat  range, this dummy argument is replaced with suc-
        cessive elements of a real  argument  list.   Since  the  repeat
        directive  and its associated range are coded in-line within the
        source program, this type of macro definition and expansion does
        not require calling the macro by name, as required in the expan-
        sion of the conventional macros previously described.  

           An  indefinite  repeat  block  can  appear  within or outside
        another macro definition, indefinite  repeat  block,  or  repeat
        block.   The  rules specifying indefinite repeat block arguments
        are the same as for specifying macro arguments.  


        THE MACRO PROCESSOR                                    PAGE 2-16
        INDEFINITE REPEAT BLOCK DIRECTIVES


        2.5.1  .irp Directive 


        Format:
        
            [label:]    .irp    sym,argument_list
                        .
                        .
                    (range of indefinite repeat block)
                        .
                        .
                        .endm                   
        
        where:  label   represents an optional statement label.
        
                sym     represents a dummy argument that is
                        replaced with successive real arguments
                        from the argument list.  If the dummy
                        argument is not specified, the .irp
                        directive is flagged with a <q> error.
        
                ,       represents any legal separator (comma,
                        space, and/or tab).
        
          argument_list represents a list of real arguments
                        that are to be used in the expansion
                        of the indefinite repeat range. A real
                        argument may consist of one or more
                        7-bit ascii characters; multiple
                        arguments must be separated by any
                        legal separator (comma, space, and/or
                        tab).  If an argument must contain
                        a legal separator then the up-arrow
                        (_^) construct is require for that
                        argument.  If no real arguments are
                        specified, no action is taken.
        
                range   represents the block of code to be
                        repeated once for each occurrence of
                        a real argument in the list.  The
                        range may contain other macro
                        definitions, repeat ranges and/or
                        the .mexit directive.
        
                .endm   indicates the end of the indefinite
                        repeat block range.

        The .irp directive is used to replace a dummy argument with suc-
        cessive real arguments specified  in  an  argument  list.   This


        THE MACRO PROCESSOR                                    PAGE 2-17
        INDEFINITE REPEAT BLOCK DIRECTIVES


        replacement  process  occurrs  during  the expansion of an inde-
        finite repeat block range.  


        2.5.2  .irpc Directive 


        Format:
        
            [label:]    .irpc   sym,string
                        .
                        .
                    (range of indefinite repeat block)
                        .
                        .
                        .endm                   
        
        where:  label   represents an optional statement label.
        
                sym     represents a dummy argument that is
                        replaced with successive real characters
                        from the argument string.  If the dummy
                        argument is not specified, the .irpc
                        directive is flagged with a <q> error.
        
                ,       represents any legal separator (comma,
                        space, and/or tab).
        
                string  represents a list of 7-bit ascii
                        characters.  If the string contains
                        legal separator characters (comma,
                        space, and/or tab) then the up-arrow
                        (_^) construct must delimit the string.
                        
                range   represents the block of code to be
                        repeated once for each occurrence of
                        a real argument in the list.  The
                        range may contain other macro
                        definitions, repeat ranges and/or
                        the .mexit directive.
        
                .endm   indicates the end of the indefinite
                        repeat block range.

        The .irpc directive is available to permit single character sub-
        stition.  On each iteration of the indefinite repeat range,  the
        dummy  argument  is  replaced  with successive characters in the
        specified string.  


        THE MACRO PROCESSOR                                    PAGE 2-18
        INDEFINITE REPEAT BLOCK DIRECTIVES


        2.6  REPEAT BLOCK DIRECTIVE 


           A repeat block is similar to a macro definition with only one
        argument.  The argument specifies the number of times the repeat
        block  is  inserted  into the assembly stream.  Since the repeat
        directive and its associated range are coded in-line within  the
        source program, this type of macro definition and expansion does
        not require calling the macro by name, as required in the expan-
        sion of the conventional macros previously described.  

           A repeat block can appear within or outside another macro de-
        finition, indefinite repeat block, or repeat block.  


        2.6.1  .rept Directive 


        Format:
        
            [label:]    .rept   exp
                        .
                        .
                    (range of repeat block)
                        .
                        .
                        .endm                   
        
        where:  label   represents an optional statement label.
        
                exp     represents any legal expression.
                        This value controls the number of
                        times the block of code is to be assembled
                        within the program.  When the expression
                        value is less than or equal to zero (0),
                        the repeat block is not assembled.  If
                        this value is not an absolute value, the
                        .rept directive is flagged with an <r>
                        error.
                        
                range   represents the block of code to be
                        repeated. The range may contain other
                        macro definitions, repeat ranges and/or
                        the .mexit directive.
        
                .endm   indicates the end of the repeat block
                        range.

        The  .rept  directive  is  used  to duplicate a block of code, a


        THE MACRO PROCESSOR                                    PAGE 2-19
        REPEAT BLOCK DIRECTIVE


        certain number of times, in line with other source code.  


        2.7  MACRO DELETION DIRECTIVE 


           The  .mdelete  directive  deletes  the definitions of the the
        specified macro(s).  


        2.7.1  .mdelete Directive 


        Format:
        
                .mdelete        name1,name2,...,namen
        
        where:  name1,  represent legal macro names. When multiple
                name2,  names are specified, they are separated
                ...,    by any legal separator (comma, space, and/or
                namen   tab).



        2.8  MACRO INVOCATION DETAILS 


           The invocation of a macro, indefinite repeat block, or repeat
        block has specific implications for .if-.else-.endif  constructs
        and for .list-.nlist directives.  

           At  the  point  a  macro,  indefinite repeat block, or repeat
        block is called the following occurs:  

                1)      The initial .if-.else-.endif
                        state is saved.
        
                2)      The initial .list-.nlist
                        state is saved.
        
                3)      The macro, indefinite repeat block,
                        or repeat block is inserted into the
                        assembler source code stream.  All
                        argument substitution is performed
                        at this point.

        When  the  macro  completes and after each pass through an inde-
        finite repeat block or repeat  block  the  .if-.else-.endif  and
        .list-.nlist state is reset to the initial state.  


        THE MACRO PROCESSOR                                    PAGE 2-20
        MACRO INVOCATION DETAILS


           The  reset of the .if-.else-.endif state means that the invo-
        cation of a macro, indefinite repeat block, or repeat block can-
        not  change the .if-.else-.endif state of the calling code.  For
        example the following code does not change the  .if-.else-.endif
        condition at macro completion:  

        
                .macro  fnc     A
                  .if nb,^!A!
                    ...
                    .list (meb)
                    .mexit
                  .else
                    ...
                    .nlist
                    .mexit
                  .endif
                .endm
        
        code:   fnc


           Within the macro the .if condition becomes false but the con-
        dition is not propagated outside the macro.  

           Similarly,  when  the  .list-.nlist state is changed within a
        macro the change is not propogated outside the macro.  

           The  normal  .if-.else-.endif  processing verifies that every
        .if has a corresponding .endif.  When a macro, indefinite repeat
        block,  or repeat block terminates by using the .mexit directive
        the .if-.endif checking is bypassed  because  all  source  lines
        between the .mexit and .endm directives are skipped.  


        2.9  CONTROLLING MACRO LISTINGS 


           The  basic  .list  directive enables listing of all fields in
        the assembler listing and clears the 'meb' and 'me' options.  

           When  a  macro is entered the listing is by default inhibited
        unless the 'meb' (list only binary and location) or 'me' (enable
        listing) options have been specified.  The meb option clears all
        listing options and sets the 'bin' and 'loc' options.  The  'me'
        option simply enables any previously set listing options.  If no
        listing options have been set then a list 'me' option  will  not
        cause any listing.  



        THE MACRO PROCESSOR                                    PAGE 2-21
        CONTROLLING MACRO LISTINGS


           Within  a  macro the .list/.nlist directives can set or clear
        any of the listing options but listing will only occur when  the
        'me' option is set.  


        2.10  BUILDING A MACRO LIBRARY 


           Using  the macro facilities of the ASxxxx assemblers a simple
        macro library can be built.  The macro library is built by  com-
        bining individual macros, sets of macros, or include file direc-
        tives into a single file.  Each macro entity is enclosed  within
        a .if/.endif block that selects the desired macro definitions.  

           The  selection of specific macros to be imported in a program
        is performed by three macros, .mlib, .mcall,  and  .mload,  con-
        tained in the file mlib.def.  


        2.10.1  .mlib Macro Directive 


        Format:
        
                .mlib   file
        
        where:  file    represents the macro library file name.
                        If the file name does not include a path
                        then the path of the current assembly
                        file is used.  If the file name (and/or
                        path) contains white space then the
                        path/name must be delimited with the
                        up-arrow (^) construct ^/    /.

        The .mlib directive defines two macros, .mcall and .mload, which
        when invoked will read a file, importing specific macro  defini-
        tions.   Any  previous  .mcall  and/or .mload directives will be
        deleted before the new .mcall and .mload directives are defined. 

           The  .mload  directive  is an internal directive which simply
        includes the macro library file with the listing disabled.  

           The  following  is the mlib.def file which defines the macros
        .mlib, .mcall, and .mload.  



        THE MACRO PROCESSOR                                    PAGE 2-22
        BUILDING A MACRO LIBRARY


        ;************************************************
        ;*                                              *
        ;*    A simple Macro Library Implementation     *
        ;*                                              *
        ;*              December 2008                   *
        ;*                                              *
        ;************************************************
        
        .macro .mlib    FileName
          .if b,^!FileName!
            .error 1    ; File Name Required
            .mexit
          .endif
          .mdelete      .mcall
          .macro        .mcall  a,b,c,d,e,f,g,h
            .irp sym    ^!a!,^!b!,^!c!,^!d!,^!e!,^!f!,^!g!,^!h!
              .iif nb,^!sym!    .define    .$$.'sym
            .endm
            .mload
            .irp sym    ^!a!,^!b!,^!c!,^!d!,^!e!,^!f!,^!g!,^!h!
              .if nb,^!sym!
                .iif ndef,sym'.$$.    .error  1  ; macro not found
                .undefine       .$$.'sym
                .undefine       sym'.$$.
              .endif
            .endm
          .endm ;.mcall
          .mdelete      .mload
          .macro        .mload
            .nlist
            .include ^!FileName!
            .list
          .endm ;.mload
        .endm   ;.mlib


        2.10.2  .mcall Macro Directive 


        Format:
        
                .mcall  macro1,macro2,...,macro8
        
        where:
        
                macro1, represents from 1 to 8 macro library
                macro2, references to a macro definition or
                ...,    set of macro definitions included in
                macro8  the file specified with the .mlib macro.


        THE MACRO PROCESSOR                                    PAGE 2-23
        BUILDING A MACRO LIBRARY



        As  can  be  seen  from the macro definition of .mlib and .mcall
        shown above, when .mcall is invoked temporary  symbols  are  de-
        fined  for  each macro or macro set that is to be imported.  The
        macro .mload is then invoked to  load  the  macro  library  file
        specified in the call to .mlib.  

           For example, when the following macros are invoked:  

                .mlib   crossasm.sml    ; Cross Assembler Macros
                .mcall  M6809           ; M6809 Macro Group

        The  .mlib  macro  defines the .mload macro to access the system
        macro file crossasm.sml.  Invoking the .mcall  macro  creates  a
        temporary symbol, '.$$.M6809', and then invokes the macro .mload
        to import the system macro file crossasm.sml.   The  file  cros-
        sasm.sml  contains  conditional  statements  that define the re-
        quired macros and creates  a  temporary  symbol  'M6809.$$.'  to
        indicate  the  macro group was found.  If the macro is not found
        an error message is generated.  

           The  following  is a small portion of the crossasm.sml system
        macro file which shows the M6809 macro group:  

                .title  Cross Assembler Macro Library
        
                ; This MACRO Library is Case Insensitive.
                ;
        
                ...
        
        ; Macro Based 6809 Cross Assembler
        
        .$.SML.$. =: 0
        .if     idn     a,A
          .iif  def,.$$.m6809   .$.SML.$. = -1
        .else
          .iif  def,.$$.m6809   .$.SML.$. = -1
          .iif  def,.$$.M6809   .$.SML.$. =  1
        .endif
        .iif    lt,.$.SML.$.    .define         m6809.$$.
        .iif    gt,.$.SML.$.    .define         M6809.$$.
        .iif    ne,.$.SML.$.    .include        "m6809.mac"
        
                ...
        




        THE MACRO PROCESSOR                                    PAGE 2-24
        EXAMPLE MACRO CROSS ASSEMBLERS


        2.11  EXAMPLE MACRO CROSS ASSEMBLERS 


           The  'ascheck'  subdirectory 'macroasm' contains 7 assemblers
        written using only the general macro processing facility of  the
        ASxxxx assemblers:  

                i8085.mac       -  8085 Microprocessor
                m6800.mac       -  6800 Microprocessor
                m6801.mac       -  6801 Microprocessor
                m6804.mac       -  6804 Microprocessor
                m6805.mac       -  6805 Microprocessor
                m6809.mac       -  6809 Microprocessor
                s2650.mac       -  2650 Microprocessor


           These  absolute  macro  cross  assemblers are included to il-
        lustrate the  functionality  of  the  general  macro  processing
        facility  of  the ASxxxx assemblers.  In general they are useful
        examples of actual macro implementations.  
