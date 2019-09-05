---
title: "Program: High Performance Computing in C using OpenMP"
date: 2019-07-10
tags: [C,OpenMP,Programming]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "C Code, OpenMP"
mathjax: "true"
---


Complete report and how to execute each code is available [here](https://github.com/MacPrash/ICHEC_Assignment_2_sem2/blob/master/Report.pdf)


*Question 1:*

*Matrix-matrix multiplication*
Based on the matrix-matrix multiplication code listed below, mma.c or mma.f90 in Section 5 carry out
the following steps:
1. Parallelize the most computational intensive loop using OpenMP directives.
2. Measure the time spent in this loop as a function of the numbers of threads.
3. Change the size of the matrix N to 500, 5000 and redo the previous steps.
4. Make a graph for absolute and relative speedup and efficiency for all the values of N.


<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture17.jpg" alt="Picture17">

*Solution:*

``````

#include <stdio.h>
#include <time.h>
#include <omp.h>
#define nd 512
int main( void )
{
const int n = nd;
int i, j, k;
static double *a, *b, *c;
double dj, di;

//Allocating the memory for each matrix
a = (double *)malloc(n*n*sizeof(double));
b = (double *)malloc(n*n*sizeof(double));
c = (double *)malloc(n*n*sizeof(double));

/*
 * * initialize the a and b matrices with nonzero values;
 * * initialize c with zeros (it will accumulate results)
 * */
for ( j = 0; j < n; j++ ) {
	dj = (double)( j + 1 );
	for ( i = 0; i < n; i++ ) {
		di = (double)( i + 1 );
		*(a+i*n+j) = 1.e-3 * di + 1.e-6 * dj;
		*(b+i*n+j) = 1.e-3 * (di + dj) + 1.e-6 * (di - dj);
		*(c+i*n+j) = 0.e0;
	}
}

double t;

//Getting the current time
t = omp_get_wtime();

//Adding the required number of threads
omp_set_num_threads(10);


//Parallelizing the most complex loop with i,j,k as private for each thread and shared variables a,b,c matrices
#pragma omp parallel for schedule(guided,1) private(i,j,k) shared(a,b,c)
for ( i = 0; i < n; i++ )
	for ( j = 0; j < n; j++ )
		for ( k = 0; k < n; k++ )
			*(c+i*n+j) = *(c+i*n+j) + *(a+i*n+k) * (*(b+k*n+j));

//Getting the current time and calculating the time taken to execute
t = omp_get_wtime() - t;
printf("elapsed time = %f seconds.\n", t);

/*
 * * print out some sample points
 * */
printf( "%20.16f %20.16f %20.16f\n",  *(c+((n/2) -1)*n+0),
*(c+((n/2)-1)*n+((n/2) -1)), *(c+((n/2) -1)*n+(n-1)));

//Freeing the memory
free(a);
free(b);
free(c);

}


```


*Question 2:*

*Compute the Area under a Curve*
One possible way to compute the area of under a curve is to use the Trapezoidal Rule, see the following site for more details: (http://mathworld.wolfram.com/TrapezoidalRule.html):

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture16.jpg" alt="Picture16">

1. Write a serial program to compute the area under the function f (x) = sin(x)+0.5∗x by using the
Trapezoidal Rule above. For this exercise set N=134217728, and compute the area for 0 ≤ x ≤ 9.
2. Parallelize the serial code by using OpenMP directives.
3. Make a graph of speedup and efficiency by using 1, 2 and 4 threads


``````

#include<stdio.h>
#include<math.h>
#include<omp.h>

//function that return the sin(x)+(0.5*x) to calling function

double f(double x){

return sin(x)+(0.5*x);

}


void main(void){

long int N = 134217728;

//storing the initial intergram value
int a=0.0;
////Storing the final integral value
int b = 9.0;
//
float h = 0,y,ans;
//// For storing the final value
double sum = 0;
//
int i;
//
//// Storing the sum of values of function of initial and final integral.
double initial = 0;
double func = 0;
h = (b - a)/(float)N;
////Calculating the value of all the numbers between inital and final integral.

double t;

//Getting the current time
t = omp_get_wtime();


//Adding the required number of threads
omp_set_num_threads(1);
//
//Adding parallelization with N as  shared variable and (i,func,initial) as private for each thread and reduction directive to sum variable
#pragma omp parallel for shared(N) private(i,func,initial) reduction(+:sum)
for(i=1;i<=N;i++)
{
//calling the function to calculate sin(x)+0.5*x
func = f(i*h);
//Calculating the sum
sum = sum + func + initial;

//Changing the initial value to func value
initial = func;

}

//Getting the current time and finding the time taken for execution
t = omp_get_wtime()-t;

//// Final value.
ans = (h/2)*(sum);
printf("Trapezoidal Valuev %20.16f \n",ans);
printf("Time Taken to execute %f\n", t);

}


```

*Question 3:*

*Computing π by using Monte Carlo Methods*
The monte monte pi.c and ran2.c code in Section 5 will compute π by using a Monte Carlo method. Carry out the following parallelisation steps:
1. Parallelize it by using an OpenMP syncronous directive.
2. Make the function ran2 thread safe.
3. By changing the number of threads the result changes, why?
4. Parallelize the code in another way using different directives.

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture18.jpg" alt="Picture18">

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture19.jpg" alt="Picture19">

*Solution*

*ran2.h*

``````

#define IM1 2147483563
#define IM2 2147483399
#define AM (1.0/IM1)
#define IMM1 (IM1-1)
#define IA1 40014
#define IA2 40692
#define IQ1 53668
#define IQ2 52774
#define IR1 12211
#define IR2 3791
#define NTAB 32
#define NDIV (1+IMM1/NTAB)
#define EPS 1.2e-7
#define RNMX (1.0-EPS)
#include<omp.h>
#include<stdio.h>
#include<stdlib.h>
float ran2(long *idum)
{

static int j;
static long k;
static long idum2=123456789;
static long iy=0;
static long iv[NTAB];
static float temp;

//Adding the threadprivate so that variables are private for each thread and no other parallel thread can access them
#pragma omp threadprivate(k,temp,iy,iv,idum2)

if (*idum <= 0) {
if (-(*idum) < 1) *idum=1;
else *idum = -(*idum);
idum2=(*idum);
for (j=NTAB+7;j>=0;j--) {
k=(*idum)/IQ1;
*idum=IA1*(*idum-k*IQ1)-k*IR1;
if (*idum < 0) *idum += IM1;
if (j < NTAB) iv[j] = *idum;
}
iy=iv[0];
}

k=(*idum)/IQ1;
*idum=IA1*(*idum-k*IQ1)-k*IR1;
if (*idum < 0) *idum += IM1;
k=idum2/IQ2;
idum2=IA2*(idum2-k*IQ2)-k*IR2;
if (idum2 < 0)
{
 idum2 += IM2;
}
j=iy/NDIV;
iy=iv[j]-idum2;
iv[j] = *idum;
if (iy < 1) iy += IMM1;


if ((temp=AM*iy) > RNMX)
{
return RNMX;
}
else{
return temp;
}

}

```

*monte_pi.c*

``````
#include <stdlib.h>
#include <stdio.h>
#include "ran2.h"
#include<omp.h>
int main(int argc, char **argv){
int niter, i;
static long seed;
double count;
static double x,y,z;
float pi;
extern float ran2();
niter=10000000;
count=0;
int tid;

omp_lock_t lock;
omp_init_lock(&lock);

double t;


//getting the current time
t = omp_get_wtime();

//Setting the number of threads
//omp_set_num_threads(8);


// Adding parallel for the loop
#pragma omp parallel for private(i,seed,x,y,z) //reduction(+:count)

for(i=1;i<=niter;i++){

seed = i;
x=ran2(&seed);
y=ran2(&seed);
z=x*x+y*y;

//omp_set_lock(&lock);
if(z<1){

#pragma omp atomic
count+=1;

}
}

//Fiding the current and calculating the time taken to execution
t = omp_get_wtime()-t;


printf("Time is = %f\n",t);
pi=count*4.0/niter;
printf("The value of pi is %8.14f\n",pi);
return 0;
}


```
