lab4
====

ALU TEST AND DEBUG
![alt text](http://i59.tinypic.com/693vok.png)

To code the ALU, I identified the 8 operations I would need to code for.  These were LDAI, ADD, IN, OR, ROR, NOT, NEG, and AND.  The coding for these did nto give me any trouble, except for the ROR.  the first time I tried coding the ROR, I only assigned one bit value.  Because of this, when I ran my test bench all of the operations were getting U's.  Realizing that to solve this I would have to assign each of the bit values I was changing by hardcoding them myself, i added in the code 

when "011" =>Result(0) <= Accumulator(1);
					Result(1) <= Accumulator(2);
					Result(2) <= Accumulator(3);
					Result(3) <= Accumulator(0);
					
Adding this solved my problem.  I then went through each of the OpSel values with their associated Accumlator and Data values and the outputs were correct.


DATAPATH 

To begin coding the datapath, we had to declare an ALU component.  To figure out what to do next, I looked at the example Captain Silva provided to us, the Program Counter, to see what was going to be required for the other parts.  After getting a basic understanding of what was going on, I used the diagram provided in the Lab to figure out what went into each component i.e (the IR needed a reset, clock, Data, and IRLd.  Doing this, I was successfuly abel to code each part of the datapath.  To check my datapth simulation I compared it against the 50 ns of code that captain silva provided, and my testbench matched.
![alt text](http://i59.tinypic.com/e1phi.png)


REVERSE ENGINEERING

Using the first 50 ns of the testbench analyzed by captin silva as an example, we then needed to reverse engineer what was going on in the time from 50ns to 100ns, and analyze the jump at 225ns.  

50ns-100ns

AT 50 ns the instruction register has the number 7, which means it operating the LDAI operation.  IRLd is set to true, so the value of 3 on teh databus is read into the Instruction reigister on the next cycle.  The instruction register now has the OpCode of '3'.  This means it is going to ROR the value that is in the accumulator (1011).  After the value in the accumulator is RORd, the accumulator now holds a value of 1101.  The instruction register then takes in the value 4 from the databus, which means output.  The IR needs a location to output to, and it just so happens that teh databus has the value of 3, which lets the instruction register know that it needs to output to output address 3.   


225ns

The Instruction Register read in 'b', which is the op code for Jump Negative (JN).  If the accumulator value is negative at the time (in this case it was, 1101), then  it jumps to the specified address. At 205 ns, ,marlold is true, so it loads the value from the accumulator ( 2 )  into marlo, and at 215ns marhild is true, so it loads the value of the accumualtor ( 0 ) into marhi. because of this, we know that are new address is going to be 02.  At 225ns, jmpsel = '1', and then our PC value jumps to the location '02'.


BELOW IS THE PRISM PROGRAM LISTING

![alt text](http://i57.tinypic.com/eu12km.png)
