# QBitSimulator
Quantum computer simulator with a visual representation of the data

Unfortunately, I mostly made this for myself so I could understand the transition between the quantum and classical worlds a bit better, so it probably isn't super user friendly.  A couple of people have expressed interest in it so I've decided to post it publically. A preliminary version of a tutorial is pasted below. Since there doesn't seem to be a way to turn off Github's creative reinterpretation of newlines, you will have to press "code" or "raw" to see the ASCII diagrams correctly.



Quantum computing convention for bit order:
bit0 is the leftmost bit, but unlike with numeric counting it is considered the MOST significant bit instead of the least. Therefore, when you order the states, it ends up looking like binary counting:
  |00>+|01>+|10>+|11>
even though qbit 0 is the leftmost instead of rightmost. I ended up following this convention unconsciously from looking at other examples without actually thinking about it.

//
//Doing Hadamard Gate by hand:
//

Hadamard Gate:
  |0> -> |0> + |1>
  |1> -> |0> - |1>

Matrix form:
  1	1
  1	-1

Applied to |0>+|1> and sometimes leaving out the |> because it can become visually redundant and I'm lazy:
  0+1  
  ->   applying Hadmard gate
  (0+1)+(0-1)
  ->   grouping like terms and putting in order
  2 |0>
Normalized: 
  |0>

//
//Apply Hadmard gate to first bit of |00>+|11>
//
Again, doing by hand:
  00
  11
  ->  applying Hadmard gate to bit 0.
  (0+1)0
  (0-1)1
  ->
  00+10
  01-11
  ->  grouping like terms and putting in order
  00
  01
  10
  -11
or
  |00>+|01>+|10>-|11>
Normalized:  
  0.70711|00> + 0.70711|01> + 0.70711|10> - 0.70711|11>



//
//Building up the entangled system from the previous example using simulator/calculator:
//

Start with one qbit:  
  |0>
Code: (put into code box at bottom of screen and press calculate)
  QSystem={"states":[[1,"0"],[0,"1"]],"names":["a"]};
Interface: Press "Zero QSystem" and "-" above any extra qbits you need to remove.

Add another qbit at position 1:
  |00>
Code: 
  QSystem={"states":[[1,"0"],[0,"1"]],"names":["a"]};
  AddBit(1);
Data Structure Code:
  QSystem={"states":[[1,"0"],[0,"1"]],"names":["a"]};
Interface:  press "+" (to the right of qbits)

Apply Hadamard gate to bit 0:  |0>  ->  |0> + |1>,  |1>  ->  |0> - |1>
  00
  ->
  (0+1)0
  ->
  00+10
  or
  |00>+|10>
Normalized:
  0.70711|00> + 0.70711|10>
Code: 
  QSystem={"states":[[1,"0"],[0,"1"]],"names":["a"]};
  AddBit(1);
  Had(0);
Data Structure Code:
  QSystem={"states":[[0.7071067811865475,"00"],[0,"01"],[0.7071067811865475,"10"],[0,"11"]],"names":["a","b"]}
Interface:   Transform: "Had", and click on qbit "a"
Hadamard Transform Matrix: 
  1	1
  1	-1
Transform Matrix Normalized:
  0.70711	0.70711
  0.70711	-0.70711
Transform Data Structure:
  [
  ["0", [[0.7071067811865476,"0"],[0.7071067811865476,"1"]]],
  ["1", [[0.7071067811865476,"0"],[-0.7071067811865476,"1"]]]
  ]

Apply CNot gate to bit "a" operating on bit "b":  |00>  ->  |00>,  |01>  ->  |01>,  |10>  ->  |11>,  |11>  ->  |10>
  00+10
  ->
  (00)+(11)
  ->
  00+11
  or
  |00>+|11>
Normalized:
  0.70711|00> + 0.70711|11>
Code: 
  QSystem={"states":[[1,"0"],[0,"1"]],"names":["a"]};
  AddBit(1);
  Had(0);
  CNot(0,1);
Data Structure Code:
  QSystem={"states":[[0.7071067811865475,"00"],[0,"01"],[0,"10"],[0.7071067811865475,"11"]],"names":["a","b"]}
Interface:   Transform: "CNot", and click on qbit "a" followed by qbit "b"
Hadamard Transform Matrix: 
  1	0	0	0
  0	1	0	0
  0	0	0	1
  0	0	1	0
Transform Matrix Normalized:
  1	0	0	0
  0	1	0	0
  0	0	0	1
  0	0	1	0
Transform Data Structure:
  [
  ["00", [[1,"00"]]],
  ["01", [[1,"01"]]],
  ["10", [[1,"11"]]],
  ["11", [[1,"10"]]]
  ]

//
//Constructing a Bell state:
//
qbit state after running each line of code:
  00       QSystem={"states":[[1,"00"],[0,"01"],[0,"10"],[0,"11"]],"names":["a","b"]}
  00+10    Had(0);
  00+11    CNot(0,1);
  01+10    Not(0);    (Not(1) also works)
Normalized:  
  0.70711|01> + 0.70711|10>


//
//Switching bits:
//
Start with a 3 qbit state:
  -0.5|001> + 0.5|010> + 0.5|101> - 0.5|110>
Rows:
  0  000
  .5 001
  .5 010
  0  011
  0  100
  .5 101
  .5 110
  0  111
Code: 
  QSystem={"states":[[0,"000"],[-0.5,"001"],[0.5,"010"],[0,"011"],[0,"100"],[0.5,"101"],[-0.5,"110"],[0,"111"]],"names":["a","b","c"]}

