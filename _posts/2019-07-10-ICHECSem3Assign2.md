---
title: "Program: High Performance Computing in C using MPI"
date: 2019-07-10
tags: [C,MPI,Programming]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "C Code, MPI"
mathjax: "true"
---

*Question 1*

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture11.jpg" alt="Picture11">

*Solution: 1a*

``````


#include<stdio.h>
#include<mpi.h>

int main(int argc, char **argv)
{
  // defining the rank
  int Rank, ierror;

  // Defining number of process, left procees id and right process id
  int number_of_process, left_process_id, right_process_id;

  //defining sum to store to total sum value, tmp to store left side of current process sum value, val to store rank and check until myRank != myRank  
  int sum,tmp, val;

  //initilaizing status messages
  MPI_Status rcv_stat, snd_stat;

  //initializing request
  MPI_Request send_request;

  //Initializing MPI environment
  ierror = MPI_Init(&argc, &argv);

  //Using rank we identify different process within the communicator
  ierror = MPI_Comm_rank(MPI_COMM_WORLD, &Rank);

  //Number of processes contained within a communicator
  ierror = MPI_Comm_size(MPI_COMM_WORLD, &number_of_process);

  //left process id is previous process id in chain
  left_process_id=(Rank-1);
  // right process id is next process id in chain
  right_process_id=(Rank+1);

  //Checking is left process id less than zero then assgin number of process - 1 as this will be the previous process
  if (left_process_id < 0)
  {
   left_process_id = number_of_process-1;
  }

  //Similarly if right process id equal to number of process then assigning zero process id as right process to number of process - 1 process.
  if (right_process_id == number_of_process)
  {
   right_process_id = 0;
  }
  //Assign the process rank to val and initialize sum to 0
  val = Rank;
  sum = 0;
   // Non-blocking Synchronous Send
   MPI_Issend(&val,1,MPI_INT,right_process_id,99,MPI_COMM_WORLD,&send_request);
   // Receive
   MPI_Recv(&tmp,1,MPI_INT,left_process_id,99,MPI_COMM_WORLD,&rcv_stat);

   MPI_Wait(&send_request,&snd_stat);
   //Assigning the received value to tmp variable
   //Adding the received process number value from tmp to sum
   val = tmp;
   sum += val;
  int count = 0;
  while(val != Rank) {
    // Non-blocking Synchronous Send
    MPI_Issend(&val,1,MPI_INT,right_process_id,100,MPI_COMM_WORLD,&send_request);
    // Receive
    MPI_Recv(&tmp,1,MPI_INT,left_process_id,100,MPI_COMM_WORLD,&rcv_stat);
    MPI_Wait(&send_request,&snd_stat);
    //Assigning the received value to tmp variable
    val = tmp;
    //Adding the received process number value from tmp to sum
    sum += val;
  }

  printf("Process id:  %d \t Sum : %d \n", Rank, sum);
  //Terminates MPI execution environment
  ierror = MPI_Finalize();

  return 0;
}

```


*Solution 1b:*

``````

#include<stdio.h>
#include<mpi.h>

int main(int argc, char **argv)
{

    // Defining the rank, number of process
    int myRank, ierror, number_of_process;
    // Initializing sum to 0
    int sum = 0;
    //initilaizing status messages
    MPI_Status rcv_stat, snd_stat;
    //initializing request
    MPI_Request send_request;
    //Initializing MPI Environment
    ierror = MPI_Init(&argc, &argv);
    //Using rank we identify different process within the communicator
    ierror = MPI_Comm_rank(MPI_COMM_WORLD, &myRank);
    //Number of processes contained within a communicator
    ierror = MPI_Comm_size(MPI_COMM_WORLD, &number_of_process);
    int val = 0;
    //Loop to calculate sum value
    while(val < number_of_process)
    {
        //MPI_Ireduce Reduces values on all processes to a single value in a nonblocking way
        MPI_Ireduce(&myRank, &sum, 1, MPI_INT, MPI_SUM, val, MPI_COMM_WORLD, &send_request);
        MPI_Wait(&send_request, &rcv_stat);
        val = val + 1;
    }

        printf("Using IReduce \t Processor: %d \t Sum: %d\n", myRank,sum);
        //Terminates MPI execution environment
        ierror = MPI_Finalize();

        return 0;
}

```

