---
tags:
- C
- Math
date: 2022-03-28
title: Numerical Analysis - Lab
categories:
lastMod: 2025-10-18
--- 
## Index

*   Lab2 - Direct Methods for Solving Linear Equations
*   Lab3 - Inverse Power Method to Solve Eigenvalue
*   Lab4 - Jacobi Method to Solve Eigenvalue
*   Lab 5 - Cubic Spline Interpolation
*   Lab 6 - FFT & IFFT]
*   Lab 7 - Romberg Integral
  
## Lab2 - Direct Methods for Solving Linear Equations
  
### Description

Use C/C++, implement the following algorithms:

1.  Gaussian Elimination
2.  Doolittle Decomposition
  
### Implement
  
#### Predefinition and Useful functions

```cpp
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <iostream>
#include <fstream>
#include <math.h>
using namespace std;

#define SUCCESS 0
#define FAILURE 1
#define INPUTPATH "./InputData.txt"

typedef int Status;
typedef double* Vector;
typedef double** Matrix;

Status PrintMatrix(int n, Matrix A){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PrintMatrix
   *  Description:  Print the Matrix A to the terminal. n is the Dimension of A. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++){
      for(int j = 0; j < n; j++){
          printf("%.2lf\t", A[i][j]);
      }
      printf("\n");
  }
  return SUCCESS;
}

Status PrintVector(int n, Vector v){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PrintVector
   *  Description:  Print the Vector v to the terminal(in row form). n is the Dimension of v. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++)
      printf("%.2lf\t", v[i]);
  printf("\n");
  return SUCCESS;
}
```
  
#### Input

