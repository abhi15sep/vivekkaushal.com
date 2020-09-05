---
title:  "Subtle Art of De-MOSS-ing"
date:   2018-06-03
categories:
- Other
tags:
- Hack
cover:
thumbnail:
---
Ever since it’s introduction in 1994, Stanford's MOSS, or Measure of Software Similarity, has been an integral part of programming classes worldwide to detect plagiarism in coding assignments. It’s an effective measure, a good system, but, it’s not perfect. 

Stanford’s MOSS is beatable. 

In this article, I aim to accumulate all knows techniques of beating MOSS, summarise knowledge from various sources, and bring it all together in one place. Though I would like to clarify that (most) assignments are an integral part of learning and this article has been written solely to satiate academic curiosity. 

<!--more-->

## 1. The 0-1 Ruse

I discovered this method by accident, and have since been extremely fond of it. It’s easy to implement and insanely effective; the only drawback is that it’s detectable if manual evaluation is a possibility. Declare variables for one and zero, and use them in all numeric assignments and operations, as frequently as you can. When all else fails, the 0-1 Ruse never disappoints. 

**MOSS Check : 0% Similarity**

```c
// Original Code
#include <stdio.h>
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```
```c
// Declarations made for one = 1 and zero = 0 and used as explained.
#include <stdio.h>
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    int one = 1;
    printf ("Enter second number: ");
    scanf("%d", &b);
    int zero = 0;
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op*one) {
        case 1: ans = a*one+b + zero;
                break;
        case 2: ans = (int) abs(a-b*one)+ zero;
                break;
        case 3: ans = a*one*b+ zero;
                break;
        case 4: ans = (int) pow(a*one,b)+ zero;
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans*one + zero);
    return 0;
}
```

### 2. Changing Workflow
The most common method to beat MOSS albeit with limited success in bigger projects, is the to change the code’s workflow; ie, rearrange the code’s core logic such that it’s in a different order than the codebase it’ll be tested against. The results, can be dramatic if done properly. Example:

**MOSS Check : 0% Similarity**

```c
// Original Code
#include <stdio.h>

int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

```c
// Code with workflow changed.

#include <stdio.h>

