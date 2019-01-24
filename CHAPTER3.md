                                    CHAPTER 3

                                   THE LINKER





        3.1  ASLINK RELOCATING LINKER 


           ASLINK  is  the  companion  linker for the ASxxxx assemblers.
        The linker supports versions 3.xx, 4.xx, and 5.xx of the  ASxxxx
        assemblers.  Object files from version 3, 4, and 5 may be freely
        mixed while linking.  Note that version 3 object  files  contain
        only a subset of the options available in versions 4 and 5.  

           The  program ASLINK is a general relocating linker performing
        the following functions:  

             1.  Bind multiple object modules into a single memory image 

             2.  Resolve inter-module symbol references 

             3.  Combine  code  belonging to the same area from multiple
                 object files into a single contiguous memory region 

             4.  Search and import object module libraries for undefined
                 global variables 

             5.  Perform   byte   and   word  program  counter  relative
                 (pc or pcr) addressing calculations 

             6.  Define absolute symbol values at link time 

             7.  Define absolute area base address values at link time 

             8.  Produce Intel Hex, Motorola S, or Tandy CoCo Disk Basic
                 output files 



        THE LINKER                                              PAGE 3-2
        ASLINK RELOCATING LINKER


             9.  Produce a map of the linked memory image 

            10.  Produce  an updated listing file with the relocated ad-
                 dresses and data 



        3.2  INVOKING ASLINK 


           Starting  ASlink without any arguments provides the following
        option list and then exits:  

        Usage: [-Options] [-Option with arg] file
        Usage: [-Options] [-Option with arg] outfile file [file ...]
          -h   or NO ARGUMENTS  Show this help list
          -p   Echo commands to stdout (default)
          -n   No echo of commands to stdout
        Alternates to Command Line Input:
          -c                   ASlink >> prompt input
          -f   file[.lnk]      Command File input
        Librarys:
          -k   Library path specification, one per -k
          -l   Library file specification, one per -l
        Relocation:
          -b   area base address=expression
          -g   global symbol=expression
        Map format:
          -m   Map output generated as (out)file[.map]
          -m1    Linker generated symbols included in (out)file[.map]
          -w   Wide listing format for map file
          -x   Hexidecimal (default)
          -d   Decimal
          -q   Octal
        Output:
          -i   Intel Hex as (out)file[.i--]
          -i1    Legacy: start address record type set to 1
          -s   Motorola S Record as (out)file[.s--]
          -t   Tandy CoCo Disk BASIC binary as (out)file[.bi-]
          -j   NoICE Debug output as (out)file[.noi]
          -y   SDCDB Debug output as (out)file[.cdb]
          -o   Linked file/library object output enable (default)
          -v   Linked file/library object output disable
        List:
          -u   Update listing file(s) with link data as file(s)[.rst]
        Case Sensitivity:
          -z   Disable Case Sensitivity for Symbols
        End:
          -e   or null line terminates input


        THE LINKER                                              PAGE 3-3
        INVOKING ASLINK






                                      NOTE

             When  ASlink  is  invoked  with  a single filename the
             created output file will have the same filename as the
             .rel file.  

             When  ASlink  is  invoked  with multiple filenames the
             first filename is the output filename and the  remain-
             ing  filenames  are  linked  together  into the output
             filename.  




        Most  sytems  require  the  options to be entered on the command
        line:  

        aslink [-Options] [-Options with args] file
        
        aslink [-Options] [-Options with args] outfile file1 [file2 ...]


        Some  systems  may  request  the  arguments  after the linker is
        started at a system specific prompt:  

        aslink
        argv: -[options] -[options with args] file
        
        aslink
        argv: [-Options] [-Options with args] outfile file1 [file2 ...]


        The linker commands are explained in some more detail:  

             1.  -h        or NO ARGUMENTS Show this help list 
                 Simply prints the help list on stdout.  

             2.  -c        ASlink >> prompt mode.  
                 The  ASlink >>  prompt  mode reads linker commands from
                 stdin.  

             3.  -f file   Command file mode.  
                 The  command file mode imports linker commands from the
                 specified file (extension must be  .lnk),  imported  -c
                 and  -f  commands  are ignored.  If the directory path,


        THE LINKER                                              PAGE 3-4
        INVOKING ASLINK


                 for a file to be linked, is not specified in  the  com-
                 mand  file  then  the  path  defaults  to the .lnk file
                 directory path.  

             4.  -p/-n     enable/disable echoing commands to stdout.  

             5.  -i/-s/-t  Intel  Hex (file.i--), Motorola S (file.s--),
                 or Tandy Color Computer  Disk  Basic  (file.bi-)  image
                 output file.  

             6.  -i1       Legacy:  start address record type set to 1 
                 Use the type 1 record to indicate the program start ad-
                 dress instead of record type 3.  Record type 1 was used
                 in some older tools.  

             7.  -o/-v     Specifies      that     subsequent     linked
                 files/libraries will generate object  output  (default)
                 or  suppress  object  output.  (if option -i, -s, or -t
                 was specified) 

             8.  -z        Disable Case Sensitivity for Symbols 

             9.  -m        Generate  a  map  file (file.map).  This file
                 contains a list of the symbols (by area) with  absolute
                 addresses, sizes of linked areas, and other linking in-
                 formation.  

            10.  -m1       Linker    generated   symbols   included   in
                 (out)file[.map] 
                 The linker creates internal symbols for each area (area
                 segment) input during the linking process but  normally
                 suppresses  their  inclusion in the map file.  This op-
                 tion enables their inclusion in the map file.  

            11.  -w        Specifies  that a wide listing format be used
                 for the map file.  

            12.  -xdq      Specifies  the  number radix for the map file
                 (Hexidecimal, Decimal, or Octal).  

            13.  -u        Generate  an  updated listing file (file.rst)
                 derived from the relocated addresses and data from  the
                 linker  and  the hint file (file.hlr) output by the as-
                 sembler.  

            14.  file      File(s)  to  be  linked.  Files may be on the
                 same line as the above options or on a separate line(s)
                 one file per line or multiple files separated by spaces
                 or tabs.  


        THE LINKER                                              PAGE 3-5
        INVOKING ASLINK


            15.  -b  area=expression 
                 (one definition per line in a linker command file.) 
                 This  specifies  an area base address where the expres-
                 sion may contain constants and/or defined symbols  from
                 the linked files.  

            16.  -g  symbol=expression 
                 (one definition per line in a linker command file.) 
                 This  specifies  the value for the symbol where the ex-
                 pression may contain constants and/or  defined  symbols
                 from the linked files.  

            17.  -k  library directory path 
                 (one definition per line in a linker command file.) 
                 This  specifies one possible path to an object library.
                 More than one path is allowed.  

            18.  -l  library file specification 
                 (one definition per line in a linker command file.) 
                 This  specifies a possible library file.  More than one
                 file is allowed.  

            19.  -e  or null line, terminates input to the linker.  



        3.3  LIBRARY PATH(S) AND FILE(S) 


           The process of resolving undefined symbols after scanning the
        input object  files  includes  the  scanning  of  object  module
        libraries.   The  linker will search through all combinations of
        the library path specifications (input by the -k option) and the
        library  file  specifications (input by the -l option) that lead
        to an existing library file.  Each library file contains a  list
        (one  file  per  line)  of  modules  included in this particular
        library.  Each existing object module is scanned for a match  to
        the undefined symbol.  The first module containing the symbol is
        then linked with the previous modules to resolve the symbol  de-
        finition.   The  library  object  modules are rescanned until no
        more symbols can be resolved.   The  scanning  algorithm  allows
        resolution  of  back references.  No errors are reported for non
        existant library files or object modules.  

           The  library  file  specification may be formed in one of two
        ways:  



        THE LINKER                                              PAGE 3-6
        LIBRARY PATH(S) AND FILE(S)


             1.  If  the  library  file  contained an absolute path/file
                 specification  then  this  is   the   object   module's
                 path/file.  
                 (i.e.  C:\...  or C:/...) 

             2.  If  the  library  file  contains  a  relative path/file
                 specification then the concatenation of  the  path  and
                 this  file  specification  becomes  the object module's
                 path/file.  
                 (i.e.  \...  or /...) 


           As  an example, assume there exists a library file termio.lib
        in the syslib directory specifying the following object modules: 

        \6809\io_disk        first object module 
        d:\special\io_comm   second object module 

        and the following parameters were specified to the linker:  

        -k c:\iosystem\    the first path 
        -k c:\syslib\      the second path 

        -l termio          the first library file 
        -l io              the second library file (no such file) 

        The  linker  will attempt to use the following object modules to
        resolve any undefined symbols:  

        c:\syslib\6809\io_disk.rel     (concatenated path/file) 
        d:\special\io_comm.rel         (absolute path/file) 

        all  other path(s)/file(s) don't exist.  (No errors are reported
        for non existant path(s)/file(s).) 


        3.4  ASLINK PROCESSING 


           The  linker  processes  the  files  in  the  order  they  are
        presented.  The first pass through the input files  is  used  to
        define  all  program  areas, the section area sizes, and symbols
        defined or referenced.  Undefined symbols will initiate a search
        of any specified library file(s) and the importing of the module
        containing the symbol definition.  After the first pass  the  -b
        (area  base  address) definitions, if any, are processed and the
        areas linked.  



        THE LINKER                                              PAGE 3-7
        ASLINK PROCESSING


           The  area  linking proceeds by first examining the area types
        ABS, CON, REL, OVR and PAG.  Absolute areas (ABS) from  separate
        object modules are always overlayed and have been assembled at a
        specific address, these are not normally relocated (if a -b com-
        mand  is  used  on an absolute area the area will be relocated).
        Relative areas (normally defined as REL|CON) have a base address
        of  0x0000  as read from the object files, the -b command speci-
        fies the beginning address of the area.  All subsequent relative
        areas  will  be  concatenated  with  preceeding  relative areas.
        Where specific ordering is desired, the first linker input  file
        should  have  the area definitions in the desired order.  At the
        completion of the area linking all area  addresses  and  lengths
        have  been determined.  The areas of type PAG are verified to be
        on a 256 byte boundary and that the length does not  exceed  256
        bytes.  Any errors are noted on stderr and in the map file.  

        The  linker  also  automatically  generates two symbols for each
        linked program area:  

            'a_<area>'      The starting address of the area.  

            'l_<area>'      The length of the area.  

        and two symbols for each area segment:  

            'm_<area>_n'    The boundary modulus of the area segment.  

            's_<area>_n'    The starting address of the area segment.  

        The  appended  '_n'  signifies  the area segment number within a
        linked area.  

           These  symbols  are in general only useful diagnostically and
        are not visible externally.  However if the -m1 linker option is
        used these symbols will be output to the map file.  

           Next  the  global symbol definitions (-g option), if any, are
        processed.  The symbol definitions have been delayed until  this
        point because the absolute addresses of all internal symbols are
        known and can be used in the expression calculations.  

           Before  continuing  with the linking process the symbol table
        is scanned to determine if any symbols have been referenced  but
        not defined.  Undefined symbols are listed on the stderr device.
        if a .module directive was included in the  assembled  file  the
        module  making  the reference to this undefined variable will be
        printed.  



        THE LINKER                                              PAGE 3-8
        ASLINK PROCESSING


           Constants  defined  as global in more than one module will be
        flagged as multiple definitions if their values are not  identi-
        cal.  

           After  the  preceeding  processes are complete the linker may
        output a map file (-m option).  This file provides the following
        information:  

             1.  Global symbol values and label absolute addresses 

             2.  Defined areas and there lengths 

             3.  Remaining undefined symbols 

             4.  List of modules linked 

             5.  List of library modules linked 

             6.  List of -b and -g definitions 




           The final step of the linking process is performed during the
        second pass of the input files.  As the xxx.rel files  are  read
        the code is relocated by substituting the physical addresses for
        the referenced symbols and areas and may  be  output  in  Intel,
        Motorola, or Tandy CoCo Disk Basic formats.  The number of files
        linked and symbols defined/referenced is limited by the  proces-
        sor  space  available to build the area/symbol lists.  If the -u
        option is specified then the listing files (file.lst) associated
        with  the  relocation  files  (file.rel) are scanned and used to
        create a new file (file.rst) which has all  addresses  and  data
        relocated to their final values.  

           The  -o/-v  options  allow the simple creation of loadable or
        overlay modules.  Loadable and overlay modules normally need  to
        be  linked  with  a  main module(s) to resolve external symbols.
        The -o/-v options can be used to enable object  output  for  the
        loadable  or overlay module(s) and suppress the object code from
        the linked main module(s).  The -o/-v  options  can  be  applied
        repeatedly  to specify a single linked file, groups of files, or
        libraries for object code inclusion or suppression.  


        THE LINKER                                              Page 3-9
        ASXXXX VERSION 5.XX (4.XX) LINKING


        3.5  ASXXXX VERSION 5.XX (4.XX) LINKING 


           The  linkers'  input  object file is an ascii file containing
        the information needed by the linker  to  bind  multiple  object
        modules into a complete loadable memory image.  

        The object module contains the following designators:  

                [XDQ][HL][234]
                        X       Hexidecimal radix
                        D       Decimal radix
                        Q       Octal radix
        
                        H       Most significant byte first
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


        3.5.1  Object Module Format 


           The   first   line   of   an   object   module  contains  the
        [XDQ][HL][234] format specifier  (i.e.   XH2  indicates  a  hex-
        idecimal  file  with  most significant byte first and 16-bit ad-
        dressing) for the following designators.  




        THE LINKER                                             PAGE 3-10
        ASXXXX VERSION 5.XX (4.XX) LINKING


        3.5.2  Header Line 

                H aa areas gg global symbols 

           The  header  line  specifies  the number of areas(aa) and the
        number of global symbols(gg) defined or referenced in  this  ob-
        ject module segment.  


        3.5.3  Module Line 

                M name 

           The  module  line  specifies  the module name from which this
        header segment was assembled.  The module line will  not  appear
        if the .module directive was not used in the source program.  


        3.5.4  Merge Mode Line 

                G nn ii 00 01 02 03 04 05 06 07 08 09 0A 0B 0C 0D 0E 0F 

           The  mode  structure  contains  the specification (or partial
        specification) of one of the assemblers' merge  modes.   Sixteen
        bits  may  be  specified  on a single line.  Each assembler must
        specify at least one merge mode.  The merging specification  al-
        lows  arbitrarily defined active bits and bit positions.  The 32
        element arrays are indexed from 0 to 31.  Index 0 corresponds to
        bit  0,  ...,  and  31 corresponds to bit 31 of a normal integer
        value.  

             1.   nn is merge mode number 

             2.   ii is the beginning bit position of the following data 

             3.   00 ...  merge mode bit elements 


                    The value of the element specifies if the normal in-
                    teger bit is active (bit <7> is set, 0x80) and  what
                    destination  bit  (bits  <4:0>,  0  -  31) should be
                    loaded with this normal integer bit.  




        THE LINKER                                             PAGE 3-11
        ASXXXX VERSION 5.XX (4.XX) LINKING


        3.5.5  Bank Line 

                B name base nn size nn map nn flags nn fsfx string 

           The  B  line  defines a bank identifier as name.  A bank is a
        structure containing a collection of areas.  The bank is treated
        as  a  unique linking structure seperate from other banks.  Each
        bank can have a unique base  address  (starting  address).   The
        size  specification  may  be  used to signal the overflow of the
        banks' allocated space.  The Linker combines all areas  included
        within  a  bank  as  seperate from other areas.  The code from a
        bank may be output to a unique file by specifying the File  Suf-
        fix  parameter  (fsfx).   This allows the seperation of multiple
        data and code segments into  isolated  output  files.   The  map
        parameter  is  for  NOICE processing.  The flags indicate if the
        parameters have been set.  


        3.5.6  Area Line 

                A label size ss flags ff [bank bb] [bndry mm] 

           The  area  line  defines the area label, the size (ss) of the
        area in bytes, the area  flags  (ff),  the  optional  [bank  bb]
        specifies  the  bank  this area is a member of, and the optional
        [bndry mm] which specifies the boundary modulus  for  this  area
        segment.  The area flags specify the ABS, REL, CON, OVR, and PAG
        parameters:  

                OVR/CON  (0x04/0x00 i.e.  bit position 2) 

                ABS/REL  (0x08/0x00 i.e.  bit position 3) 

                PAG      (0x10 i.e.  bit position 4) 

           The  bank label is optional and only specified if the area is
        to be included within a bank.  

           When  this area (area segment) is linked and their is a boun-
        dary modulus specified then the code/data beginning address will
        be  increased to match the boundary modulus.  This will also in-
        crease the area (area segment) size by the same amount.  




        THE LINKER                                             PAGE 3-12
        ASXXXX VERSION 5.XX (4.XX) LINKING


        3.5.7  Symbol Line 

                S name Defnnnn 

                        or 

                S name Refnnnn 

           The symbol line defines (Def) or references (Ref) the identi-
        fier name with the value nnnn.  The defined value is relative to
        the  current area base address.  References to constants and ex-
        ternal global symbols will always appear before the  first  area
        definition.  References to external symbols will have a value of
        zero.  


        3.5.8  T Line 

                T xx xx nn nn nn nn nn ...  

           The  T  line contains the assembled code output by the assem-
        bler with xx xx being the offset address from the  current  area
        base address and nn being the assembled instructions and data in
        byte format.  (xx xx and nn nn can be 2, 3, or 4 bytes as speci-
        fied by the .REL file header.) 


        3.5.9  R Line 

                R 0 0 nn nn n1 n2 xx xx ...  

           The R line provides the relocation information to the linker.
        The nn nn value is the current area index, i.e.  which area  the
        current  values  were  assembled.  Relocation information is en-
        coded in groups of 4 bytes:  

             1.  n1 is the relocation mode and object format.  
                 1.  bits <1:0> specify the number of bytes to output 
                 2.  bits <2:3> normal(0x00) /   MSB   (0x0C) 
                                signed(0x04) / unsigned(0x08) 
                 3.  bit 4  normal(0x00)/page   '0' (0x10) reference 
                 4.  bit 5  normal(0x00)/page 'nnn' (0x20) reference 
                            PAGX mode if both bits are set (0x30) 
                 5.  bit 6  normal(0x00)/PC relative(0x40) relocation 
                 6.  bit 7  relocatable area(0x00)/symbol(0x80) 

             2.  n2 is a byte index and a merge mode index 


        THE LINKER                                             PAGE 3-13
        ASXXXX VERSION 5.XX (4.XX) LINKING


                 1.  bits <3:0>  are a byte index into the corresponding
                     (i.e.  preceeding) T line data (i.e.  a pointer  to
                     the data to be updated by the relocation).  
                 2.  bits <7:4> are an index into a selected merge mode.
                     Currently mode 0 simply specifies to  use  standard
                     byte processing modes and merging is ignored.  

             3.  xx xx  is the area/symbol index for the area/symbol be-
                 ing referenced.  the corresponding area/symbol is found
                 in the header area/symbol lists.  


        The groups of 4 bytes are repeated for each item requiring relo-
        cation in the preceeding T line.  


        3.5.10  P Line 

                P 0 0 nn nn n1 n2 xx xx 

           The  P  line provides the paging information to the linker as
        specified by a .setdp directive.  The format of  the  relocation
        information is identical to that of the R line.  The correspond-
        ing T line has the following information:  
                T xx xx aa aa bb bb 

           Where  aa aa is the area reference number which specifies the
        selected page area and bb bb is the base address  of  the  page.
        bb bb will require relocation processing if the 'n1 n2 xx xx' is
        specified in the P line.  The linker will verify that  the  base
        address is on a 256 byte boundary and that the page length of an
        area defined with the PAG type is not larger than 256 bytes.  

           The  linker  defaults any direct page references to the first
        area defined in the input REL file.  All ASxxxx assemblers  will
        specify the _CODE area first, making this the default page area. 




        THE LINKER                                             PAGE 3-14
        ASXXXX VERSION 5.XX (4.XX) LINKING


        3.5.11  24-Bit and 32-Bit Addressing 


           When  24-bit  or  32-bit  addressing is specified in the file
        format line [XDQ][HL][234] then the S and T Lines have  modified
        formats:  
                S name Defnnnnnn                        (24-bit)
                S name Refnnnnnn                        (24-bit)
                T xx xx xx nn nn nn nn nn ...           (24-bit)
        
                S name Defnnnnnnnn                      (32-bit)
                S name Refnnnnnnnn                      (32-bit)
                T xx xx xx xx nn nn nn nn nn ...        (32-bit)

           The  multibyte  formats for byte data replace the 2-byte form
        for 16-bit data with 3-byte or 4-byte data for 24-bit or  32-bit
        data  respectively.  The 2nd byte format (also named MSB) always
        uses the second byte of the 2, 3, or 4-byte data.  


        3.5.12  ASlink V5.xx (V4.xx) Error Messages 


           The linker provides detailed error messages allowing the pro-
        grammer to quickly find the errant code.   As  the  linker  com-
        pletes  pass 1  over  the  input  file(s)  it  reports  any page
        boundary or page length errors as follows:  

        ?ASlink-Warning-Paged Area PAGE0 Boundary Error
        
        and/or
        
        ?ASlink-Warning-Paged Area PAGE0 Length Error

        where PAGE0 is the paged area.  

           Also  during  Pass   1  any bank size (length) errors will be
        reported as follows:  

        ?ASlink-Warning-Size limit exceeded in bank BANK
        where BANK is the bank name.  

           During  Pass  two the linker reads the T, R, and P lines per-
        forming the necessary relocations and  outputting  the  absolute
        code.  Various errors may be reported during this process 


        THE LINKER                                             PAGE 3-15
        ASXXXX VERSION 5.XX (4.XX) LINKING


        The P line processing can produce only one possible error:  

        ?ASlink-Warning-Page Definition Boundary Error
                 file        module      pgarea    pgoffset
          PgDef  t6809l      t6809l      PAGE0         0001

        The error message specifies the file and module where the .setdp
        direct was issued and indicates  the  page  area  and  the  page
        offset value determined after relocation.  


        The R line processing produces various error messages:  

        ?ASlink-Warning-Signed value error
        ?ASlink-Warning-Unsigned value error
        ?ASlink-Warning-Byte PCR relocation error
        ?ASlink-Warning-Word PCR relocation error
        ?ASlink-Warning-3-Byte PCR relocation error
        ?ASlink-Warning-4-Byte PCR relocation error
        ?ASlink-Warning-Page0 relocation error
        ?ASlink-Warning-PageN relocation error
        ?ASlink-Warning-PageX relocation error
        ?ASlink-Warning-Signed Merge Bit Range error
        ?ASlink-Warning-Unsigned/Overflow Merge Bit Range error

        These  error  messages  also specify the file, module, area, and
        offset within the area of the code referencing (Refby)  and  de-
        fining (Defin) the symbol:  

        ?ASlink-Warning-Signed value error for symbol  two56
                 file        module      area        offset
          Refby  t           Pagetest    PROGRAM     0006
          Defin  t           Pagetest    DIRECT      0100

        If the symbol is defined in the same module as the reference the
        linker is unable to report the symbol name.  The assembler list-
        ing  file(s) should be examined at the offset from the specified
        area to locate the offending code.  

           The errors are:  

             1.  The  Signed value error indicates an indexing value ex-
                 ceeded the maximum negative or maximum  positive  value
                 for the current variable size.  

             2.  The  Unsigned  value  error indicates an indexing value
                 was greater than maximum positive value for the current
                 variable size.  



        THE LINKER                                             PAGE 3-16
        ASXXXX VERSION 5.XX (4.XX) LINKING


             3.  The  byte PCR error is caused by exceeding the pc rela-
                 tive byte branch range.  

             4.  The  word PCR error is caused by exceeding the pc rela-
                 tive word branch range.  

             5.  The  3-byte PCR error is caused by exceeding the pc re-
                 lative 3-byte branch range.  

             6.  The  4-byte PCR error is caused by exceeding the pc re-
                 lative 4-byte branch range.  

             7.  The  Page0  error is generated if the direct page vari-
                 able is not in the page0 range of 0 to 255.  

             8.  The  PageN  error is generated if the direct page vari-
                 able is not within the Nth page range of 0 to 255.  

             9.  The  PageX  error is generated if the direct page vari-
                 able is not within the extended page range.  

            10.  The  Signed Merge Bit Range error indicates an indexing
                 value exceeded the maximum negative or maximum positive
                 value for the current signed merge variable size.  

            11.  The  Unsigned/Overflow  Merge Bit Range error indicates
                 an indexing value was  greater  than  maximum  positive
                 value for the current unsigned merge variable size.  