Using a file to input is the most stable way. Two key functions here: [_getline_](https://www.cplusplus.com/reference/string/string/getline/) and [_strtod_](http://www.cplusplus.com/reference/cstdlib/strtod/).

```cpp
Status ReadFromInput(int& n, Matrix& A, Vector& b, bool PrintResult){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  ReadFromInput
   *  Description:  Read data from input file. File Format:
   *                  The first line indicates n.
   *                  The next n line is the Matrix A, separated by space.
   *                  The last line is the vector b, separated by space.
   *                  
   *                  It's recommended that the number are written in demical.
   *                      e.g.  Use "1.0", not "1".
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  ifstream inputfile;
  inputfile.open(INPUTPATH);
  string str;
  char* endptr;
  if(inputfile.is_open()){
      getline(inputfile, str);    //Reading n.
      n = stoi(str);

      A = new Vector[n];
      for(int i = 0; i < n; i++){
          A[i] = new double[n];
      }
      b = new double[n];

      for(int i = 0; i < n; i++){ // Reading A.
          getline(inputfile, str);
          char SameAsStr[str.length() + 1];   //* Convert str to char* type for 
          strcpy(SameAsStr, str.c_str());     //*   the strtod function.
          for(int j = 0; j < n; j++){
              A[i][j] = strtod(SameAsStr, &endptr);
              strcpy(SameAsStr, endptr);
          }
      }

      getline(inputfile, str);    // Reading b.
      char SameAsStr[str.length() + 1];   //* Convert str to char* type for 
      strcpy(SameAsStr, str.c_str());     //*  the strtod function.
      for(int j = 0; j < n; j++){
          b[j] = strtod(SameAsStr, &endptr);
          strcpy(SameAsStr, endptr);
      }


      inputfile.close();

      if(PrintResult){
          printf("Matrix A:\n");
          PrintMatrix(n, A);
          printf("Vector b:\n");
          PrintVector(n, b);
          printf("\n");
      }
  }
  else{
      printf("Input Error, please check.\n");
      return FAILURE;
  }
  return SUCCESS;
}
```
  
#### Solving Triangular-Form Equations

LTM Solve Algorithm and UTM Solve Algorithm is symmetric. So we just call _UpperSolve_ function when implementing _LowerSolve_.

```cpp
Status UpperSolve(int n, Matrix A, Vector b, Vector x){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  UpperSolve
   *  Description:  When A is an Upper Triangular Matrix, use elimination, solve Ax == b. 
   *    Parameter:  n is D[A]. x is the solution.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */

  for(int i = 0; i < n; i++){
      for(int j = 0; j < i; j++){
          if(abs(A[i][j]) > 0.000001){
              printf("This Matrix is not a Upper Triangular Matrix.\n");
              return FAILURE;
          }
      }
  }
  for(int i = n - 1; i > -1; i--){
      if(A[i][i] == 0){
          printf("This entry have infinity solutions.\n");
          return FAILURE;
      }
      double InPathe = b[i]; 
      for(int j = n - 1; j > i; j--){
          InPathe -= A[i][j] * x[j];
      }
      x[i] = InPathe / A[i][i];
  }
  return SUCCESS;
}

Status LowerSolve(int n, Matrix A, Vector b, Vector x){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  LowerSolve
   *  Description:   When A is an Lower Triangular Matrix, use elimination, solve Ax == b. 
   *    Parameter:  n is D[A]. x is the solution.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
      Matrix TempA = new Vector[n];
      for(int i = 0; i < n; i++){
          TempA[i] = new double[n];
      }
      Vector Tempb = new double[n];
      Vector Tempx = new double[n];
      for(int i = 0; i < n; i++){
          Tempb[i] = b[n - 1 - i];
          for(int j = 0; j < n; j++)
              TempA[i][j] = A[n - 1 - i][n - 1 - j];
      }

      if(UpperSolve(n, TempA, Tempb, Tempx)){
          printf("This Matrix is not a Lower Triangular Matrix.\n");
          return FAILURE;
      }

      for(int i = 0; i < n; i++)
          x[i] = Tempx[n - 1 - i];
  return SUCCESS;
}
```
  
#### Gaussian Method

```cpp
Status ChoosePivot(int n, Matrix A, Vector b, int ProcessingColumn){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  ChoosePivot
   *  Description:  Choose the Pivot of Processing Column, do the exchange. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  double MaxAbs = abs(A[ProcessingColumn][ProcessingColumn]);
  int MaxAbsRow = ProcessingColumn;
  for(int i = ProcessingColumn + 1; i < n; i++){
      if(abs(A[i][ProcessingColumn]) > MaxAbs){
          MaxAbs = abs(A[i][ProcessingColumn]);
          MaxAbsRow = i;
      }
  }

  double temp = 0;
  for(int j = 0; j < n; j++){
      temp = A[ProcessingColumn][j]; A[ProcessingColumn][j] = A[MaxAbsRow][j]; A[MaxAbsRow][j] = temp;
  }
  temp = b[ProcessingColumn]; b[ProcessingColumn] = b[MaxAbsRow]; b[MaxAbsRow] = temp;
  return SUCCESS;
}

Status GaussMethod(int n, Matrix A, Vector b, Vector x){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  GaussMethod
   *  Description:   
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int k = 0; k < n; k++){
      ChoosePivot(n, A, b, k);

// {{< logseq/mark >}}{{< / logseq/mark >}} set the condition to non-zero to display eliminating process. {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}==
      if(1){ 
          printf("ChoosePivot #%d\n", k);
          printf("Matrix A:\n");
          PrintMatrix(n, A);
          printf("Vector b:\n");
          PrintVector(n, b);
          printf("\n");
      }
// {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}

      if(abs(A[k][k]) < 0.0000000001){
          printf("This pivot can't be den.\n");
          return FAILURE;
      }
      for(int i = k + 1; i < n; i++){
          double Coefficient = A[i][k] / A[k][k];
          for(int j = k; j < n; j++)
              A[i][j] -= Coefficient * A[k][j];
          b[i] -= Coefficient * b[k];
      }
  }

  printf("Matrix After Elimination:\n");
  PrintMatrix(n, A);
  printf("Vector After Elimination:\n");
  PrintVector(n, b);

  UpperSolve(n, A, b, x);
  printf("The solution is:\n");
  PrintVector(n, x);
  return SUCCESS;
}
```
  
#### LU Decomposition and Doolittle Method

```cpp
Status LUDecomposition(int n, Matrix A, Matrix L, Matrix U){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  LUDecomposition
   *  Description:  Do Doolittle decompose: A = LU, return via param L and U. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++){
      for(int j = 0; j < i + 1; j++){
          L[i][j] = A[i][j];
      }
      for(int j = i + 1; j < n; j++)
          L[i][j] = 0;
  }
  for(int i = 0; i < n; i++){
      for(int j = 0; j < i; j++)
          U[i][j] = 0;
      for(int j = i; j< n; j++)
          U[i][j] = A[i][j];
  }
  
  for(int k = 0; k < n; k++){
      for(int j = k; j < n; j++){
          for(int r = 0; r < k; r++)
              U[k][j] -= L[k][r] * U[r][j];
      }
      for(int i = k; i < n; i++){
          for(int r = 0; r < k; r++)
              L[i][k] -= L[i][r] * U[r][k];
          if(U[k][k] == 0){
              printf("Divided by 0.\n");
              return FAILURE;
          }    
          L[i][k] /= U[k][k];
      }
  }
  return SUCCESS;
}

Status DoolittleMethod(int n, Matrix A, Vector b, Vector x){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  DoolittleMethod
   *  Description:  Using DoolittleMethod to Solve Ax == b,
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  Matrix L = new Vector[n];
  for(int i = 0; i < n; i++){
      L[i] = new double[n];
  }
  Matrix U = new Vector[n];
  for(int i = 0; i < n; i++){
      U[i] = new double[n];
  }
  Vector y = new double[n];

  LUDecomposition(n, A, L, U);
  printf("L:\n");
  PrintMatrix(n, L);
  printf("U:\n");
  PrintMatrix(n, U);
  LowerSolve(n, L, b, y);
  UpperSolve(n, U, y, x);
  printf("\n");
  PrintVector(n, x);
  return SUCCESS;
}
```
  
#### Calculate Error

```cpp
Status CalculateError(int n, Matrix A, Vector x, Vector b){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  CalculateError
   *  Description:  Using 2-Norm to Calculate Norm(Ax - b). 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  Vector t = new double[n];   //t = Ax - b.
  double Sum = 0;
  for(int k = 0; k < n; k++){
      t[k] = -b[k];
      for(int j = 0; j < n; j++)
          t[k] += A[k][j] * x[j];
      Sum += t[k] * t[k];
  }
  double Error = sqrt(Sum);
  printf("The Error Vector is:\n");
  PrintVector(n, t);
  printf("Error = %.12lf\n", Error);
  return SUCCESS;
}
```
  
#### Main Function - Calling the Functions implemented above

```cpp
int main(){
  Matrix A = NULL;
  Vector b = NULL;
  int n = 0;
  ReadFromInput(n, A, b, 1);
  Vector x = NULL;
  x = new double[n];
  switch(1){
      case 1:
          GaussMethod(n, A, b, x);
          break;
      case 2:
          DoolittleMethod(n, A, b, x);
          break;
      default:
          printf("Invalid Argument.\n");
          exit(1);
  }
  ReadFromInput(n, A, b, 0);
  CalculateError(n, A, x, b);
  return 0;
}
```
  
### Result
  
#### Example 1


Using Gaussian Method:

$ g++ DecompositionMethod.cpp -o De.out
$ ./De.out 
Matrix A:
0.11110.12500.14290.16670.2000
0.12500.14290.16670.20000.2500
0.14290.16670.20000.25000.3333
0.16670.20000.25000.33330.5000
0.20000.25000.33330.50001.0000
Vector b:
1.00001.00001.00001.00001.0000

Matrix After Elimination:
0.20000.25000.33330.50001.0000
0.0000-0.0139-0.0423-0.1111-0.3556
0.00000.0000-0.0024-0.0167-0.1200
0.00000.0000-0.00000.00080.0152
0.00000.00000.00000.0000-0.0007
Vector After Elimination:
1.00000.4444-0.1000-0.0190-0.0036
The solution is:
629.9978 -1119.9957 629.9972 -119.9994 5.0000

The Error Vector is:
-0.0000-0.00000.0000-0.0000-0.0000
Error = 0.000000000000

Using Doolittle Method:

$ g++ DecompositionMethod.cpp -o De.out
$ ./De.out 
Matrix A:
0.11110.12500.14290.16670.2000
0.12500.14290.16670.20000.2500
0.14290.16670.20000.25000.3333
0.16670.20000.25000.33330.5000
0.20000.25000.33330.50001.0000
Vector b:
1.00001.00001.00001.00001.0000

L:
1.00000.00000.00000.00000.0000
1.12501.00000.00000.00000.0000
1.28572.66671.00000.00000.0000
1.50005.60005.25001.00000.0000
1.800011.200021.000012.00001.0000
U:
0.11110.12500.14290.16670.2000
0.00000.00220.00600.01250.0250
0.00000.00000.00050.00240.0095
0.00000.00000.00000.00080.0100
0.00000.00000.00000.00000.0400
x:
629.9978 -1119.9957 629.9972 -119.9994 5.0000
The Error Vector is:
-0.00000.0000-0.0000-0.00000.0000
Error = 0.000000000000

We don't have much error here...thanks to the double precision floating point type.
  
#### Example 2


Using Gaussian Method:

$ g++ DecompositionMethod.cpp -o De.out
$ ./De.out 
Matrix A:
7.20002.3000-4.40000.5000
1.30006.3000-3.50002.8000
5.60000.90008.1000-1.3000
1.50000.40003.70005.9000
Vector b:
15.10001.800016.600036.9000

Matrix After Elimination:
7.20002.3000-4.40000.5000
0.00005.8847-2.70562.7097
0.00000.000011.1135-1.2796
0.00000.00000.00006.3596
Vector After Elimination:
15.1000-0.92644.715631.7982
The solution is:
3.0000-2.00001.00005.0000
The Error Vector is:
-0.0000-0.0000-0.00000.0000
Error = 0.000000000000

Using Doolittle Method:

$ g++ DecompositionMethod.cpp -o De.out
$ ./De.out 
Matrix A:
7.20002.3000-4.40000.5000
1.30006.3000-3.50002.8000
5.60000.90008.1000-1.3000
1.50000.40003.70005.9000
Vector b:
15.10001.800016.600036.9000

L:
1.00000.00000.00000.0000
0.18061.00000.00000.0000
0.7778-0.15111.00000.0000
0.2083-0.01350.41211.0000
U:
7.20002.3000-4.40000.5000
0.00005.8847-2.70562.7097
0.00000.000011.1135-1.2796
0.00000.00000.00006.3596
x:
3.0000-2.00001.00005.0000
The Error Vector is:
-0.0000-0.0000-0.00000.0000
Error = 0.000000000000
  
### Summary


For the 1st example, when the true input are fractions, we still have some larger error because I use an file to input, and introduced function _strtod_. So I don't even have a way to represent fractions as accurate as a double type - What I can do is just input more digits, as an approximate value.

Finally, in these two examples, our error is eliminated below $10^{-12}$. We can say our programs works well.
  
## Lab3 - Inverse Power Method to Solve Eigenvalue
  
### Description

Solve the minimal eigenvalue and its eigenvector of a given matrix.
  
### Code

Note: Lab3 referenced the code through "_Lab2.h_"

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  Inverse-Power-Method.cpp
*
*    Description: Use inverse power method to calculate the minimal eigenvalue.
*
*        Version:  1.0
*        Created:  04/08/2022 02:35:07 UTC
*       Revision:  none
*       Compiler:  gcc
*
*         Author:  CuSO4_Deposit
*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <iostream>
#include <fstream>
#include <math.h>
#include "../Lab2/Lab2.h"

using namespace std;

#define SUCCESS 0
#define FAILURE 1
#define INPUTPATH "./InputData.txt"

typedef int Status;
typedef double* Vector;
typedef double** Matrix;

Status Matrix_Multiplication(int A_m, int A_n, int B_m, int B_n, Matrix A, Matrix B, Matrix Result){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Matrix_Multiplication
   *  Description:  Calculate the multiplication of A and B. A has m rows and n cols. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(A_n != B_m){
      printf("A and B can't multiply.\n");
      return FAILURE;
  }
  for(int i = 0; i < A_m; i++){
      for(int j = 0; j < B_n; j++){
          Result[i][j] = 0;
          for(int k = 0; k < A_n; k++)
              Result[i][j] += A[i][k] * B[k][j];
      }
  }
  return SUCCESS;
}

Status IPM_Iteration(int n, Matrix A, double& eigenvalue, Vector eigenvector){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  IPM_Iteration
   *  Description:  Do the iteration of inverse power method to A. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  Vector x = new double[n];
  //double value = sqrt(double(1) / n); 
  double value = 1;
  for(int i = 0; i < n; i++)
      x[i] = 1;
  Vector x_prior = new double[n];
  for(int i = 0; i < n; i++)
      x_prior[i] = value;
  Vector temp = new double[n];
  for(int i = 0; i < n; i++)
      temp[i] = 1;

  Matrix L = new Vector[n];
  for(int i = 0; i < n; i++)
      L[i] = new double[n];
  Matrix U = new Vector[n];
  for(int i = 0; i < n; i++)
      U[i] = new double[n];
  LUDecomposition(n, A, L, U);
  double error = 10;
  double Max_prior = value;
  double Max = 0;
  int x_idx = 0;
  int x_prior_idx = 0;
  bool indexFlag = 0;
  double eigenvalue_prior;
  int iter_counter = 0;

  while(error > pow(10, -5)  indexFlag == 0){
      printf("\n");
      printf("Iteration #%d:\n", ++iter_counter);
      LowerSolve(n, L, x_prior, temp); // Now temp is the temp vector to solve equation.
      UpperSolve(n, U, temp, x);       //
      printf("x[%d] =\n", iter_counter);
      PrintVector(n, x);
      Max = x[0]; x_idx = 0;
      for(int i = 1; i < n; i++)  // Find max entry in x(not normalized)
          if(x[i] > Max){
              Max = x[i];
              x_idx = i;
          }
      //error = abs(Max - Max_prior);
      printf("Error = %lf.\n", error);
      if(x_idx == x_prior_idx)
          indexFlag = 1;
      else    indexFlag = 0;

      for(int i = 0; i < n; i++)              //* Update data (x -> x_prior)
          x_prior[i] = x[i] / Max;            //*
      Max_prior = Max; x_prior_idx = x_idx;   //*
      printf("Y[%d] =\n", iter_counter);//*
      PrintVector(n, x_prior);

      eigenvalue_prior = eigenvalue;
      eigenvalue = 1 / x[x_idx];
      error = abs(eigenvalue - eigenvalue_prior);

      printf("Estimated EV = %lf\n", eigenvalue);
  }
  for(int i = 0; i < n; i++)
      eigenvector[i] = x_prior[i];
  return SUCCESS;
}

int main(){
  Matrix A = NULL;
  int n = 0;
  ReadFromInput(n, A, 1);
  double EValue;
  Vector EVector = new double[n];
  IPM_Iteration(n, A, EValue, EVector);
  printf("EigenValue = %lf\n", EValue);
  printf("EigenVector:\n");
  PrintVector(n, EVector);
  return 0;
}
```
  
### Result
  
#### Example 1


Raw Output:

$ ./Inverse-Power-Method.out 
Matrix A:
0.11110.12500.14290.16670.2000
0.12500.14290.16670.20000.2500
0.14290.16670.20000.25000.3333
0.16670.20000.25000.33330.5000
0.20000.25000.33330.50001.0000


Iteration #1:
x\[1\] =
629.9978-1119.9957629.9972-119.99945.0000
Error = 10.000000.
Y\[1\] =
1.0000-1.77781.0000-0.19050.0079
Estimated EV = 0.001587

Iteration #2:
x\[2\] =
260004.0147-529506.9293348753.7531-80203.06204226.2173
Error = 0.001587.
Y\[2\] =
0.7455-1.51831.0000-0.23000.0121
Estimated EV = 0.000003

Iteration #3:
x\[3\] =
226394.6541-461628.5632304559.4849-70212.33383714.5607
Error = 0.001584.
Y\[3\] =
0.7434-1.51571.0000-0.23050.0122
Estimated EV = 0.000003
EigenValue = 0.000003
EigenVector:
0.7434-1.51571.0000-0.23050.0122

Iteration # i

$X^{(i)}$

$Y^{(i)}$

$\\lambda$

1

629.9978 -1119.9957 629.9972 -119.9994 5.0000

1.0000 -1.7778 1.0000 -0.1905 0.0079

0.001587

2

260004.0147 -529506.9293 348753.7531 -80203.0620 4226.2173

0.7455 -1.5183 1.0000 -0.2300 0.0121

0.000003

3

226394.6541 -461628.5632 304559.4849 -70212.3338 3714.5607

0.7434 -1.5157 1.0000 -0.2305 0.0122

0.000003

Iteration Process
  
#### Example 2


Raw Output:

$ ./Inverse-Power-Method.out
Matrix A:
4.0000-1.00001.00003.0000
16.0000-2.0000-2.00005.0000
16.0000-3.0000-1.00007.0000
6.0000-4.00002.00009.0000


Iteration #1:
x\[1\] =
0.00002.0000-0.00001.0000
Error = 10.000000.
Y\[1\] =
0.00001.0000-0.00000.5000
Estimated EV = 0.500000

Iteration #2:
x\[2\] =
-0.62505.6250-2.37503.5000
Error = 0.500000.
Y\[2\] =
-0.11111.0000-0.42220.6222
Estimated EV = 0.177778

Iteration #3:
x\[3\] =
-0.93338.0778-3.43335.0444
Error = 0.322222.
Y\[3\] =
-0.11551.0000-0.42500.6245
Estimated EV = 0.123796

Iteration #4:
x\[4\] =
-0.93628.0899-3.44385.0543
Error = 0.053981.
Y\[4\] =
-0.11571.0000-0.42570.6248
Estimated EV = 0.123611

Iteration #5:
x\[5\] =
-0.93678.0938-3.44555.0568
Error = 0.000186.
Y\[5\] =
-0.11571.0000-0.42570.6248
Estimated EV = 0.123551

Iteration #6:
x\[6\] =
-0.93678.0939-3.44555.0568
Error = 0.000059.
Y\[6\] =
-0.11571.0000-0.42570.6248
Estimated EV = 0.123551
EigenValue = 0.123551
EigenVector:
-0.11571.0000-0.42570.6248

Iteration # i

X^{(i)}

Y^{(i)}

$\\lambda$

1

0.0000 2.0000 -0.0000 1.0000

0.0000 1.0000 -0.0000 0.5000

0.500000

2

\-0.6250 5.6250 -2.3750 3.5000

\-0.1111 1.0000 -0.4222 0.6222

0.177778

3

\-0.9333 8.0778 -3.4333 5.0444

\-0.1155 1.0000 -0.4250 0.6245

0.123796

4

\-0.9362 8.0899 -3.4438 5.0543

\-0.1157 1.0000 -0.4257 0.6248

0.123611

5

\-0.9367 8.0938 -3.4455 5.0568

\-0.1157 1.0000 -0.4257 0.6248

0.123551

6

\-0.9367 8.0939 -3.4455 5.0568

\-0.1157 1.0000 -0.4257 0.6248

0.123551

Iteration Process
  
### Analysis

The smaller the minimal eigenvalue is, the faster the iteration converges. In example 1(iterated 3 times), eigenvalue is 0.000003, much smaller than 0.123 in ex2(iterated 6 times).
  
## Lab4 - Jacobi Method to Solve Eigenvalue
  
### Description

Solve all eigenvalues of a given matrix, using Jacobi method.
  
### Code

Note: Lab4 referenced the code through "_Lab2.h_", it's code is [here](#Lab2).

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  JacobiMethod.cpp
*
*    Description:  Solve n Eigenvalues of a given matrix.
*
*        Version:  1.0
*        Created:  04/25/2022 14:41:30 UTC
*       Revision:  none
*       Compiler:  gcc
*         Author:  CuSO4_Deposit
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*/
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <iostream>
#include <fstream>
#include <math.h>
#include <ctime>
#include "..\Lab2\Lab2.h"
using namespace std;

#define SUCCESS 0
#define FAILURE 1
#define INPUTPATH "./InputData.txt"

Status GenerateRandomMatrix(int n, Matrix A){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  GenerateRandomMatrix
   *  Description:  Generate a random symmetry matrix, where each entry ranges from 0 to 1.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  srand(time(0));
  for(int i = 0; i < n; i++)
      for(int j = i; j < n; j++){
          A[i][j] = rand()%10001 / 10000.0;
          A[j][i] = A[i][j];
      }
          
  return SUCCESS;
}

bool CheckSymmetry(int n, Matrix A){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  CheckSymmetry
   *  Description:  if A is symmetry, return 1, otherwise return 0.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++){
      for(int j = i + 1; j < n; j++){
          if(A[i][j] != A[j][i])
              return false;
      }
  }
  return true;
}

double NonDiagSquareSum(int n, Matrix A){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  NonDiagSum
   *  Description:  return the sum of non-diagonal entries' square of A. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  double Sum = 0;
  for(int i = 0; i < n; i++)
      for(int j = i + 1; j < n; j++)
          Sum += A[i][j] * A[i][j];
  return 2 * Sum;
}

Status JacobiMethod(int n, Matrix A, Vector EigenvalueVector, double ErrorController){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  JacobiMethod
   *  Description:  Output the n eigenvalues via <EigenvalueVector>
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(!CheckSymmetry(n, A)){
      printf("The input matrix is not symmetry.\n");
      return FAILURE;
  }
  int MaxEntry_i = 0; int MaxEntry_j = 0;
  double MaxEntry = 0;
  int LoopCounter = 0;
  double s = 0; double t = 0;
  double t1 = 0; double t2 = 0;
  double c = 0; double d = 0;
  double api = 0; double aqi = 0;
  while(NonDiagSquareSum(n, A) > ErrorController){
      MaxEntry = abs(A[0][1]); MaxEntry_i = 0; MaxEntry_j = 1;
      for(int i = 0; i < n; i++)
          for(int j = 0; j < n; j++){
              if(j == i)  continue;
              if(abs(A[i][j]) > MaxEntry){
                  MaxEntry_i = i;
                  MaxEntry_j = j;
                  MaxEntry = abs(A[i][j]);
              }
          }
      s = (A[MaxEntry_j][MaxEntry_j] - A[MaxEntry_i][MaxEntry_i]) / (2 * A[MaxEntry_i][MaxEntry_j]);

      t1 = -s - sqrt(s * s + 1);  t2 = -t1 - 2 * s;
      t = abs(t1) > abs(t2) ? t2 : t1;
      if(s == 0)  t = 1;
      c = 1 / sqrt(1 + t * t); d = t * c;

      for(int i = 0; i < n; i++){
          if(i {{< logseq/mark >}} MaxEntry_i  i {{< / logseq/mark >}} MaxEntry_j)
              continue;
          api = A[i][MaxEntry_i]; aqi = A[i][MaxEntry_j];
          A[i][MaxEntry_i] = c * api - d * aqi; 
          A[MaxEntry_i][i] = A[i][MaxEntry_i];
          A[i][MaxEntry_j] = c * aqi + d * api;
          A[MaxEntry_j][i] = A[i][MaxEntry_j];
      }
      A[MaxEntry_i][MaxEntry_i] -= t * A[MaxEntry_i][MaxEntry_j];
      A[MaxEntry_j][MaxEntry_j] += t * A[MaxEntry_i][MaxEntry_j];
      A[MaxEntry_i][MaxEntry_j] = 0;
      A[MaxEntry_j][MaxEntry_i] = 0;

      printf("Iteration #%d\n", ++LoopCounter);
      printf("Non-Diagonal Entries' Square Sum = %lf.\n", NonDiagSquareSum(n, A));
      PrintMatrix(n, A);
  } 
  for(int i = 0; i < n; i++)
      EigenvalueVector[i] = A[i][i];
  printf("Eigenvalues:\n");
  PrintVector(n, EigenvalueVector);
  return SUCCESS;
}

int main(){
  Matrix A = NULL;
  int n = 0;
  ReadFromInput(n, A, 1);
//    GenerateRandomMatrix(n, A);
  PrintMatrix(n, A);
  Vector EigenvalueVector = new double[n];
  JacobiMethod(n, A, EigenvalueVector, 0.000001);
  return 0;
}
```
  
### Result
  
#### Example 1



Raw Output:

$ ./JacobiMethod.o 
Matrix A:
1.0000    2.0000    3.0000    4.0000    
2.0000    5.0000    6.0000    7.0000    
3.0000    6.0000    8.0000    9.0000    
4.0000    7.0000    9.0000    10.0000    

Iteration #1
Non-Diagonal Entries' Square Sum = 228.000000.
1.0000    2.0000    -0.4323    4.9813    
2.0000    5.0000    -0.1977    9.2174    
-0.4323    -0.1977    -0.0554    0.0000    
4.9813    9.2174    0.0000    18.0554    
Iteration #2
Non-Diagonal Entries' Square Sum = 58.078156.
1.0000    -0.5118    -0.4323    5.3433    
-0.5118    0.2329    -0.1756    0.0000    
-0.4323    -0.1756    -0.0554    -0.0908    
5.3433    0.0000    -0.0908    22.8225    
Iteration #3
Non-Diagonal Entries' Square Sum = 0.975846.
-0.2381    -0.4986    -0.4006    0.0000    
-0.4986    0.2329    -0.1756    -0.1155    
-0.4006    -0.1756    -0.0554    -0.1861    
0.0000    -0.1155    -0.1861    24.0606    
Iteration #4
Non-Diagonal Entries' Square Sum = 0.478622.
-0.5540    0.0000    -0.4324    -0.0618    
0.0000    0.5488    0.0661    -0.0976    
-0.4324    0.0661    -0.0554    -0.1861    
-0.0618    -0.0976    -0.1861    24.0606    
Iteration #5
Non-Diagonal Entries' Square Sum = 0.104665.
-0.8038    0.0331    0.0000    -0.1466    
0.0331    0.5488    0.0572    -0.0976    
0.0000    0.0572    0.1944    -0.1302    
-0.1466    -0.0976    -0.1302    24.0606    
Iteration #6
Non-Diagonal Entries' Square Sum = 0.061674.
-0.8047    0.0325    -0.0008    0.0000    
0.0325    0.5488    0.0572    -0.0978    
-0.0008    0.0572    0.1944    -0.1302    
0.0000    -0.0978    -0.1302    24.0614    
Iteration #7
Non-Diagonal Entries' Square Sum = 0.027790.
-0.8047    0.0325    -0.0008    0.0000    
0.0325    0.5488    0.0567    -0.0981    
-0.0008    0.0567    0.1937    0.0000    
0.0000    -0.0981    0.0000    24.0621    
Iteration #8
Non-Diagonal Entries' Square Sum = 0.008545.
-0.8047    0.0325    -0.0008    -0.0001    
0.0325    0.5484    0.0567    0.0000    
-0.0008    0.0567    0.1937    -0.0002    
-0.0001    0.0000    -0.0002    24.0625    
Iteration #9
Non-Diagonal Entries' Square Sum = 0.002113.
-0.8047    0.0320    -0.0058    -0.0001    
0.0320    0.5573    0.0000    -0.0000    
-0.0058    0.0000    0.1849    -0.0002    
-0.0001    -0.0000    -0.0002    24.0625    
Iteration #10
Non-Diagonal Entries' Square Sum = 0.000067.
-0.8055    0.0000    -0.0058    -0.0001    
0.0000    0.5580    -0.0001    -0.0000    
-0.0058    -0.0001    0.1849    -0.0002    
-0.0001    -0.0000    -0.0002    24.0625    
Iteration #11
Non-Diagonal Entries' Square Sum = 0.000000.
-0.8055    -0.0000    0.0000    -0.0001    
-0.0000    0.5580    -0.0001    -0.0000    
0.0000    -0.0001    0.1849    -0.0002    
-0.0001    -0.0000    -0.0002    24.0625    
Eigenvalues:
-0.8055    0.5580    0.1849    24.0625    

分析：

1\. The sum of non-diagonal entries' square shows a downward tendency。


2\. Is the output correct？

```mathics
(* Mathicscript: 4.0.0, Mathics 4.0.0 *) 
In[1]:= A = {{1 - x,2,3,4},{2,5 - x,6,7},{3,6,8 - x,9},{4,7,9,10 - x}} Out[1]:= {{1 - x, 2, 3, 4}, {2, 5 - x, 6, 7}, {3, 6, 8 - x, 9}, {4, 7, 9, 10 - x}} 
In[2]:= Solve[Det[A] == 0, x] 
Out[2]:= {{x -> 6 + Sqrt[143 / 2 + 29 Sqrt[6]] + 5 Sqrt[6] / 2}, {x -> 6 - 5 Sqrt[6] / 2 - Sqrt[143 / 2 - 29 Sqrt[6]]}, {x -> 6 + Sqrt[143 / 2 - 29 Sqrt[6]] - 5 Sqrt[6] / 2}, {x -> 6 - Sqrt[143 / 2 + 29 Sqrt[6]] + 5 Sqrt[6] / 2}} 
In[3]:= Solve[Det[A] == 0, x]//N 
Out[3]:= {{x -> 24.0625}, {x -> -0.805485}, {x -> 0.558036}, {x -> 0.184914}}
```

So the program gives a correct output.
  
## Lab 5 - Cubic Spline Interpolation
  
### Description

Given function(or a point set), do a cubic spline interpolation. Require: at least 20 interpolation points, implement 2 boundary conditions and 3 interpolation method.
  
### Code
  
#### Directory Tree

.
├── CubicSpline.md
├── Figure
└── src
  ├── Arguments.txt
  ├── CubicSpline.py
  ├── FunctionSet.py
  ├── Lab5.cpp
  ├── Lab5.o
  └── \_\_pycache\_\_
      └── FunctionSet.cpython-38.pyc
  
#### C code

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  Lab5.cpp
*
*    Description:  
*
*        Version:  1.0
*        Created:  05/11/2022 03:42:39 PM
*       Revision:  none
*       Compiler:  gcc
*
*         Author:  CuSO4_Deposit
*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*/


#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include <iostream>
#include <fstream>
#include <math.h>
#include <ctime>
using namespace std;

#define SUCCESS 0
#define FAILURE 1
#define INPUTPATH "./InputData.txt"

typedef int Status;
typedef double* Vector;
typedef double** Matrix;
typedef double Function;

Function f(double x){
  double e = 2.718281828; 
  return (x + sin(2 * x)) / (1 + pow(e, -x));
}

Status PrintMatrix(int n, Matrix A){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PrintMatrix
   *  Description:  Print the Matrix A to the terminal. n is the Dimension of A. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++){
      for(int j = 0; j < n; j++){
          printf("%.4lf\t", A[i][j]);
      }
      printf("\n");
  }
  return SUCCESS;
}

