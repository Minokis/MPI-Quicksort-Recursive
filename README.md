# MPI-Quicksort-Recursive

The goal:
Realization of quicksort algorithm with means of MPI library.

Input data:
Text file with int numbers, separated with space.

NB:
This algorithm is not very effective. The code is written mostly in educational purposes.
If you are interested in a better realization of parallel quicksort, take a look at MPI-Quicksort-Hypercubes project.

A simplified scheme of a program:

1. The array of numbers is divided into parts. Parts are sent to different processes. 
2. When each process gets its part, it sort its part sequentially with usual quicksort algorithm.
3. All the parts are sent back to the sending process. Eventually they are all gathered by the master process (here it is 0).
4. Master process writes the result into file.

How partition is performed:

1. Each process divides an array with Lomuto quicksort algorithm. This way a pivot receives its final position and won't move until the end of sorting. On the left there will be numbers less or equal to pivot, on the right - numbers which are greater.
More on different quicksort algorithms can be found on Wikipedia (look for article in English).

2. The smallest of two parts is sent to available process. If there is one more available process, the array is divided again, otherwise it is sequentially sorted with Hoare quicksort.

3. The sorted part is sent back to the source process.

Why smallest part? :
I believe it helps to reduce overhead a little.

How available process is found?:
Rank of available = current rank + 2^n,
where n satisfies 2^(n-1) <= current rank < 2^n

Why so difficult?
Division and sending is done mostly by first (half of) processes. Others are receiving, sorting and sending back.
If you have a lot of processes, 0 process sends parts to 1, 2, 4 and 8 process (if such exist). The 1 process sends to 3, 5 9 and so on. 

Credit:
I used ideas of PrasadPerera from here: https://www.codeproject.com/Articles/42311/Parallel-Quicksort-using-MPI-Performance-Analysis 
If you don't understand what is going on, try to read his article, it explains better.

How to configure your Visual Studio to use MPI:
There is a good and simple article here: https://blogs.technet.microsoft.com/windowshpc/2015/02/02/how-to-compile-and-run-a-simple-ms-mpi-program/



