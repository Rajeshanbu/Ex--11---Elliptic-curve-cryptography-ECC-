# Ex--11---Elliptic-curve-cryptography(ECC):
## Aim:
To implement Elliptic Curve Cryptography (ECC) for a practical application like key exchange between two parties, Alice and Bob.

## ALGORITHM:
### Step1:
Elliptic Curve Equation: ECC uses the equation y² = x³ + ax + b over a finite field defined by a prime number p.
### Step2:
Point Addition and Doubling: ECC performs operations like point addition and point doubling on points that lie on the elliptic curve.
### Step3:
Scalar Multiplication: ECC computes scalar multiplication (repeated addition) of a point, which is essential for key exchange.
### Step4:
Shared Secret: The final shared secret is generated through scalar multiplication of public keys and private keys.
## PROGRAM:
```py
# A simple class to represent points on the elliptic curve
class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y

# Function to compute modular inverse (using Extended Euclidean Algorithm)
def modInverse(a, m):
    m0, x0, x1 = m, 0, 1
    if m == 1:
        return 0
    while a > 1:
        q = a // m
        a, m = m, a % m
        x0, x1 = x1 - q * x0, x0
    return x1 + m0 if x1 < 0 else x1

# Function to perform point addition on elliptic curves
def pointAddition(P, Q, a, p):
    # Check if P == Q (Point doubling)
    if P.x == Q.x and P.y == Q.y:
        lambda_val = (3 * P.x * P.x + a) * modInverse(2 * P.y, p) % p
    else:  # Point addition
        lambda_val = (Q.y - P.y) * modInverse(Q.x - P.x, p) % p

    # Calculate resulting point
    x_r = (lambda_val * lambda_val - P.x - Q.x) % p
    y_r = (lambda_val * (P.x - x_r) - P.y) % p

    # Ensure values are positive
    return Point((x_r + p) % p, (y_r + p) % p)

# Function to perform scalar multiplication (Elliptic Curve Point Multiplication)
def scalarMultiplication(P, k, a, p):
    result = P
    k -= 1  # Subtract 1 because we start with the base point

    while k > 0:
        result = pointAddition(result, P, a, p)  # Add the point to itself (k times)
        k -= 1

    return result

def main():
    p = int(input("Enter the prime number (p): "))
    a = int(input("Enter the curve parameter a for equation y^2 = x^3 + ax + b: "))
    b = int(input("Enter the curve parameter b for equation y^2 = x^3 + ax + b: "))

    Gx = int(input("Enter the base point G (x): "))
    Gy = int(input("Enter the base point G (y): "))
    G = Point(Gx, Gy)

    privateA = int(input("Enter Alice's private key: "))
    privateB = int(input("Enter Bob's private key: "))

    # Compute public keys (Elliptic Curve Point Multiplication)
    publicA = scalarMultiplication(G, privateA, a, p)  # Alice's public key
    publicB = scalarMultiplication(G, privateB, a, p)  # Bob's public key

    print(f"Alice's public key: ({publicA.x}, {publicA.y})")
    print(f"Bob's public key: ({publicB.x}, {publicB.y})")

    # Compute shared secrets (Elliptic Curve Point Multiplication)
    sharedSecretA = scalarMultiplication(publicB, privateA, a, p)  # Alice's shared secret
    sharedSecretB = scalarMultiplication(publicA, privateB, a, p)  # Bob's shared secret

    # Display shared secret
    print(f"Shared secret computed by Alice: ({sharedSecretA.x}, {sharedSecretA.y})")
    print(f"Shared secret computed by Bob: ({sharedSecretB.x}, {sharedSecretB.y})")

    if sharedSecretA.x == sharedSecretB.x and sharedSecretA.y == sharedSecretB.y:
        print("Key exchange successful. Both shared secrets match!")
    else:
        print("Key exchange failed. Shared secrets do not match.")

if __name__ == "__main__":
    main()
```
## Output:
![image](https://github.com/user-attachments/assets/4e23da40-bd1b-404f-ae94-413265cfb7ad)
## Result:
Hence, the implementation of Elliptic Curve Cryptography (ECC) for a practical application like key exchange between two parties, Alice and Bob, has been successfully completed.