Status PrintVector(int n, Vector v){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PrintVector
   *  Description:  Print the Vector v to the terminal(in row form). n is the Dimension of v. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++)
      printf("%.4lf\t", v[i]);
  printf("\n");
  return SUCCESS;
}


Status ThomasSolve(int n, Matrix A, Vector x, Vector b){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  ThomasSolve
   *  Description:  Solve Equation using Thomas Algorithm. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  for(int i = 0; i < n; i++)
      for(int j = 0; j < n; j++){
          if(j >= i - 1 && j <= i + 1 )
              continue;
          if(A[i][j] != 0){
              printf("Matrix is invalid.\n A[%d][%d] = %lf.\n", i, j, A[i][j]);
              return FAILURE;
          }
      }
  Vector u = new double[n];
  Vector v = new double[n];
  Vector y = new double[n];
  u[0] = A[0][0];
  v[0] = A[0][1] / u[0];
  y[0] = b[0] / u[0];
  for(int k = 1; k < n - 1; k++){
      u[k] = A[k][k] - A[k][k - 1] * v[k - 1];
      v[k] = A[k][k + 1] / u[k];
      y[k] = (b[k] - A[k][k - 1] * y[k - 1]) / u[k];
  }
  u[n - 1] = A[n - 1][n - 1] - A[n - 1][n - 2] * v[n - 2];
  v[n - 1] = 0;
  y[n - 1] = (b[n - 1] - A[n - 1][n - 2] * y[n - 2]) / u[n - 1];

  x[n - 1] = y[n - 1];
  for(int k = n - 2; k > -1; k--){
      x[k] = y[k] - v[k] * x[k + 1];
  }
  return SUCCESS;
}