Move qbit "b" to position "a" (which puts "a" in position formerly occupied by "b"):
  -0.5|001> + 0.5|100> + 0.5|011> - 0.5|110>
Rows:
  0  000
  .5 001
  .5 100
  0  101
  0  010
  .5 011
  .5 110
  0  111
Code:
  MoveBit(1,0);
Data Code:
  QSystem={"states":[[0,"000"],[-0.5,"001"],[0.5,"100"],[0,"101"],[0,"010"],[0.5,"011"],[-0.5,"110"],[0,"111"]],"names":["b","a","c"]}
Interface:  Click on "b" and drag to column position "a"

Reorder by qbit number:    (for 3 qbits: 000, 001, 010, 100, 101, 110, 111)
  -0.5|001> + 0.5|011> + 0.5|100> - 0.5|110>
Rows change places, along with their complex amplitudes (compare to rows list above):
  0  000
  .5 001
  0  010 <--.
  .5 011 <--|--.
  .5 100 <--*  |
  0  101 <-----*
  .5 110
  0  111
Code:
  MoveBit(1,0);
  OrderByQbit(QSystem,0);
Interface: click on the dot above "b"

//
//Applying Hadmard operation to a Qbit other than the first one:
//
Doing the calculation for one state
  00       QSystem={"states":[[1,"00"],[0,"01"],[0,"10"],[0,"11"]],"names":["a","b"]}
  0(0+1)   Had(1); 
  00+01
Normalized:
  0.70711|00> + 0.70711|01>

The way we implement Had(1) is to move it to position 0 before applying the gate to the qbit now at position 0.
In terms of code:
  Had(1) = MoveBit(1,0);Had(0);
This takes advantage of the fact that it is easy to apply a quantum gate/transform to the first qbit of a multi-qbit system:
Normal Hadmard gate:
   0       1
0  0.70711 0.70711
1  0.70711 -0.70711
Applied to first bit (0) in a 2 qbit system:
    00      01      10       11
00  0.70711 0       0.70711  0
01  0       0.70711 0        0.70711
10  0.70711 0       -0.70711 0
11  0       0.70711 0        -0.70711

Creating a full transform by applying it to set of basis states (zero amplitude terms left out):
  00 -> 00+01
  01 -> 00-01
  10 -> 10+11
  11 -> 10-11
Normalized and zero amplitude terms included:
  |00>  ->  0.70711|00> + 0.70711|01> + 0|10>       + 0|11>,  
  |01>  ->  0.70711|00> - 0.70711|01> + 0|10>       + 0|11>,  
  |10>  ->  0|00>       + 0|01>       + 0.70711|10> + 0.70711|11>,  
  |11>  ->  0|00>       + 0|01>       +0.70711|10>  - 0.70711|11>
Matrix Form (labeled):
      00      01       10      11
  00  0.70711 0.70711  0       0
  01  0.70711 -0.70711 0       0
  10  0       0        0.70711 0.70711
  11  0       0        0.70711 -0.70711
Transform Data Structure:
  [
  ["00", [[0.7071067811865476,"00"],[0.7071067811865476,"01"],[0,"10"],[0,"11"]]],
  ["01", [[0.7071067811865476,"00"],[-0.7071067811865476,"01"],[0,"10"],[0,"11"]]],
  ["10", [[0,"00"],[0,"01"],[0.7071067811865476,"10"],[0.7071067811865476,"11"]]],
  ["11", [[0,"00"],[0,"01"],[0.7071067811865476,"10"],[-0.7071067811865476,"11"]]]
  ]
Simplified:
  [
  ["00",[[0.7071067811865476,"00"],[0.7071067811865476,"01"]]],
  ["01",[[0.7071067811865476,"00"],[-0.7071067811865476,"01"]]],
  ["10",[[0.7071067811865476,"10"],[0.7071067811865476,"11"]]],
  ["11",[[0.7071067811865476,"10"],[-0.7071067811865476,"11"]]]
  ]
Interface:
  Zero out a 2 bit sytem manually or running QSystem={"states":[[1,"00"],[0,"01"],[0,"10"],[0,"11"]],"names":["a","b"]} in the code box.
  Put "Had(1);" into code box and instead of pressing "Calculate", press "Create New Transform"
  Press the "Copy" button above the new transform displayed in 2 forms above the code box.
  Paste it into the transform box at top of screen and press "Norm Trans" or "Simplify".

You can get the same thing by applying the same reordering operation to the matrix columns and rows:
      00      01       10       11
  00  0.70711 0        0.70711  0
  01  0       0.70711  0        0.70711
  10  0.70711 0        -0.70711 0
  11  0       0.70711  0        -0.70711
->  switching order of qbits "a" and "b"
      00      10       01       11
  00  0.70711 0        0.70711  0
  10  0       0.70711  0        0.70711
  01  0.70711 0        -0.70711 0
  11  0       0.70711  0        -0.70711
-> reordering matrix rows to reflect new bit order
      00      10       01       11
  00  0.70711 0        0.70711  0
  01  0.70711 0        -0.70711 0
  10  0       0.70711  0        0.70711
  11  0       0.70711  0        -0.70711
-> reordering matrix columns to reflect new bit order
      00      01       10       11
  00  0.70711 0.70711  0        0
  01  0.70711 -0.70711 0        0
  10  0       0        0.70711  0.70711
  11  0       0        0.70711  -0.70711


