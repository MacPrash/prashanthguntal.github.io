---
title: "Program: C code on Geometric Series and Others"
date: 2019-07-10
tags: [C Code,Geometric Series,Programming]
header:
  image: "/images/PCAutomation.jpg"
excerpt: "C Code"
mathjax: "true"
---


*Question 1*

<img src="{{ site.url }}{{ site.baseurl }}/Users/prash/Code/prashanthguntal.github.io/images/ICHEC/Screenshot 2019-09-04 at 13.31.45.png" alt="Picture1">


```c

#include <stdio.h>

//Run the program as gcc -o Assignment1 Assignment1.c -lm
// ./Assignment1

// Below function returns the values by comsidering values of k, if k is even k/2 values is returned else if k is of odd 3k+1 is returned.

int f(int k){

if(k%2 == 0){   //checks if k is even

k = k/2; //replaces k with k/2 value

}
else{ //runs when k is odd

k = (3*k)+1; //replaces k value with 3k+1

}

return(k); //returns the new k value to the called function.

}

// Below function return the count of numbers in integer number.
int char_count_fun(int c)
{

int count_char_1 = 0; //Initialize the count value

while(c != 0) //Loop to calculate number of characters in int value
{

c = c/10; //Here we remove the last number and increment the count by 1.
++count_char_1; //Incrementing the count value

}

return(count_char_1 + 2); //returning count of numbers in Integer value + 2(because we are printing space and "," along with value )

}

int main(){ //Main function
int count = 0;
int char_count = 0; //Storing the number of characters printed on output.
int n = 101; //Inditialize the n value to 101
int len = 0; //Storing the number returned by above function which calculates number of characters in integer value
int check = 0;

//Condition to check if entered number is positive integer

if(n<1)
{

printf("Error : Please enter only the positive intergers \n");
return 0;

}

do
{

printf("%d, ",n); //Print the first value of n

//Condtion for Setting check point if n = 4 encountered.

if(n == 4)
{

check = 1;

}

n = f(n); //calling the function to return respective value if n is even or odd

len = char_count_fun(n); //storing the count of numbers in interger value
char_count = char_count + len; //Storing the number of characters printed on output screen.


if(char_count >=38) //Condition to ensure that printout doesnot extend more than 40 characters for each line
{
printf("\n"); //Printing new line if 40 characters condition is met.
char_count = 0; //And initializing the number of character printed to 0 on new line
}

// Condition to store and terminate the program if last 3 number are 4,2,1. After 4 next number is 2 and then 1 for any value of n.
if(n == 2 || n == 1){

if(check == 1)
{

++count; //Increment the count of any of these values are seen.

}

}

}while(count != 2); //Exit from loop when last 3 number 4,2,1, are found

printf("%d, \n",n); // printing the last number or 1 on the output screen

}



```



*Question 2*

<img src="{{ site.url }}{{ site.baseurl }}/Users/prash/Code/prashanthguntal.github.io/images/ICHEC/Screenshot 2019-09-04 at 13.31.53.png" alt="Picture2">

```c

#include <stdio.h>
#include <math.h> //Header for using pow function

//Function to calculate Sn(geometric series) values

float function(int n, float a, float r)
{
float Sn;
Sn = a*(1-pow(r,n+1))/(1-r); //Formula to calculate Sn
return(Sn); //Returning the geometric serie value for particular values
}

int main(){
float Sn;
int i;

for(i=1;i<=3;i++) //Loop for 3 different values of n,a,r
{

switch(i){ //Switch case to run particular case for particular values of n,a,r

	case 1:

		Sn = function(10000,2.0,0.01); //Calling the geometric function for n=10000, a=2.0, r=0.01

		printf("%f \n",Sn); //Printing Sn values
		break;
	case 2:

		Sn = function(500,0.01,1.1); //Calling the geometric function to return Sn value for n=500,a=0.01, r=1.1		

		printf("%f \n",Sn);
		break;
	case 3:

		Sn = function(100,0.0001,2.0);	//Calling the geometric function to return Sn value for n=100,a=0.0001,r=2.0
		printf("%f \n",Sn);
		break;
	default:
		printf("Invalid"); //if any of the 3 cases is satisfied.

}


}

}

// Output : As "n" and "a" value decreases the ouput values of Sn increases similaryl as "r" values increase the output values of Sn also increases.



```
