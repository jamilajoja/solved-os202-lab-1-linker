Download Link: https://assignmentchef.com/product/solved-os202-lab-1-linker
<br>
<em>                                                                                         </em>

You are to implement a two-pass linker in C, C++, or Java and submit the <strong>source </strong>code on NYU Classes, which we compile and run.

The target machine is word addressable and has a memory of 300 words, each consisting of 4 decimal digits. The first (leftmost) digit is the opcode, which is unchanged by the linker. The remaining three digits (called the address field) form either

<ul>

 <li>An immediate operand, which is unchanged.</li>

 <li>An absolute address, which is unchanged.</li>

 <li>A relative address, which is relocated.</li>

 <li>An external address, which is resolved.</li>

</ul>

Relocating relative addresses and resolving external references were discussed in class and are in the notes.

The input consists of a series of object modules, each of which contains three parts: definition list, use list, and program text. Preceding all the object modules is a single integer giving the number of modules present.

The linker processes the input twice (that is why it is called two-pass). Pass one determines the base address for each module and the absolute address for each external symbol, storing the later in the symbol table it produces. The first module has base address zero; the base address for module <em>I </em>+ 1 is equal to the base address of module <em>I </em>plus the length of module <em>I</em>. The absolute address for a symbol <em>S </em>defined in module <em>M </em>is the base address of <em>M </em>plus the relative address of <em>S </em>within <em>M</em>. Pass two uses the base addresses and the symbol table computed in pass one to generate the actual output by relocating relative addresses and resolving external references.

The definition list is a count ND (Number of Definitions) followed by ND pairs (<em>S,R</em>) where <em>S </em>is the symbol being defined and <em>R </em>is the relative address to which the symbol refers. Pass one relocates <em>R </em>forming the absolute address <em>A </em>and stores the pair (<em>S,A</em>) in the symbol table.

The use list is a count NU (Number of Uses) followed by NU pairs (<em>S,R</em>), where <em>S </em>is an external symbol used in the module and <em>R </em>is a relative address where <em>S </em>is used. The (dummy) address initially in <em>R </em>is a pointer to the next use of <em>S</em>. This linked list of uses ends with a pointer of 777.

The program text consists of a count NT (Number of Text entries) followed by NT pairs (<em>type</em>, <em>word</em>), where <em>word </em>is a 4-digit instruction as described above and <em>type </em>is a single character indicating if the address in <em>word </em>is <strong>I</strong>mmediate, <strong>A</strong>bsolute, <strong>R</strong>elative, or <strong>E</strong>xternal. NT is also the length of the module.

<strong>Other requirements: Error detection, arbitrary limits, et al.</strong>

Your program must check the input for the errors listed below. All error messages produced must be informative, e.g., “Error: X21 was used but not defined. It has been given the value 111”.

<ul>

 <li>If a symbol is multiply defined, print an error message and use the value given in the last definition.</li>

 <li>If a symbol is used but not defined, print an error message and use the value 111.</li>

 <li>If a symbol is defined but not used, print a warning message.</li>

 <li>If an absolute address exceeds the size of the machine, print an error message and use the largest legal value.</li>

 <li>If multiple symbols are listed as used in the same instruction, print an error message and ignore all but the last usage given.</li>

 <li>If a type R address in the list of Text entries exceeds the size of the module, treat the address as 0 (and relocate it since it is of type R).</li>

</ul>

You may need to set “arbitrary limits”, for example you may wish to limit the number of characters in a symbol to (say) 8. Any such limits should be clearly documented in the program and if the input fails to meet your limits, your program must print an error message. Naturally, any such limits must be large enough for all the inputs on the web. Under no circumstances should your program reference an array out of bounds, etc.

Submit the <strong>source </strong>code for your lab, together with a README file (required) describing how to compile and run it. Your program must read an input set from standard input, i.e., directly from the keyboard. It is an error for you to require the input be in a file.

You may develop your lab on any machine you wish, but must insure that it compiles and runs on the NYU system assigned to the course.

There are several sample input sets on the web. The first is shown below and the second is an re-formatted version of the first. If you use the java Scanner or C’s scanf() (which I recommend you do), inputs 1 and 2 will look the same to your program. Some of the input sets contain errors that you are to detect as described above. We will run your lab on these (and other) input sets. The expected output is also on the web. Please let me know right away if you think any of the outputs are wrong.

4

<ul>

 <li>xy 2</li>

 <li>xy 4 z 2</li>

</ul>

5 R 1004 I 5678 E 2777 R 8002 E 7777

0

1 z 3

6 R 8001 E 1777 E 1001 E 3002 R 1002 A 1010

0

<ul>

 <li>z 1</li>

 <li>R 5001 E 4777</li>

</ul>

1 z 2

1 xy 2

3 A 8000 E 1777 E 2001

The following is output annotated for clarity and class discussion. Your output is not expected to be this fancy.

Symbol Table xy=2 z=15

Memory Map

+0

0:                   R 1004                1004+0 = 1004

1:                   I 5678                                    5678

2: xy:              E 2777 -&gt;z                            2015

3:                   R 8002                8002+0 = 8002

4:                    E 7777 -&gt;xy                          7002

+5

<ul>

 <li>R 8001 8001+5 = 8006</li>

 <li>E 1777 -&gt;z 1015</li>

 <li>E 1001 -&gt;z 1015</li>

 <li>E 3002 -&gt;z 3015</li>

 <li>R 1002 1002+5 = 1007</li>

 <li>A 1010 1010</li>

</ul>

+11

<ul>

 <li>R 5001 5001+11= 5012</li>

 <li>E 4777 -&gt;z 4015</li>

</ul>

+13

<ul>

 <li>A 8000 8000</li>

 <li>E 1777 -&gt;xy 1002</li>

 <li>z: E 2001 -&gt;xy           2002</li>

</ul>