Status Sample(int n, double a, double b, Vector SamplePoints, int method){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Sample
   *  Description:  return a vector of SamplePoints. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  switch(method){
      case 0:{
          double h = (b - a) / n;
          SamplePoints[0] = a;
          for(int i = 1; i < n + 1; i++)
              SamplePoints[i] = SamplePoints[i - 1] + h;

          printf("SamplePoints:\n");
          PrintVector(n + 1, SamplePoints);

          break;
      }
      case 1:{
          SamplePoints[0] = a;
          SamplePoints[n] = b;
          double randomNumber;
          bool flag_moved;
          for(int i = 1; i < n; i++){
              randomNumber = a + (b - a) * (rand() / (RAND_MAX + 1.0));
              flag_moved = 0;
              for(int j = 0; j < i; j++){
                  if(SamplePoints[j] < randomNumber)
                      continue;
                  flag_moved = 1;
                  for(int k = i - 1; k >= j; k--){
                      SamplePoints[k + 1] = SamplePoints[k];
                  }
                  SamplePoints[j] = randomNumber;
                  break;
              }
              if(flag_moved == 0)
                  SamplePoints[i] = randomNumber;
          }

          break;
      }
      case 2:{
          SamplePoints[0] = a;
          SamplePoints[n] = b;
          double Mid = (a + b) / 2;
          if(n % 2)   // n - 1 is odd
              SamplePoints[n / 2] = Mid;
          double randomNumber;
          bool flag_moved;
          for(int i = 1; i < n / 2.0; i++){
              randomNumber = a + (Mid - a) * (rand() / (RAND_MAX + 1.0));
              flag_moved = 0;
              for(int j = 0; j < i; j++){
                  if(SamplePoints[j] < randomNumber)
                      continue;
                  flag_moved = 1;
                  for(int k = i - 1; k >= j; k--){
                      SamplePoints[k + 1] = SamplePoints[k];
                  }
                  SamplePoints[j] = randomNumber;
                  break;
              }
              if(flag_moved == 0)
                  SamplePoints[i] = randomNumber;
          }

          for(int i = n / 2 + 1; i < n; i++)
              SamplePoints[i] = 2 * Mid - SamplePoints[n - i];

          break;
      }
      default:
          return FAILURE;
          break;
  }
  return SUCCESS;
}

