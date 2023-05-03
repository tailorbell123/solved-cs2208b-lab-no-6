Download Link: https://assignmentchef.com/product/solved-cs2208b-lab-no-6
<br>
<strong>===</strong><strong>Recursion  </strong>

Recursion is the process of repeating items in a self-similar way. Recursion in computer science is a method where the solution to a problem depends on solutions to smaller instances of the same problem (as opposed to iteration). A common computer programming tactic is to divide a problem into sub-problems of the same type as the original, solve those sub-problems, and combine the results. This is often referred to as the divide-and-conquer method.




In this lab, we will study a recursive subroutine that computes the factorial of a number, using the stack for temporary data storage. <strong><em><u>Recursion is not the best way to compute the factorial of a number. The intention is to illustrate the use of</u> <u>subroutines and the stack</u></em></strong>.




The factorial of an integer <em>n</em> (i.e., <em>n!</em>) is the product of all integers from 1 up to <em>n</em>. The factorial is meaningless for negative numbers. The factorial can be expressed recursively, where <em>n! = n × (n – 1)!</em> The basis case which stops the recursion is that <em>1! = 1</em>. The following C function recursively computes <em>n!,</em>




int fact(unsigned int n)

{

if(n &lt;= 1)          return 1;       else

return n * fact(n-1);

}




For the sake of experimentation only, we will add two local variables to the above function so that we can also experiment with handling local variables. Hence, the function will be as follow:




int fact(unsigned int n)

{

int x,y;        //useless, just for the sake of experimentations




x = n + 10;     //useless, just for the sake of experimentations       y = x – 10;     //useless, just for the sake of experimentations




if(n &lt;= 1)          return 1;       else

return n * fact(n-1);

}




In ARM assembly, you can convert the algorithm, as well as the caller main function, as follows:

;——————————————————————————–

AREA factorial, CODE, READONLY n    EQU 3      ENTRY

Main ADR   sp,stack      ;define the stack




MOV   r0, #n        ;prepare the parameter

STR   r0,[sp,#-4]!  ;push the parameter on the stack




SUB   sp,sp,#4      ;reserve a place in the stack for the return value




BL    Fact          ;call the Fact subroutine




LDR   r0,[sp],#4    ;load the result in r0 and pop it from the stack

ADD   sp,sp,#4      ;also remove the parameter from the stack




ADR   r1,result     ;get the address of the result variable

STR   r0,[r1]       ;store the final result in the result variable




Loop B     Loop          ;infinite loop

;——————————————————————————–      AREA factorial, CODE, READONLY

Fact STMFD sp!,{r0,r1,r2,fp,lr} ;push general registers, as well as fp and lr

MOV   fp,sp         ;set the fp for this call

SUB   sp,sp,#8      ;create space for the x and y local variables




LDR   r0,[fp,#0x18] ;get the parameter from the stack




ADD   r1,r0,#10     ;calculate the x value

STR   r1,[fp,#-0x8] ;update the value of the local variable x




SUB   r1,r0,#10     ;calculate the y value

STR   r1,[fp,#-0x4] ;update the value of the local variable y




CMP   r0,#1         ;if (n &lt;= 1)

MOVLE r0,#1         ;{ prepare the value to be returned

STRLE r0,[fp,0x14]  ;  store the returned value in the stack

BLE   ret           ;  branch to the return section

;}




SUB   r1,r0,#1      ;{ prepare the new parameter value

STR   r1,[sp,#-4]!  ;  push the parameter on the stack




SUB   sp,sp,#4      ;  reserve a place in the stack for the return value




BL    Fact          ;  call the Fact subroutine




LDR r1,[sp],#4      ;  load the result in r0 and pop it from the stack

ADD sp,sp,#4        ;  remove also the parameter from the stack




MUL   r2,r0,r1      ;  prepare the value to be returned

STR   r2,[fp,#0x14] ;  store the returned value in the stack

;}




ret  MOV   sp,fp         ;collapse all working spaces for this function call      LDMFD sp!,{r0,r1,r2,fp,pc} ;load all registers and return to the caller

;——————————————————————————–      AREA factorial, DATA, READWRITE result DCD   0x00        ;the final result

SPACE 0xB4        ;declare the space for stack stack  DCD   0x00        ;initial stack position (FD model)

;——————————————————————————–        END

<h1>PROBLEM SET</h1>

Before you start practicing this lab, you need to review and fully understand how to use <em>the Keil ARM Simulator</em>,      as        well     as        tutorial           9<em>          (Tutorial_09_ARM_Block_Move.pdf)      </em>and             tutorial           10<em> (Tutorial_10_ARM_Stack_Frames.pdf).</em>




<ol>

 <li>Fully understand the code above. Definitely, you MUST do more effort than just quickly scanning the code.</li>

 <li>How many stack bytes does the <em>Fact</em> subroutine need in each time it is called?</li>

 <li>Sketch what a stack frame looks like. Include in your drawing the pushed parameter, the pushed returning value, the location of the current FP pointer, and the offset of each item in the stack relative to the current FP</li>

 <li>How many function calls will be performed to calculate Fact(3)?</li>

 <li>Sketch the content of the stack at its maximum occupancy when Fact(3) is called?</li>

</ol>

Include in your answer  o the address of each stack element,

<ul>

 <li>the name of the stack element content (e.g., <em>local variable x, local variable y, pushed r0 register, returning value, passed parameter</em>),</li>

 <li>the value of each stack element, and o the part of the code that pushed this value.</li>

</ul>

<ol start="6">

 <li>Verify your answer in Q5 by running the program and comparing the actual stack values with the sketched stack values.</li>

 <li>The length of the current stack is 0xB4. What is the maximum factorial that can be calculated using this stack?</li>

 <li>Verify your answer in Q7 by running the program and examining the generated values for n! and (n+1)!, where n is your answer in Q7.</li>

 <li>What is the minimum stack size that you need to use to be able to correctly calculate Fact(12)?</li>

 <li>Verify your answer in Q9 by changing the stack size as suggested in your answer in Q9, then run the program, and examine the generated value for 12!.</li>

 <li>If you managed to increase the stack size to accommodate all the required pushes when calling Fact(13), will you get the correct result of 13!?</li>

 <li>Verify your answer in Q11 using the provided program.</li>

</ol>