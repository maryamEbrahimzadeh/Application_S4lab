**Maryam Ebrahimzadeh 98201893**<br/>
**(State of) The Art of War: Offensive Techniques in Binary Analysis**

Vulnerabilities have been discovered in binary codes, so we need to analyze them in different approaches and with different targets. Angr is a binary analysis framework that integrates many techniques for code analysis.<br/>
In general, any automated binary analysis must adopt trade-off between replayability and semantic insight because of the analysis scalability issue. Code analysis consists of two categories: static and dynamic.<br/>
The static analysis contains the control flow graph and the data flow graph. In static analysis, results are not replayable, and false-positive error is high.<br/>
The dynamic analysis consists of two categories: **concrete** and **symbolic** execution which are highly replayable. In concrete execution, having test cases is necessary.<br/>
Fuzzing is a Dynamic concrete execution which has multiple type:<br/>
&nbsp;&nbsp;&nbsp;&nbsp;**coverage-based fuzzing:** maximize the amount of code for testing, but it does not have semantic insight.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;**Taint-based fuzzing:** try to understand the processing of the application to change input better.<br/>
The other type of dynamic analysis is dynamic symbolic execution which has good semantic insight but is limited with respect of scalability.<br/>
 
**Analysis engine** <br/>
In Angr, design points consider modern processors and OS and usability.<br/>
This design contains five submodules :<br/>
**1. Intermediate Representation**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Implement an Intermediate Representation and leveraging linVEX to support different architecture<br/>
**2. Binary Loading**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Load given binary code and its requirements then initialize program state.<br/>
**3. Program State Representation/Modification**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;SimState in SimuVEX collect state which originally specified by user<br/>
**4. Data Model**	<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Claripy module is for representing values which are in registers, as expressions and it can translate them to data itself. 
&nbsp;&nbsp;&nbsp;&nbsp;Claripy has a modular design.<br/>
**5. Full-Program Analysis**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Angr use both dynamic symbolic execution and control flow graph recovery to analyze.<br/>
 <br/>
Two main interfaces are **Path Groups** and **Analyses**. <br/>
Path group is for dynamic symbolic execution and manage paths.
Analyses are to manage full program analysis.
Angr store some true information in the knowledge base to use them.<br/>
 
Angr is a python library. And it can be installed using pip command.In Angr they used a corpus of CGC binaries, released by DARPA, to carry out their evaluation and implemented these techniques:  CFG recovery, dynamic and static vulnerability discovery, crash replay, exploitation, and exploit hardening.
<br/><br/>
<hr/>
**AEG: Automatic Exploit Generation**

Exploit generation is very hard for human so automatic exploit generation is program which find vulnerabilities and write exploit string for them.<br/>

I case of exploit generation, on one hand source code analysis is insufficient on the other hand binary level analysis is unscalable which in our approach we combine them. For this approach priority of path that will be checked is important to find exploitable path. In general this system analyze source code then extract vulnerability and then write exploit string for them.<br/> 

Actually here the problem is that AEG must find bug in code and find the way to hijack control of program.<br/>

**Approach of AEG**<br/>
AEG contains six components: <br/>
**1.PRE-PROCESS**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;AEG get 2 inputs binary and the LLVM bytecode of the same program are compile them down<br/>
**2.SRC-ANALYSIS**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Find maximum size of symbolic data by searching for the largest statically allocated buffers<br/>
**3.BUG-FIND**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Generate Π_bug, path predicate and V, source level for each vulnerability.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;In this component AEG use  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- preconditioned symbolic execution<br/> 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;In symbolic execution the total number of interpreters is exponential in the number of branches so this feature is used 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;to make this state space smaller. These precondition must be neither too specific, nor too general.<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- path prioritization techniques<br/>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;It use 2 new path prioritization heuristics: buggy-path-first and loop exhaustion<br/>
**4.DBA** <br/>
&nbsp;&nbsp;&nbsp;&nbsp;Performs dynamic binary analysis and find runtime information R<br/>
**5.EXPLOIT-GEN**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Get  Π_bug and R and generate control flow of an exploit. Two type of exploit will be generated return-to-stack  
&nbsp;&nbsp;&nbsp;&nbsp;and return-to-libc to change the return address<br/>
**6.VERIFY**<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Check that this exploit can get adversarial goal or not<br/>
<hr/>
**Questions**<br/>
* Many websites expose their “.git” files, please show how it could be dangerous.<br/>
We can be exposed to fishing attack.<br/>
Attackers  can retrieve database access passwords and other critical info using git folder. <br/>

* Imagine that we have 2**48 text files. Explain how can we find which files are the same.<br/>
Copy all files in two folder and use this command in linux <br/>
&nbsp;&nbsp;&nbsp;&nbsp;diff -r folder1 folder2<br/>
it reports that each file in identical to itself and can be identical to other ones.<br/>
for this we can use a simple python code.<br/>

* Write a hello-world C program and explain how we can dump its binary code with radare2.<br/>
 r2 -q -c 'pi $s' ./a.out > out.txt<br/>
r2 is for radaer<br/>
-q to exit from shell and stdout to file<br/>
-c to execute command in radar2<br/>
Pi print<br/>
$s file size<br/>