Status CubicSpline(Function f(double), double a, double b, int n, int Boundary, int SampleMethod){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  CubicSpline
   *  Description:  Use Cubic Spline Method to interpolate function f.
   *    Parameter:  0, ..., n is the number of interpolation points.
   *                  Condition = 0: Natrual Condition
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  Matrix A = NULL;

  ofstream ofs;
  ofs.open("Arguments.txt", ios::out);
  ofs << "[" << n << ", " << a << ", " << b << ", " << Boundary << ", " << SampleMethod << "]\n";

  switch(Boundary){
      case 0:{
          A = new Vector[n - 1];
          for(int i = 0; i < n - 1; i++){
              A[i] = new double[n - 1];
          }
          for(int i = 0; i < n - 1; i++)
          for(int j = 0; j < n - 1; j++)
          A[i][j] = 0;
          Vector SamplePoints = new double[n + 1];
          Sample(n, a, b, SamplePoints, SampleMethod);
          PrintVector(n + 1, SamplePoints);
          Vector h = new double[n];
          for(int i = 0; i < n; i++)
          h[i] = SamplePoints[i + 1] - SamplePoints[i];
          Vector d = new double[n - 1];
          
          A[0][0] = 2;
          A[0][1] = h[1] / (h[1] + h[0]);
          d[0] = 6 * ((f(SamplePoints[2]) - f(SamplePoints[1])) / h[1] - (f(SamplePoints[1]) - f(SamplePoints[0])) / h[0]) / (h[1] + h[0]) - 0;
          
for(int i = 1; i < n - 2; i++){
              A[i][i] = 2;
              A[i][i + 1] = h[i + 1] / (h[i + 1] + h[i]);
              A[i][i - 1] = 1 - A[i][i + 1];
              d[i] = 6 * ((f(SamplePoints[i + 2]) - f(SamplePoints[i + 1])) / h[i + 1] - (f(SamplePoints[i + 1]) - f(SamplePoints[i + 0])) / h[i + 0]) / (h[i + 1] + h[i]);
          }
          A[n - 2][n - 2] = 2;
          h[n - 1] = SamplePoints[n] - SamplePoints[n - 1];

          A[n - 2][n - 3] = 1 - h[n - 1] / (h[n - 1] + h[n - 2]);
          d[n - 2] = 6 * ((f(SamplePoints[n]) - f(SamplePoints[n - 1])) / h[n - 1] - (f(SamplePoints[n - 1]) - f(SamplePoints[n - 2])) / h[n - 2]) / (h[n - 1] + h[n - 2]);
Vector M = new double[n - 1];
          ThomasSolve(n - 1, A, M, d);


          printf("The Vector [x_0, ..., x_%d] is:\n[", n);
          for(int i = 0; i < n; i++)
              printf("%.4lf, ", SamplePoints[i]);
          printf("%.4lf]\n", SamplePoints[n]);
          printf("The Vector [h_0, ..., h_%d] is:\n[", n - 1);
          for(int i = 0; i < n - 1; i++)
              printf("%.4lf, ", h[i]);
          printf("%.4lf]\n", h[n - 1]);
          printf("The Vector [M_0, ..., M_%d] is:\n[%.4lf, ", n, 0.0);
          for(int i = 0; i < n - 1; i++)
              printf("%.4lf, ", M[i]);
          printf("%.4lf]\n", 0.0);

          ofs << "[";
          for(int i = 0; i < n; i++)
              ofs << SamplePoints[i] << ", ";
          ofs << SamplePoints[n] << "]\n";
          ofs << "[";
          for(int i = 0; i < n - 1; i++)
              ofs << h[i] << ", ";
          ofs << h[n - 1] << "]\n";
          ofs << "[0.0000, ";
          for(int i = 0; i < n - 1; i++)
              ofs << M[i] << ", ";
          ofs << "0.0000]\n";

          break;
      }
      case 1:{
          double m0, mn;
          printf("Please input f'(x_0), f'(x_n), seperated by space:\n");
          cin >> m0 >> mn;
          printf("\n");
          A = new Vector[n + 1];
          for(int i = 0; i < n + 1; i++){
              A[i] = new double[n + 1];
          }
          for(int i = 0; i < n + 1; i++)
          for(int j = 0; j < n + 1; j++)
          A[i][j] = 0;
          Vector SamplePoints = new double[n + 1];
          Sample(n, a, b, SamplePoints, SampleMethod);
          Vector h = new double[n];
          for(int i = 0; i < n; i++)
          h[i] = SamplePoints[i + 1] - SamplePoints[i];
          Vector d = new double[n + 1];
          
          A[0][0] = 2;
          A[0][1] = 1;
          d[0] = 6 * (((f(SamplePoints[1]) - f(SamplePoints[0])) / h[0]) - m0) / h[0];
          
for(int i = 1; i < n; i++){
              A[i][i] = 2;
              A[i][i + 1] = h[i] / (h[i] + h[i - 1]);
              A[i][i - 1] = 1 - A[i][i + 1];
              d[i] = 6 * ((f(SamplePoints[i + 1]) - f(SamplePoints[i])) / h[i] - (f(SamplePoints[i]) - f(SamplePoints[i - 1])) / h[i - 1]) / (h[i - 1] + h[i]);
          }
          A[n][n] = 2;
          A[n][n - 1] = 1;
          d[n] = 6 * (mn - (f(SamplePoints[n]) - f(SamplePoints[n - 1])) / h[n - 1]) / h[n - 1];
Vector M = new double[n + 1];
          ThomasSolve(n + 1, A, M, d);

          printf("The Vector [x_0, ..., x_%d] is:\n[", n);
          for(int i = 0; i < n; i++)
              printf("%.4lf, ", SamplePoints[i]);
          printf("%.4lf]\n", SamplePoints[n]);
          printf("The Vector [h_0, ..., h_%d] is:\n[", n - 1);
          for(int i = 0; i < n - 1; i++)
              printf("%.4lf, ", h[i]);
          printf("%.4lf]\n", h[n - 1]);
          printf("The Vector [M_0, ..., M_%d] is:\n[", n);
          for(int i = 0; i < n; i++)
              printf("%.4lf, ", M[i]);
          printf("%.4lf]\n", M[n]);

          ofs << "[";
          for(int i = 0; i < n; i++)
              ofs << SamplePoints[i] << ", ";
          ofs << SamplePoints[n] << "]\n";
          ofs << "[";
          for(int i = 0; i < n - 1; i++)
              ofs << h[i] << ", ";
          ofs << h[n - 1] << "]\n";
          ofs << "[";
          for(int i = 0; i < n; i++)
              ofs << M[i] << ", ";
          ofs << M[n] << "]\n";
      }
  }

  return SUCCESS;
}

