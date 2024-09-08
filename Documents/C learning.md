
```c
#include <stdio.h>

int main(void)
{
  int grades[5];
  printf("grades[5] = %d\n", grades[5]);
  printf("grades[500] = %d\n", grades[500]);
  return 0;
}
```

and those values inside grades scope will be 0
grades[500] out of space but it will given a random value, as it goes to other memories


initializing array:
```c
int grade[5] = {1,2,3,4,5};
```



Mutlidimensional array:
```
#include <stdio.h>
int main(void){
	int grades[2][2] = {1,2,3,4}; # 1,2// 3,4 separa
}

```
otuput: grades[0][0]=1 grades[0][1]=2 grades[1][0]=3 grades[1][1]=4



### command line arguments
1. int argc - # of arguments passed + 1 (this 1 means counts the program name)
2. char * argv[] - name of the program + arguments passed
```c
// if no arguments been given; # args = 1 argv[0] = ./main
```
to make "some cats here" as one argument you should put with ""

example of arguments been passed:
./simple_sequence -d 5 10 4 "some cats here" 
this will give 6 # of args 

```c
//final example 
int main(int argc, char *argv[]){
	int i;
	printf("argc =%d\n',argc");
	for (i = 0; i < argc; i++){
		printf("argv[%d] = %s\n", argv[i]);
	}
}
```




## Structure
```c
include <stdio.h>
int main(void){
	// create
	struct Point3D {
		int x;
		int y;
		int z;
	};
	// init 1
	struct Point3D p1 {
		p1.x = 0;
		p2.y = 0;
		p3.z = 0;
	}
	// init 2
	struct Point3D p2 = {.x = 2, .y = 3, .z = 4};

	// typedef  - this can avoid recalling struct in each init
	typedef struct Point1D {
		int x;
	}

	Point1D p { 
		p.x = 1;
	}
	
}
```





# Quiz - Matrix multiplication
```c
Matrix matrixMult(Matrix A, Matrix B)
{
  // Write your code here...
  // You may change the return statement 
  for (int ia =0 ; i < A.nrows ; ia++){
    for (int ja =0 ; j < A.ncols ; ja++){
	    
    }
  
  }
  
  return A;
  
}

int main(void)
{
  Matrix A = { {1.2, 2.3,
                3.4, 4.5,
                5.6, 6.7},
               3,
               2};
  Matrix B = { {5.5, 6.6, 7.7,
                1.2, 2.1, 3.3},
               2,
               3}; 
  
  Matrix C = matrixMult(A, B);
  printMatrix(C);

  return 0;
}
```