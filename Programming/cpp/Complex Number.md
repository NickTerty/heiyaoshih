The **complex numbers**(複數) are defined as
$$\mathbb{C}=\{a+bi \mid a,b\in\mathbb{R}\},$$
where $i^2=-1$. If $z = a + bi$, then $a$ is the **real part**(實部) of $z$ and $b$ is the **imaginary part**(虛部) of $z$.

To add two complex numbers $z = a + bi$ and $w = c + di$, we just add the corresponding real and imaginary parts: 
$$z + w = (a + bi) + (c + di) = (a + c) + (b + d)i.$$
Remembering that $i^2=-1$, we multiply complex numbers just like polynomials. The product of $z$ and $w$ is
$$(a + bi)(c + di)=ac+bdi^2+adi+bci=(ac-bd)+(ad+bc)i.$$

The norm of complex number, which mean the absolute value of complex number, is defined as
$$
|a+bi|=\sqrt{a^2+b^2}.
$$

Now, design the program that using the `class` to define the new type `Complex` as below:
```
class Complex{
    int real;
    int image;
public:
    Complex();
    //~Complex();
    Complex(int realInp, int imageInp);
    Complex(const Complex &c);
};
```
Declare below member functions,
- `void complexAdd(Complex c1, Complex c2);` as adding two complex numbers.
- `void complexMin(Complex c1, Complex c2);` as $z - w = (a + bi) - (c + di)$.
- `void complexMult(Complex c1, Complex c2);` to multiply complex numbers.
- `double complexNorm();` to compute the norm(absolute value) of complex number.
- `void complexPrint();` to print out the complex number as $a+bi$.

In main function, you should make user input four numbers as two complex numbers, then print out the result of adding, substruction, multiplication and norm of two complex numbers, the sample run should be like this.
Input:
```
3 4 5 12
```
Output:
```
8 + 16i
-2 + -8i
-33 + 56i
|3 + 4i| = 5
|5 + 12i| = 13
```

Solution:
```cpp
#include<iostream>
#include<cmath>
using namespace std;

class Complex{
private:
    int real;
    int image;

public:
    Complex();
    //~Complex();
    Complex(int realInp, int imageInp);
    Complex(const Complex &c);
    // Member Functions
    void complexAdd(Complex c1, Complex c2);
    void complexMin(Complex c1, Complex c2);
    void complexMult(Complex c1, Complex c2);
    double complexNorm();
    void complexPrint();
};

Complex::Complex(){
    this->real = this->image = 0;
}

Complex::Complex(int realInp, int imageInp){
    this->real = realInp;
    this->image = imageInp;
}

Complex::Complex(const Complex &c){
    this->real = c.real;
    this->image = c.image;
}

void Complex::complexAdd(Complex c1, Complex c2){
    this->real = c1.real + c2.real;
    this->image = c1.image + c2.image;
}

void Complex::complexMin(Complex c1, Complex c2){
    this->real = c1.real - c2.real;
    this->image = c1.image - c2.image;
}

void Complex::complexMult(Complex c1, Complex c2){
    /*
    (a+bi)(c+di)=ac+adi+bci-bd=(ac-bd)+(ad+bc)i
    */
    this->real = c1.real * c2.real - c1.image * c2.image;
    this->image = c1.real * c2.image + c1.image * c2.real;
}

double Complex::complexNorm(){
    double norm = sqrt(pow(real, 2) + pow(image, 2));
    return norm;
}

void Complex::complexPrint(){
    cout << real << " + " << image << "i";
}

void endline(){
    cout << endl;
}

int main(){
    int real[3], image[3];
    for (int i = 0; i < 2; i++){
        cin >> real[i] >> image[i];
    }
    real[2] = image[2] = 0;
    Complex **complexPtr = new Complex*[3];
    for (int i = 0; i < 3; i++){
        complexPtr[i] = new Complex(real[i], image[i]);
    }

    // Add
    complexPtr[2]->complexAdd(*complexPtr[0], *complexPtr[1]);
    complexPtr[2]->complexPrint();
    endline();

    // Min
    complexPtr[2]->complexMin(*complexPtr[0], *complexPtr[1]);
    complexPtr[2]->complexPrint();
    endline();

    // Mult
    complexPtr[2]->complexMult(*complexPtr[0], *complexPtr[1]);
    complexPtr[2]->complexPrint();
    endline();

    // Norm
    for (int i = 0; i < 2; i++){
        double norm = complexPtr[i]->complexNorm();
        cout << "|";
        complexPtr[i]->complexPrint();
        cout << "| = " << norm;
        endline();
    }

    // delete
    for (int i = 0; i < 3; i++){
        delete complexPtr[i];
    }
    delete[] complexPtr;

    return 0;
}
```