int main(){
  srand(time(0));
  Matrix A = NULL;
  Vector b = NULL;
  CubicSpline(f, -2, 4, 25, 1, 2);
  return 0;
}
```
  
#### python code

```python
import numpy as np
import matplotlib.pyplot as plt

ArgumentsFile = open('Arguments.txt', 'r')
Arguments = ArgumentsFile.readlines()

n, a, b, Boundary, Sample = eval(Arguments[0])

x = np.array(eval(Arguments[1])).astype(float)
h = np.array(eval(Arguments[2])).astype(float)
M = np.array(eval(Arguments[3])).astype(float)
ArgumentsFile.close()


def truef(x):
  return (x + np.sin(2 * x)) / (1 + (np.e) ** (-x))
y = truef(x)

FunctionFile = open('FunctionSet.py', 'w')
FunctionFile.write("def func(i, x):\n")
for i in range(25):
  a0 = (-M[i + 1] * x[i] ** 3 + M[i] * x[i + 1] ** 3) / (6 * h[i]) + (-M[i] * x[i + 1] + M[i + 1] * x[i]) * h[i] / 6 + (x[i + 1] * y[i] - x[i] * y[i + 1]) / h[i]
  a1 = (M[i + 1] * x[i] ** 2 - M[i] * x[i + 1] ** 2) / (2 * h[i]) + (M[i] - M[i + 1]) * h[i] / 6 + (y[i + 1] - y[i]) / h[i]
  a2 = (M[i] * x[i + 1] - M[i + 1] * x[i]) / (2 * h[i])
  a3 = (M[i + 1] - M[i]) / (6 * h[i])
  FunctionFile.write("    if i == {}:\n".format(i))
  #FunctionFile.write("        return", a0, "+ (", a1, ") * x + (", a2, ") * x ** 2 + (", a3, ") * x ** 3")
  FunctionFile.write("        return " + str(a0) + " + (" + str(a1) + ") * x + (" + str(a2) + ") * x ** 2 + (" + str(a3) + ") * x ** 3\n")
