# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC
## NAME: SHALINI N
## REG NO: 212224040305

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:

```
#include <stdio.h>

typedef struct
{
    int x, y;
} Point;

int mod(int a, int p)
{
    return (a % p + p) % p;
}

/* Point addition: R = P + Q */
Point add(Point P, Point Q, int a, int p)
{
    Point R;
    int m;

    if (P.x == Q.x && P.y == Q.y)
        m = mod((3 * P.x * P.x + a) * 1, p);
    else
        m = mod((Q.y - P.y), p);

    /* Simple inverse search */
    for (int i = 1; i < p; i++)
    {
        if (mod(m * i, p) == 1)
        {
            m = i;
            break;
        }
    }

    if (P.x != Q.x)
        m = mod((Q.y - P.y) * m, p);
    else
        m = mod((3 * P.x * P.x + a) * m, p);

    R.x = mod(m * m - P.x - Q.x, p);
    R.y = mod(m * (P.x - R.x) - P.y, p);

    return R;
}

/* Scalar multiplication: kG */
Point multiply(Point G, int k, int a, int p)
{
    Point R = G;

    for (int i = 1; i < k; i++)
        R = add(R, G, a, p);

    return R;
}

int main()
{
    int a = 2, b = 3, p = 97;
    int dA = 5, dB = 7;

    Point G = {3, 6};

    Point QA = multiply(G, dA, a, p);
    Point QB = multiply(G, dB, a, p);

    printf("Elliptic Curve: y^2 = x^3 + %dx + %d mod %d\n", a, b, p);

    printf("\nBase Point G = (%d, %d)", G.x, G.y);

    printf("\n\nAlice Private Key = %d", dA);
    printf("\nAlice Public Key = (%d, %d)", QA.x, QA.y);

    printf("\n\nBob Private Key = %d", dB);
    printf("\nBob Public Key = (%d, %d)", QB.x, QB.y);

    /* Shared secret */
    Point S1 = multiply(QB, dA, a, p);
    Point S2 = multiply(QA, dB, a, p);

    printf("\n\nShared Secret (Alice) = (%d, %d)", S1.x, S1.y);
    printf("\nShared Secret (Bob) = (%d, %d)\n", S2.x, S2.y);

    return 0;
}
```

## Output:

<img width="602" height="487" alt="image" src="https://github.com/user-attachments/assets/e81f7a9a-f139-434e-a2c3-c1dbec86d961" />


## Result:
The program is executed successfully

