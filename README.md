# StemCellSim

StemCellSim is a program designed to infer the population dynamics of mutated HSCs from lineage trees using ABC (Approximate Bayesian Computation). StemCellSim has the ability to simulate data trees from simulated clonal expansions with or without feedback, and to infer model parameters from real or simulated data trees. 

## Download

To download StemCellSim, type the following into the command prompt:

```
$ [insert command to download here] 
$ unzip StemCellSim.zip
```

After executing the unzip command, the resulting files are main.cpp, main2.cpp, main3.cpp, StemCellSim.h, StemCellSim.cpp, makefile, tree34cancerous.txt, and tree63cancerous.txt.

## Executing StemCellSim with example main files

The makefile included is an example makefile that combines main.cpp, StemCellSim.h, and StemCellSim.cpp into an executable. To instead use main2.cpp (or main3.cpp), one must edit the makefile by changing every occurence of main.cpp to main2.cpp (or main3.cpp).

main.cpp is used to produce 50 simulated data trees and stores each of them individually in: inputDataFile1.txt, inputDataFile2.txt, ... , inputDataFile50.txt. These data trees are constructed (and the output files then displayed) by executing the following commands:

```
$ make clean
$ make
$ ./sim.x
$ ls
```

main2.cpp is used to infer the model parameters from these data trees. To infer the model parameters, first open makefile in a text editor, and change every occurence of main.cpp to main2.cpp. Then run:

```
$ make clean
$ make
$ ./sim.x 1
$ ls
```
This will infer the model parameters from the first data tree, held in the file inputDataFile1.txt. The corresponding output parameter files which contain points sampled from the posterior distribution will have the number 1 attached to them. Note that "./sim.x 1" will enumerate the number of points collected by the posterior using cout statements. To terminate the "./sim.x 1" command early, type CTR c into the command prompt. The points kept by the posterior up until the point of termination will be found in the corresponding output files.

To infer the model parameters of the second data tree, redo the previous set of commands, but instead use "./sim.x 2" instead of "./sim.x 1". The corresponding output parameter files will have the number 2 attached to them. In general, to infer the model parameters from each data tree simultaneously, it is better to write a script that executes the following commands simultaneously:

```
$ ./sim.x 1
$ ./sim.x 2
     .
     .
     .
$ ./sim.x 50
```

Main3.cpp is used to infer the parameter values from real data. To infer the parameter values from real data (in this case, the real data is contained in tree34cancerous.txt, which is a tree in Newick format that corresponds to the ET2 patient), edit the makefile as before to instead use main3.cpp. The following commands run ABC on the tree and print the inferred parameter values to their corresponding text files.

$ make clean
$ make
$ ./sim.x 100
$ ls

The "./sim.x 100" command can also be terminated by typing CTR c into the command prompt. The corresponding output parameter files will have the number 100 attached to them, and will contain all the points kept for the posterior up until the point of termination.

For a detailed description of the StemCellSim implementation, see [...].