FunctionFile.close()


from FunctionSet import func as f

plt.figure()
for i in range(n):
  xi = np.linspace(x[i], x[i + 1], 15)
  yi = f(i, xi)
  plt.plot(xi, yi, color='blue')
plt.scatter(x, y, color='red', marker='x')
xtrue = np.linspace(a ,b)
plt.plot(xtrue, truef(xtrue), color='orange', alpha=0.7)
name = "Ex1Bdr" + str(Boundary) + "Sp" + str(Sample)
plt.title(name)
plt.savefig("../Figure/" + name + ".png")
plt.show()
```

In the python program, the expanded form of interpolation function is calculated by mathics/mathematica:

```mathics
In[1]:= Collect[(Ma (b - x)^3 + Mb (x - a)^3) / (6 h) + ((b - x) ya + (x - a) yb) / h - (h / 6) ((b - x) Ma + (x - a) Mb), x]
Out[1]:= Ma b ^ 3 / (6 h) - Ma b h / 6 - Mb a ^ 3 / (6 h) + Mb a h / 6 - a yb / h + b ya / h + x (-Ma b ^ 2 / (2 h) + Ma h / 6 + Mb a ^ 2 / (2 h) - Mb h / 6 - ya / h + yb / h) + x ^ 2 (Ma b / (2 h) - Mb a / (2 h)) + x ^ 3 (-Ma / (6 h) + Mb / (6 h))
```
  
### Result

$f(x) = \\dfrac{x}{x ^ 2 + x + 1}$

$f(x) = \\dfrac{x + \\sin {2x}}{1 + e^{-x}}$
  
## Lab 6 - FFT & IFFT
  
### Code

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  FFT.cpp
*
*    Description:  Fast Fourier Transform Algorithm
*
*        Version:  1.0
*        Created:  05/29/2022 02:17:10 UTC
*       Revision:  none
*       Compiler:  gcc
*
*         Author:  CuSO4_Deposit
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*/

#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>
#include <iostream>
#include <fstream>
#include <math.h>
#include <ctime>
#include <complex>
#include <vector>
using namespace std;

#define SUCCESS 0
#define FAILURE 1
#define PI 3.1415926535
#define E 2.718281828

const complex<double>I(0, 1);

typedef int Status;
typedef vector<complex<double>> Vector;
typedef complex<double> Function; 

Function F(double x){
  return 0.7 * sin(4 * PI * x) + sin(10 * PI * x);
}

Status PrintVector(Vector v, int mode){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PrintVector
   *  Description:  Print the Vector v to the terminal(in row form). n is the Dimension of v. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  int n = v.size();
  switch(mode){
      case 0:     
          for(int i = 0; i < n; i++)
          cout<< v[i] << " ";
          break;
      case 1:
          for(int i = 0; i < n; i++)
          cout<< v[i].real() << " ";
          break;
      case 2:
          for(int i = 0; i < n; i++)
          cout<< v[i].imag() << " ";
          break;
      default:
          cout << "Invalid mode input." << endl;
  }
  printf("\n");
  return SUCCESS;
}

Vector Sample(Function f(double), int Number){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Sample
   *  Description:  Sample Number points on interval[0, 1]
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  if(Number & (Number - 1)){
      cout << "Number must be the power of 2." << endl;
      exit(1); 
  }
  Vector fvector;
  double stepsize = 1.0 / Number;
  for(int i = 0; i < Number; i++){
      fvector.push_back(f(i * stepsize));
  }
  return fvector;
}

Vector AddError(double size, Vector v){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  AddError 
   *  Description:  Add a error of [0, size) to v. 
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  int n = v.size();
  cout << n;
  Vector result(n);
  for(int i = 0; i < n; i++)
      result[i] = v[i] + size * (rand() / (RAND_MAX + 1.0));
  return result;
}

Vector FFT(const Vector& f){
  int n = f.size();
  if(n == 1)
      return f;
  complex<double>wn;
  wn = pow(E, - I * 2.0 * PI / (double)n);
  complex<double>w(1, 0);
  Vector f0, f1;
  for(int i = 0; i < n / 2; i++){
      f0.push_back(f[2 * i]);
      f1.push_back(f[2 * i + 1]);
  }
  Vector g0, g1;
  g0 = FFT(f0);
  g1 = FFT(f1);
  Vector g(n);
  for(int k = 0; k < n / 2; k++){
      g[k] = (g0[k] + w * g1[k]) / 2.0;
      g[k + n / 2] = (g0[k] - w * g1[k]) / 2.0;
      w = w * wn;
  }
  return g;
}

Vector IFFT(const Vector& f){
  int n = f.size();
  if(n == 1)
      return f;
  complex<double>wn;
  wn = pow(E, I * 2.0 * PI / (double)n);
  complex<double>w(1, 0);
  Vector f0, f1;
  for(int i = 0; i < n / 2; i++){
      f0.push_back(f[2 * i]);
      f1.push_back(f[2 * i + 1]);
  }
  Vector g0, g1;
  g0 = IFFT(f0);
  g1 = IFFT(f1);
  Vector g(n);
  for(int k = 0; k < n / 2; k++){
      g[k] = (g0[k] + w * g1[k]);
      g[k + n / 2] = (g0[k] - w * g1[k]);
      w = w * wn;
  }
  return g;
}

Status PlotTime(Function F(double), int n){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PlotTime
   *  Description:  Output the code to plot F in python.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  ofstream ofs;
  ofs.open("TimeParam.txt", ios::out);
  ofs << "[";
  for(int i = 0; i < n - 1; i++){
      ofs << (double)i / n << ", "; 
  }
  ofs << (double)(n - 1) / n << "]\n";
  
  ofs << "[";
  for(int i = 0; i < n - 1; i++){
      ofs << F((double)i / n).real() << ", ";
  }
  ofs << F((double)(n - 1) / n).real() << "]\n";
  return 0;
}

Status PlotFrequency(Vector g, int n){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PlotFrequency
   *  Description:  Visualize Fourier Transformation.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  ofstream ofs;
  ofs.open("FrequencyParam.txt", ios::out);
  ofs << "[";
  for(int i = 0; i < n - 1; i++){
      ofs << i << ", "; 
  }
  ofs << n - 1 << "]\n";
  
  ofs << "[";
  for(int i = 0; i < n - 1; i++){
      ofs << sqrt(g[i].real() * g[i].real() + g[i].imag() * g[i].imag()) << ", ";
  }
  ofs << sqrt(g[n - 1].real() * g[n - 1].real() + g[n - 1].imag() * g[n - 1].imag()) << "]\n";
  return 0;
 
}

Status PlotTimeI(Vector g, int n, int mode){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  PlotFrequency
   *  Description:  Visualize Fourier Transformation.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */
  ofstream ofs;
  if(mode)
      ofs.open("ITimeParam1.txt", ios::out);
  else
      ofs.open("ITimeParam0.txt", ios::out);
  ofs << "[";
  for(int i = 0; i < n - 1; i++){
      ofs << i << ", "; 
  }
  ofs << n - 1 << "]\n";
  
  ofs << "[";
  for(int i = 0; i < n - 1; i++){
      ofs << g[i].real() << ", ";
  }
  ofs << g[n - 1].real() << "]\n";
  return 0;
 
}

int main(){
  srand(time(0));
  Vector tempvector = Sample(F, 64);
  Vector fvector = AddError(0.3, tempvector);
  Vector gvector = FFT(fvector);
  PrintVector(gvector, 1);
  PrintVector(gvector, 2);
  PlotTime(F, 64);
  PlotFrequency(gvector, 64);
  Vector igvector = IFFT(gvector);
  PlotTimeI(igvector, 64, 0);

  int n = gvector.size();
  for(int i = 0; i < n; i++)
      if(i > n / 8 && i < 3 * n / 8)
          gvector[i] = (0, 0);
  igvector = IFFT(gvector);

  PlotTimeI(igvector, 64, 1);
  return 0;
}
```

