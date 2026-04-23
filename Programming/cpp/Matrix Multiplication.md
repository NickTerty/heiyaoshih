### 定義
矩陣乘法是利用兩個矩陣相乘得到另一個不同的矩陣，假設 $A$ 是一個 $m\times k$ 矩陣， $B$ 是一個 $k \times n$ 矩陣，當兩個矩陣相乘會得到一個 $m\times n$ 的矩陣。
$$\begin{align}
AB&=\begin{bmatrix}
a_{11} &a_{12} &\cdots &a_{1k}\\
a_{21} &a_{22} &\cdots &a_{2k}\\
\vdots &\vdots &\ddots &\vdots\\
a_{m1} &a_{m2} &\cdots &a_{mk}\\
\end{bmatrix}\begin{bmatrix}
b_{11} &b_{12} &\cdots &b_{1n}\\
a_{21} &a_{22} &\cdots &b_{2n}\\
\vdots &\vdots &\ddots &\vdots\\
b_{k1} &b_{k2} &\cdots &b_{kn}\\
\end{bmatrix}
\end{align} $$
其中矩陣元素
$$(AB)_{ij}=\sum_{i=1}^n a_{in}b_{nj}$$
但要注意的是，矩陣乘法沒有交換率，$AB\neq BA$。
### 我們要做什麼
現在我們換成利用C++，去寫出一個矩陣乘法計算機的程式。假設最高矩陣只能 $10\times10$，然後我們需要一個讓使用者輸入兩個矩陣，同時要防止出錯，以下是我們要防止的。
1. 矩陣 $A$ 列值以及矩陣 $B$ 行值不相等
2. 輸入行或列的值出錯

#### Sample Run:
```
Welcome to the calculator of matrix multiplication, we will ask you to input two rows and columns of matrix, and all elements one by one.
Your input should be integer, and any row or column can't be greater than 10, 0 or negative integer.
If you are calculate for square matrices(n x n), check your matrices because the matrix multiplication doesn't have commutative, AB is not equal to BA.

Input first matrix for m by n => 2 3
Input second matrix for m by n => 3 1
Enter the first matrix(Suggestion: max column that you input for every row)
1 2 3
4 5 6
Enter the second matrix(Suggestion: max column that you input for every row)
1
2
3
The matrix multiplication is
14 
32 
Do you want to try another matrix multiplication? (1 for yes, others for no) => 0
```

```
Welcome to the calculator of matrix multiplication, we will ask you to input two rows and columns of matrix, and all elements one by one.
Your input should be integer, and any row or column can't be greater than 10, 0 or negative integer.
If you are calculate for square matrices(n x n), check your matrices because the matrix multiplication doesn't have commutative, AB is not equal to BA.

Input first matrix for m by n => 3 4
Input second matrix for m by n => 4 3
Enter the first matrix(Suggestion: max column that you input for every row)
1 2 3 4
4 3 2 1
1 1 1 1
Enter the second matrix(Suggestion: max column that you input for every row)
1 1 1
2 2 2
3 3 3
-1 -1 -1
The matrix multiplication is
10 10 10
15 15 15
5 5 5
Do you want to try another matrix multiplication? (1 for yes, others for no) => 1
Input first matrix for m by n => 3 3
Input second matrix for m by n => 3 3
Enter the first matrix(Suggestion: max column that you input for every row)
1 0 0
0 1 0
0 0 1
Enter the second matrix(Suggestion: max column that you input for every row)
-3 10 2
2 -7 19
3 6 9
The matrix multiplication is
-3 10 2
2 -7 19
3 6 9
Do you want to try another matrix multiplication? (1 for yes, others for no) => 1
Input first matrix for m by n => 3 -1
Input second matrix for m by n => -1 3
The input is not valid!
Do you want to try another matrix multiplication? (1 for yes, others for no) => -1
```

Solution:
```cpp
#include<iostream>
using namespace std;

const int MAX_N = 10;

// Function as matrix calculator
void matrixInput(int matrix[][MAX_N], int num[2]){
    for(int i = 0; i < num[0]; i++){
        for(int j = 0; j < num[1]; j++){
            cin >> matrix[i][j];
        }
    }
}

void matrixCal(int AB[][MAX_N], int A[][MAX_N], int B[][MAX_N], int multNum){
    for(int i = 0; i < MAX_N; i++){
        for(int j = 0; j < MAX_N; j++){
            for(int k = 0; k < multNum; k++){
                AB[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}

void matrixPrint(int AB[][MAX_N], int num[2]){
    cout << "The matrix multiplication is" << endl;
    for(int i = 0; i < num[0]; i++){
        for (int j = 0; j < num[1]; j++){
            cout << AB[i][j] << " ";
        }
        cout << endl;
    }
}

void instructionDisplay(void){
    cout << "Welcome to the calculator of matrix multiplication, we will ask you to input two rows and columns of matrix, and all elements one by one." << endl;
    cout << "Your input should be integer, and any row or column can't be greater than 10, 0 or negative integer." << endl;
    cout << "If you are calculate for square matrices(n x n), check your matrices because the matrix multiplication doesn't have commutative, AB is not equal to BA." << endl << endl;
}

int main(){
    //display the instruction
    instructionDisplay();

    while (1){
        //input
        int A[MAX_N][MAX_N] = {0}, B[MAX_N][MAX_N] = {0};
        int matrixA_Num[2] = {0}, matrixB_Num[2] = {0};

        //output
        int AB[MAX_N][MAX_N] = {0};

        //input number of matrix
        cout << "Input first matrix for m by n => ";
        for(int i = 0; i < 2; i++){
            cin >> matrixA_Num[i];
        }

        cout << "Input second matrix for m by n => ";
        for(int i = 0; i < 2; i++){
            cin >> matrixB_Num[i];
        }
        
        // Determine the n of matrix A and the m of matrix B is equal,
        // if not, stop the matrix calculation.
        if(matrixA_Num[1] != matrixB_Num[0]){
            cout << "The input is not valid!" << endl;
        }
        else if(matrixA_Num[0] <= 0 || matrixA_Num[0] > 10 ||
                matrixA_Num[1] <= 0 || matrixA_Num[1] > 10 ||
                matrixB_Num[0] <= 0 || matrixB_Num[0] > 10 ||
                matrixB_Num[1] <= 0 || matrixB_Num[1] > 10 ){
            cout << "The input is not valid!" << endl;
        }
        else{
            int multNum = matrixA_Num[1];
            int matrixAB_Num[2] = {matrixA_Num[0], matrixB_Num[1]};

            cout << "Enter the first matrix(Suggestion: max column that you input for every row)" << endl;
            matrixInput(A, matrixA_Num);

            cout << "Enter the second matrix(Suggestion: max column that you input for every row)" << endl;
            matrixInput(B, matrixB_Num);

            //Calculate
            matrixCal(AB, A, B, multNum);

            //output the matrix
            matrixPrint(AB, matrixAB_Num);
        }

        int yes;
        cout << "Do you want to try another matrix multiplication? (1 for yes, others for no) => ";
        cin >> yes;
        if(yes != 1) break;
    }
    
    return 0;
}
```