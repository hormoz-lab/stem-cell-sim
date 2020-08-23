# StemCellSim

StemCellSim is a program designed to infer the population dynamics of mutated HSCs from lineage trees using ABC (Approximate Bayesian Computation). StemCellSim has the ability to simulate data trees from simulated clonal expansions with or without feedback, and to infer model parameters from real or simulated data trees. 

## OS requirements
StemCellSim is supported for _Linux_ and has been tested on the following system:
     
     Linux: CentOS 7.7

## Installation guide

To install StemCellSim from gitlab, type the following into a linux command prompt:

```
$ git clone https://gitlab.com/hormozlab/stemcellsim.git
$ cd stemcellsim
$ unzip StemCellSim.zip
```

Running these commands should take a matter of seconds. 

After executing the unzip command, the resulting files should be main.cpp, main2.cpp, main3.cpp, StemCellSim.h, StemCellSim.cpp, makefile, ET1_tree.txt, and ET2_tree.txt.

## Demo: Executing StemCellSim with example main files

The makefile included is an example makefile that combines main.cpp, StemCellSim.h, and StemCellSim.cpp into an executable called ./sim.x. To instead use main2.cpp (or main3.cpp), one must edit the makefile by changing every occurence of main.cpp to main2.cpp (or main3.cpp).

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
$ ./sim.x 1 (type ctr c to terminate)
$ ls
```

This will infer the model parameters from the first data tree, held in the file inputDataFile1.txt. The corresponding output parameter files which contain points sampled from the posterior distribution will have the number 1 attached to them. To terminate the "./sim.x 1" command early, the user must type ctr c into the command prompt. The points kept by the posterior up until the point of termination will be found in the corresponding output files. Note that the "./sim.x 1" command will enumerate the number of points collected by the posterior using cout statements.

When the ls command is executed, the user should see the files nFile1,txt, sFile1.txt, gFile1.txt, LFile1.txt, NFile1.txt, as well as trajFile1.txt LTTFile1.txt. For the first five textfiles (nFile1.txt, ... , NFile1.txt), the data points are delimited by a ',', while in the last two text files (trajFile1.txt and LTTFile1.txt), the data points are delimited by a '\n'. The first data point of each text file corresponds to the first set of parameters kept by ABC, the second data point of each text file corresponds to the second set of parameters kept by ABC, etc.

To infer the model parameters of the second data tree, redo the previous set of commands, but instead type "./sim.x 2" instead of "./sim.x 1". The corresponding output parameter files will have the number 2 attached to them. In general, to infer the model parameters from each data tree simultaneously, it is better to write a script that runs the following commands simultaneously:

```
$ ./sim.x 1
$ ./sim.x 2
     .
     .
     .
$ ./sim.x 50
```

main3.cpp is used to infer the parameter values from real data. To infer the parameter values from real data (in this case, the real data is contained in ET1_tree.txt, which is a tree in Newick format that corresponds to the ET1 patient), edit the makefile as before to instead use main3.cpp. The following commands run ABC on the tree and print the inferred parameter values to their corresponding text files.

```
$ make clean
$ make
$ ./sim.x 100 (type ctr c to terminate)
$ ls
```

The corresponding output parameter files will have the number 100 attached to them, and will contain all the points kept for the posterior up until the point of termination.

## Reproducing ABC on patient data

The following should be done in a high-performance computing environment.

The files ET1_tree.txt and ET2_tree.txt contain the trees corresponding to the ET1 (34 year old) and ET2 (63 year old) patients respectively. To run ABC on the patient trees in the same way as in the manuscript, first open the makefile and make sure that every occurence of main.cpp is changed to main3.cpp. Then open main.cpp, and change line 102 to be:

```
sim.samplePosterior(0.0225, 0, outputFiles);
```

The change made above narrows the epsilon distance in ABC from 0.03 to 0.0225. After this, the user should run the following command to produce the executable ./sim.x:

```
$ make clean
$ make
```

The user should then write a script to execute the following commands in parallel:

```
$ ./sim.x 1
$ ./sim.x 2
     .
     .
     .
$ ./sim.x 400
```

In the above commands, we have run ABC 400 times on ET1 patient tree. The output files will ultimately be concatenated by writing a script to execute the following commands:

```
$ cat nFile1.txt nfile2.txt ... nFile400.txt > nFile.txt
$ cat sFile1.txt sfile2.txt ... sFile400.txt > sFile.txt
$ cat gFile1.txt gfile2.txt ... gFile400.txt > gFile.txt
$ cat LFile1.txt Lfile2.txt ... LFile400.txt > LFile.txt
$ cat NFile1.txt Nfile2.txt ... NFile400.txt > NFile.txt
$ cat trajFile1.txt trajfile2.txt ... trajFile400.txt > trajFile.txt 
```

The first data point of nFile.txt, sFile.txt, gFile.txt, LFile.txt, NFile.txt, and trajFile.txt corresponds to the first set of parameters kept by ABC, where the text files correspond to parameters n, s, g, L, N, and trajectories respectively. The second data point of the text files corresponds to the second set of parameters kept by ABC, etc.

To carry out ABC on the ET2 patient data, the user must then open main3.cpp and change line 102 to be

```
sim.samplePosterior(0.0125, 0, outputFiles);
```

The change made above narrows the epsilon distance in ABC. Afterwards, the user should change line 94 to be

```
sim.readDataAndStoreLTT("ET2_tree.txt", 63);
```

This change adjusts the code so that the ET2 patient data is instead read into StemCellSim. The ABC inference is then carried out in the exact same way as the ET1 patient data by producing the executable ./sim.x, running the inferences in parralel, and then concatenating the output files.

## StemCellSim instructions manual

For a detailed description of the StemCellSim program and implementation details, download [StemCellSim_manual.docx](StemCellSim_manual.docx)