```numpy
import numpy as np
import matplotlib.pyplot as plt

ArgumentsFile = open("./ITimeParam1.txt", 'r')
Arguments = ArgumentsFile.readlines()

x = np.array(eval(Arguments[0])).astype(float)
y = np.array(eval(Arguments[1])).astype(float)

plt.figure()
plt.plot(x, y)
name = "Ex2ITime1n7"
plt.title(name)
plt.savefig("../figure/" + name + ".png")
plt.show()
```
  
## Lab7 - Romberg Integral
  
### Code

```cpp
/*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*
*       Filename:  Romberg.cpp
*
*    Description:  Implement Romberg Integral
*
*        Version:  1.0
*        Created:  06/14/2022 10:49:02 PM
*       Revision:  none
*       Compiler:  gcc
*
*         Author:  CuSO4_Deposit
*
* {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
*/

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <iostream>
#include <math.h>
using namespace std;

#define SUCCESS 0
#define FAILURE 1
#define PI 3.1415926535
#define E 2.718281828

typedef int Status;
typedef double Function;

Function f(double x){
  //return log(x);
  return sqrt(2 - sin(x) * sin(x));
}

Status Romberg(Function f(double x), double a, double b, double& result, double AccControler, int MaxLoopNumber, int verbose){
  /* 
   * {{< logseq/mark >}}=  FUNCTION  {{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}
   *         Name:  Romberg
   *  Description:  return the Romberg integral of f on [a, b] through <result>,
   *                  exit when loop <MaxLoopNumber> times or error < AccControler.
   * {{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}{{< logseq/mark >}}{{< / logseq/mark >}}=
   */ 
  double* RombergArray1 = new double[MaxLoopNumber + 1];
  double* RombergArray2 = new double[MaxLoopNumber + 1];
  double h = b - a;
  RombergArray1[0] = (f(a) + f(b)) * h / 2;
  
  if(verbose){
      printf("%lf\n", RombergArray1[0]);
  }

  for(int k = 1; k < MaxLoopNumber; k++){
      double sum = 0;
      double hk = h / pow(2, k);
      for(int i = 1; i < pow(2, k - 1) + 1; i++)
          sum += f(a + (2 * i - 1) * hk);
      RombergArray2[0] = (RombergArray1[0] + 2 * hk * sum) / 2;
      for(int j = 1; j < k + 1; j++){
          RombergArray2[j] = RombergArray2[j - 1] + (RombergArray2[j - 1] - RombergArray1[j - 1]) / (pow(4, j));
      }
      if(abs(RombergArray2[k] - RombergArray1[k - 1]) < AccControler){
          result = RombergArray2[k];
          return SUCCESS;
      }
      
      if(verbose){
          for(int i = 0; i < k + 1; i++)
              printf("%lf\t", RombergArray2[i]);
          printf("\n");
      }

      for(int j = 0; j < k + 1; j++)
          RombergArray1[j] = RombergArray2[j];

  }
  result = RombergArray2[MaxLoopNumber];
  return SUCCESS;
}

int main(){
  double result;
  //Romberg(f, 1, 2, result, 0.000001, 20, 1);
  Romberg(f, -PI / 6.0, 3.0 * PI / 4, result, 0.000001, 20, 1);
  return 0;
}
```
 