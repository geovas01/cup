# cup

CUP 0.11b 

(http://www2.cs.tum.edu/projects/cup/)

CUP stands for Construction of Useful Parsers and is an LALR parser generator for Java. It was developed by C. Scott Ananian, Frank Flannery, Dan Wang, Andrew W. Appel and Michael Petter. It implements standard LALR(1) parser generation. As a parser author, you specify the symbols of Your grammar (terminal T1,T2; non terminal N1, N2;), as well as the productions (LHS :== RHS1 | RHS2 ;). If you provide each production alternative with action code ({: RESULT = myfunction(); :}), the parser will call this action code after performing a reduction with the particular production. You can use these callbacks to assemble an AST (Abstract Syntax Tree) or for arbitrary purposes. You should also have a look at the scanner generator JFlex, which is suited particularly well for collaboration with CUP.

 Main features

 LALR(1) parsing engine with symbol precedences
 Error recovery
 Syntax completion prerequisites
 Optional automatic parse tree generation
 Optional XML output
 Manual action code
 Grammar Engineering via Eclipse Plugin
 An open licence

Download

select the version of CUP, you would like to obtain:


 Get it!
FAQ

How do I program CUP?

CUP generates parsers from specifications, that you provide in a special file, whose syntax is quite similar to YACC:

/* Simple +/-/* expression language; parser evaluates constant expressions on the fly*/
import java_cup.runtime.*;

parser code {:
    // Connect this parser to a scanner!
    scanner s;
    Parser(scanner s){ this.s=s; }
:}

/* define how to connect to the scanner! */
init with {: s.init(); :};
scan with {: return s.next_token(); :};

/* Terminals (tokens returned by the scanner). */
terminal            SEMI, PLUS, MINUS, TIMES, UMINUS, LPAREN, RPAREN;
terminal Integer    NUMBER;        // our scanner provides numbers as integers

/* Non terminals */
non terminal            expr_list;
non terminal Integer    expr;      // used to store evaluated subexpressions

/* Precedences */
precedence left PLUS, MINUS;
precedence left TIMES;
precedence left UMINUS;

/* The grammar rules */
expr_list ::= expr_list expr:e SEMI         {: System.out.println(e);:}
            | expr:e SEMI                   {: System.out.println(e);:}
;
expr      ::= expr:e1 PLUS  expr:e2         {: RESULT = e1+e2;       :}
             | expr:e1 MINUS expr:e2        {: RESULT = e1-e2;       :}
             | expr:e1 TIMES expr:e2        {: RESULT = e1*e2;       :}
             | MINUS expr:e                 {: RESULT = -e;          :}
  	     %prec UMINUS
       | LPAREN expr:e RPAREN	         {: RESULT = e;           :}
       | NUMBER:n	                     {: RESULT = n;           :}
             ;
This will produce two files, parser.java and sym.java, which can be included in arbitrary Java projects.

Seriously, I still have a lot of questions

You can find more detailed information in CUP's manual:

 Take me to the documentation!

 Take me to the examples!
I found an error - what do I do?

Drop a patch to Michael Petter - if it is interesting enough, he will integrate it into the main sources.

Older versions

CUP is now nearly 20 years old now, and a lot of versions have been out there since then. You can find the old versions here:
 Browse old versions
Source code

Get the source code: svn checkout https://www2.in.tum.de/repos/cup/develop
