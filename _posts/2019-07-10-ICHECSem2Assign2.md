---
title: "Program: C code to find Determinant of 3*3 matrix and Area of Curve(Trapezoidal Rule)"
date: 2019-07-10
tags: [C Code,Determinant of matrix,Area of Curve(Trapezoidal Rule),Programming, Cramers Rule, Hilbert Matrix]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "C Code"
mathjax: "true"
---

Here is the code to calculate  3*3 matrix determinant(Normal Matrix, Hilbert Martix) and by using Cramers rule to calculate determinant of matrix.


Commands to execute programs:

* Please remove ".txt" extension from Makefile.
* Running Commands:
* Question 1 :
  gcc -o question1 question1.c -lm
  ./question1
* Question2: Just type make in the command line as show below.
  make
  "To Clean run" : make clean

*Question 1*

<img src="{{ site.url }}{{ site.baseurl }}/images/ICHEC/Picture3.jpg" alt="Picture3">

*Solution*



```


#include<stdio.h>
#include<stdlib.h>

// Creating a structure to access 3x3 matrix

struct pmclass{
float exam[3][3];

};

// Calculating the determinant of each of the 3x3 matrix

float det(struct pmclass mat[],int ln){

float det = 0;

det = mat[ln].exam[0][0]*((mat[ln].exam[1][1]*mat[ln].exam[2][2]) (mat[ln].exam[2][1]*mat[ln].exam[1][2])) -mat[ln].exam[0][1]*(mat[ln].exam[1][0]*mat[ln].exam[2][2] - mat[ln].exam[2][0]*mat[ln].exam[1][2]) + mat[ln].exam[0][2]*(mat[ln].exam[1][0]*mat[ln].exam[2][1] - mat[ln].exam[2][0]*mat[ln].exam[1][1]);

//printf("Determinant %e \n", det);


return(det);

}


int main(){

int row, col, i;


//Hilbert Matrix
float mat[4][4];


printf("Hilbert Matrix is : \n\n");

//Calculating the hilbert matrix values

for (row = 0; row<4;row++)
{

	for(col = 0; col<4; col++)
	{

		mat[row][col] = (float) 1.0/((row+1)+(col+1)-1.0);
		printf(" %f", mat[row][col]);
	}
	printf("\n");
}

printf("\n\n");


float mat[4][4] = {{1,0.5,0.33,0.25},{0.5,0.33,0.25,0.2},{0.33,0.25,0.2,0.16},{0.25,0.2,0.16,0.14}};

//Creating the structure pointer variables

struct pmclass *pmc;

// Allocating the memory, here we are mulitplying by 4 because we are going to get four 3x3 matrices.
pmc = (struct pmclass*) malloc(4 * sizeof(struct pmclass));

// sign variable to store the sign as we need alternating signs to calculate the determinant.

int sign = 1;
float ans = 0;



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

     pmc[i].exam[j][k] = mat[row][col];

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

// mat[0][i] for obtaining the particular values of first row of hilbert matrix and det() call the determinant to calculate determinant of 3x3 matrix.

ans += sign * mat[0][i] * det(pmc, i);

// While calculating the determinant as we need alternating signs, so we changing the sign to -sign.
sign = -sign;

}

// Freeing the memory allocated to the pointer struct.
free(pmc);
// Printing the determinant of Hilbert 4x4 matrix.
printf("Determinant of above Hilbert matrix = %e \n\n", ans);

}

```