*Question 2*

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture12.jpg" alt="Picture12">

*Solution*

``````

#include<stdio.h>
#include<stdlib.h>
#include<mpi.h>
#define N 5

        // Creating a structure to access 3x3 matrix
        struct pmclass{
        float exam[3][3];
        };

        // Calculating the determinant of each of the 3x3 matrix
        float det(struct pmclass mat[],int ln){
        float det = 0;
        det = mat[ln].exam[0][0]*((mat[ln].exam[1][1]*mat[ln].exam[2][2]) - (mat[ln].exam[2][1]*mat[ln].exam[1][2])) -mat[ln].exam[0][1]*(mat[ln].exam[1][0]*mat[ln].exam[2][2] - mat[ln].exam[2][0]*mat[ln].exam[1][2]) + mat[ln].exam[0][2]*(mat[ln].exam[1][0]*mat[ln].exam[2][1] - mat[ln].exam[2][0]*mat[ln].exam[1][1]);
        return(det);
        }

        // Function to obtain required 4 3*3 matrixs and returing the determinant of four 3*3 matrix
        double submatrix(double temp[4][4])
        {
        int row, col, i;
        //Creating the structure pointer variables
        struct pmclass *pmc;
        // Allocating the memory, here we are mulitplying by 4 because we are going to get four 3x3 matrices.
        pmc = (struct pmclass*) malloc(4 * sizeof(struct pmclass));
        // sign variable to store the sign as we need alternating signs to calculate the determinant.
        int sign = 1;
        double ans = 0;
        for(i = 0; i<4;i++) //For each of the struct we are creating 3x3 matrix.
        {
                int k =0, j=0;
           for(row = 0; row<4; row++) //row and col to obtain the particular values from hilbert matrix.
           {
                for(col = 0; col<4; col++)
                {
                        if(row != 0 && col != i) //Here we are ignoring the first row and particular column to for each 3x3 matrix.
                        {
                                 // Storing the hilbert matrix values into our struct matrix. Here pmc[i] represents different 3x3 matrix
                                pmc[i].exam[j][k] = temp[row][col];
                                // Incrementing the column value of the struct 3x3 matrix.
                                        k = k + 1;
                                // Checking if 3rd column is value is stored in struct 3x3 matrix
                                   if(k == 3)
                               {
                                // Once the 3rd column value is extracted we are moving to next row and making the column to zero.
                                                k = 0;
                                                j = j + 1;
                               }
                            }
                }
                }
// temp[0][i] for obtaining the particular values of first row of hilbert matrix and det() call the determinant to calculate determinant of 3x3 matrix.
                ans += sign * temp[0][i] * det(pmc, i);
                // While calculating the determinant as we need alternating signs, so we changing the sign to -sign.
                sign = -sign;
        }
// Freeing the memory allocated to the pointer struct.
        free(pmc);
        //returning the determinant of four 3 * 3 matrices
        return ans;
        }