int main() {
    int a;
    printf ("Enter first number: ");
    scanf("%d", &a);
    int b;
    printf ("Enter second number: ");
    scanf("%d", &b);
    int ans;
    while (1) {
        printf("Choose what you want to do:\n1 for a*b\n2 for a+b\n3 for a^b\n4 for a~b\n");
        int op;
        int flag = 0;
        scanf ("%d", &op);
        switch (op) {
            case 3: ans = (int) pow(a,b);
                    break;
            case 1: ans = a*b;
                    break;
            case 4: ans = (int) abs(a-b);
                    break;
            case 2: ans = a+b;
                    break;
            default: printf ("\nChoose a Valid Operator\n");
                    flag = 1;
        }
        if (!flag){
            break;
        }
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

## 3. Variable-Array-Vector Triad

Variables can be converted to single value arrays, and when coding in a language that has vectors, vectors can be similarly used. As variables, arrays and vectors are all differently stored in memory, MOSS treats them as different. Hence, this method is ivy effective. 

**MOSS Check : 0% Similarity**

```c
// Original Code
#include <stdio.h>
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```
```c
// Variables replaced by single value arrays
#include <stdio.h>
int main() {
    int a[1],b[1],op[1],ans[1];
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op[0]) {
        case 1: ans[0] = a[0]+b[0];
                break;
        case 2: ans[0] = (int) abs(a[0]-b[0]);
                break;
        case 3: ans[0] = a[0]*b[0];
                break;
        case 4: ans[0] = (int) pow(a[0],b[0]);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans[0]);
    return 0;
}
```

## 4. Altering Datatypes

The single more effective way to beat MOSS with minimum effort and liability is to alter variable datatypes. Int to long int, float to double, any such changes throw MOSS off significantly. 

**MOSS Check : 50% Similarity**

```c
// Original Code
#include <stdio.h>
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```
```c
// INT changed to LONG IT
#include <stdio.h>
int main() {
    long int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%ld", &a);
    printf ("Enter second number: ");
    scanf("%ld", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%ld", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (long int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (long int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %ld\n", ans);
    return 0;
}
```

## 5. Exploring New Dimensions

We saw how powerful variable-array conversions can be, but when dealing with arrays, a conversion from arrays to variables is not feasible. We use multidimensional arrays here. The principle is pretty much the same, we express a 1D array with n terms as a 2D array with 1 x n terms. The results is similarly effective. 

**MOSS Check : 14% Similarity**

```c
//Original Code with single point 1D Arrays
#include <stdio.h>
int main() {
    int a[1],b[1],op[1],ans[1];
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op[0]) {
        case 1: ans[0] = a[0]+b[0];
                break;
        case 2: ans[0] = (int) abs(a[0]-b[0]);
                break;
        case 3: ans[0] = a[0]*b[0];
                break;
        case 4: ans[0] = (int) pow(a[0],b[0]);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans[0]);
    return 0;
}
```
```c
// Modified Code with single point 2D Arrays
#include <stdio.h>
int main() {
    int a[1][1],b[1][1],op[1][1],ans[1][1];
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op[0][0]) {
        case 1: ans[0][0] = a[0][0]+b[0][0];
                break;
        case 2: ans[0][0] = (int) abs(a[0][0]-b[0][0]);
                break;
        case 3: ans[0][0] = a[0][0]*b[0][0];
                break;
        case 4: ans[0][0] = (int) pow(a[0][0],b[0][0]);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans[0][0]);
    return 0;
}
```

## 6. Modifying Conditions

By switching or altering the boolean statement on which the condition depends, MOSS can be tricked. If there are multiple conditions involved on the same variable, then using switch-cases can completely obliterate MOSS’s testing. 

**MOSS Check : 34% Similarity**

```c
//Original Code
#include <stdio.h>

int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```
```c
// Switch statement changed to if-else-if steps.

#include <stdio.h>
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    if (op == 1)
        ans = a+b;
    else if (op == 2)
        ans = (int) abs(a-b);
    else if (op == 3)
        ans = a*b;
    else if (op == 4)
        ans = (int) pow(a,b);
    else {
        printf ("\nChoose a Valid Operator\n");
        goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

## 7. Manipulating Loops

Interchanging your while, for and do while loops, with altered iteration conditions, is effective. If done intelligently, loop manipulation can be powerful. In the sample, a single line changed results in MOSS being beaten by over 30%. The best part? The loop variables aren’t even being used, it’s just a infinite run, when the loop is actively being used, the results can be more dramatic. 


**MOSS Check : 68% Similarity**  
A single line changed and similarity reduced by 32%.

```c
//Original Code
#include <stdio.h>

int main() {
    int a;
    printf ("Enter first number: ");
    scanf("%d", &a);
    int b;
    printf ("Enter second number: ");
    scanf("%d", &b);
    int ans;
    while (1) {
        printf("Choose what you want to do:\n1 for a*b\n2 for a+b\n3 for a^b\n4 for a~b\n");
        int op;
        int flag = 0;
        scanf ("%d", &op);
        switch (op) {
            case 3: ans = (int) pow(a,b);
                    break;
            case 1: ans = a*b;
                    break;
            case 4: ans = (int) abs(a-b);
                    break;
            case 2: ans = a+b;
                    break;
            default: printf ("\nChoose a Valid Operator\n");
                    flag = 1;
        }
        if (!flag){
            break;
        }
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```
```c
// Infinite while loop changed to an infinite for loop

#include <stdio.h>
int main() {
    int a;
    printf ("Enter first number: ");
    scanf("%d", &a);
    int b;
    printf ("Enter second number: ");
    scanf("%d", &b);
    int ans;
    for (int i = 0; ; i++) {
        printf("Choose what you want to do:\n1 for a*b\n2 for a+b\n3 for a^b\n4 for a~b\n");
        int op;
        int flag = 0;
        scanf ("%d", &op);
        switch (op) {
            case 3: ans = (int) pow(a,b);
                    break;
            case 1: ans = a*b;
                    break;
            case 4: ans = (int) abs(a-b);
                    break;
            case 2: ans = a+b;
                    break;
            default: printf ("\nChoose a Valid Operator\n");
                    flag = 1;
        }
        if (!flag){
            break;
        }
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

## 8. Modularity

Breaking the functional portions of your code down into modular methods can throw MOSS off balance. Modularity adds a bit of jargon to the code while also changing the workflow, its a very effective method, is rarely detected, and is manually explicable.

**MOSS Check: 21% Similarity**

```c
// Original Code
#include <stdio.h>
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```
```c
// Modular functions added for each operation.
#include <stdio.h>

int add (int a, int b) {
    int c = a+b;
    return c;
}

int difference (int a, int b) {
    int c = (int) abs(a-b);
    return c;
}

int product (int a, int b) {
    int c = a*b;
    return c;
}

int exponential (int a, int b) {
    int c = (int) pow(a,b);
    return c;
}

int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = add(a,b);
                break;
        case 2: ans = difference(a,b);
                break;
        case 3: ans = product(a,b);
                break;
        case 4: ans = exponential(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

## 9. Adding Jargon

Buffing up your code with redundant statements, variable assignments, methods that you’re never going to use are all a part of the second technique, jargon. While jargon in itself will rarely be enough to bypass MOSS, it’s a good supplement to other methods.

**MOSS Check: 24% Similarity**

```c
//Original Code

#include <stdio.h>

int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

```c
// Entire Mergesort funciton hard coded and added to the original code.

#include <stdio.h>
void merge(int arr[], int l, int m, int r)
{
    int i, j, k;
    int n1 = m - l + 1;
    int n2 =  r - m;
    int L[n1], R[n2];
    for (i = 0; i < n1; i++)
        L[i] = arr[l + i];
    for (j = 0; j < n2; j++)
        R[j] = arr[m + 1+ j];
    i = 0; // Initial index of first subarray
    j = 0; // Initial index of second subarray
    k = l; // Initial index of merged subarray
    while (i < n1 && j < n2) {
        if (L[i] <= R[j]) {
            arr[k] = L[i];
            i++;
        }
        else {
            arr[k] = R[j];
            j++;
        }
        k++;
    }
    while (i < n1) {
        arr[k] = L[i];
        i++;
        k++;
    }
    while (j < n2) {
        arr[k] = R[j];
        j++;
        k++;
    }
}
void mergeSort(int arr[], int l, int r) {
    if (l < r) {
        int m = l+(r-l)/2;
        mergeSort(arr, l, m);
        mergeSort(arr, m+1, r);
        merge(arr, l, m, r);
    }
}
void printArray(int A[], int size) {
    int i;
    for (i=0; i < size; i++)
        printf("%d ", A[i]);
    printf("\n");
}
int main() {
    int a,b,op,ans;
    printf ("Enter first number: ");
    scanf("%d", &a);
    printf ("Enter second number: ");
    scanf("%d", &b);
    here:
    printf("Choose what you want to do:\n1 for a+b\n2 for a~b\n3 for a*b\n4 for a^b\n");
    scanf ("%d", &op);
    switch (op) {
        case 1: ans = a+b;
                break;
        case 2: ans = (int) abs(a-b);
                break;
        case 3: ans = a*b;
                break;
        case 4: ans = (int) pow(a,b);
                break;
        default: printf ("\nChoose a Valid Operator\n");
                goto here;
    }
    printf ("The answer is : %d\n", ans);
    return 0;
}
```

Again, this article has been written solely from an academic point of view, but the ease with which this system can be overcome is fascinating. If all these 9 methods *(and possibly others)* are combined, the most complex of codes can be altered such that MOSS can't detect plagiarism.

With great powers come great responsibilities.