int main(int argc, char **argv){
        //Used tp store determinant of each processor
        double D = 0.0;
        // sign variable to store the sign as we need alternating signs to calculate the determinant.
        int sign = 1;
        // Processor id and number of processor
        int rank, nproc, ierror;
        //address of receive buffer
        double buff;

        //initializing status message
        MPI_Status status;

        //initializing MPI environment
        ierror = MPI_Init(&argc, &argv);
        //Using rank we identify different process within the communicator
        ierror = MPI_Comm_size(MPI_COMM_WORLD, &nproc);
        // Number of processor contained in the communicator
        ierror = MPI_Comm_rank(MPI_COMM_WORLD, &rank);

        //Final determinant value
        double Det = 0.0;
        printf("\n\n");
        // Initilazing the matrix
        double mat[5][5] = {{1.0, -1.0/2.0, -1.0/3.0, -1.0/4.0, -1.0/5.0},
                {-1.0/2.0, 1.0/3.0, -1.0/4.0, -1.0/5.0, -1.0/6.0},
                {-1.0/3.0, -1.0/4.0, 1.0/5.0, -1.0/6.0, -1.0/7.0},
                {-1.0/4.0, -1.0/5.0, -1.0/6.0, 1.0/7.0, -1.0/8.0},
                {-1.0/5.0, -1.0/6.0, -1.0/7.0, -1.0/8.0, 1.0/9.0}
        };

                        // Matrix row and column
                        int row, col;
                        // Matrix with size 4*4
                        double temp[4][4];
                        int k =0, j=0;
                   for(row = 0; row<5; row++) //row and col to obtain the particular values from hilbert matrix.
                          {
                        for(col = 0; col<5; col++)
                                {
                                        if(row != 0 && col != rank) //Here we are ignoring the first row and particular column to for each 4x4 matrix.
                                        {
                                        // Storing the required hilbert matrix values into our 4*4 size matrix
                                        temp[j][k] = mat[row][col];
                                        // Incrementing the column value of the struct 4x4 matrix.
                                                k = k + 1;
                                        // Checking if 4th column is value is stored in struct 4x4 matrix
                                               if(k == 4)
                                                    {
                                        // Once the 4th column value is extracted we are moving to next row and making the column to zero.
                                                        k = 0;
                                                        j = j + 1;
                                                }
                                        }
                                }
                                 }
                        //Assigning respective sign to respective position in the matrix
                        if(rank%2 == 0)
                                sign = 1;
                        else
                                sign = -1;
                        //Calculating the determinant each 4*4 matrix.
                        D = sign * mat[0][rank] * submatrix(temp);
                        printf("D: %lf\tProc:%d\n", D, rank);

                        // Adding all the determinant values of each processor along with the determinant returned by processor 0
                        if (rank == 0)
                        {
                                Det = Det + D;
                        }
                        // Condition to send determinant of all processor to rank 0 processor
                        if(rank!=0)
                                MPI_Send(&D,1,MPI_DOUBLE,0,1,MPI_COMM_WORLD);
                        else
                         {
                                for(int j=1;j<nproc;j=j+1){
                                MPI_Recv(&buff,1,MPI_DOUBLE,MPI_ANY_SOURCE,1,MPI_COMM_WORLD,&status);
                                Det = Det + buff;
                                }
                        }

        //print the result on rank 0
        if(rank == 0)
        {
                printf("\nDeterminant of the matrix is : %20.16f\n", Det);
                printf("\nActual Result: %20.16f\n\n", -2678797333.0/88905600000.0);
        }
        // Terminanting the MPI environment
        MPI_Finalize();
        return 0;
}
```

*Question 3:

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture13.jpg" alt="Picture13">

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture14.jpg" alt="Picture14">
<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture15.jpg" alt="Picture15">



*Solution:*

``````

#include<mpi.h>
#include<stdio.h>
#include<stdlib.h>

FILE *fr;
int main(int argc, char **argv){
double time1, time2;
double last;
int i, ierr, rank, nprocs;
long elapsed_seconds;
int indata[25];
char buf[256];
ierr = MPI_Init(&argc,&argv);
time1 = MPI_Wtime();
ierr = MPI_Comm_rank(MPI_COMM_WORLD, &rank);
ierr = MPI_Comm_size(MPI_COMM_WORLD, &nprocs);
if (rank == 0 ) {
/* open the file for reading */
fr = fopen ("values.dat", "rt");
/* fill indata */
i = 0;
while (!feof(fr)) {
fscanf(fr, "%d", &indata[i]);
i = i + 1;
}
// close the file */
fclose(fr);
}
ierr = MPI_Bcast( indata, 25, MPI_INT, 0, MPI_COMM_WORLD);

// output process rank 0 in file output.0
// // output process rank 1 in file output.1
// // output process rank 2 in file output.2
// // and so on ...
snprintf(buf, sizeof(buf), "output.%d", rank);
// /* open the file for reading */
fr = fopen (buf, "wt");
// /* fill indata */
i = 0;
while ( i<25 ) {
fprintf(fr, "%d\n", indata[i]);
i = i + 1;
}
// * close the file */
fclose(fr);
time2 = MPI_Wtime();
ierr = MPI_Finalize();
printf("Completed Successfully with %f seconds \n",time2-time1);
return 0;
}


```
