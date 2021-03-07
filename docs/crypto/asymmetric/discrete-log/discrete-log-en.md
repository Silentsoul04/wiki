[RU](./discrete-log.md) | [EN](./discrete-log-en.md)

# Discrete logarithm

## Basic definition

When we understand discrete logarithms, let's first look at a few basic definitions.

**Definition 1**

In group G, g is the generator of G, that is, each element in group G can be written as <img src="https://render.githubusercontent.com/render/math?math=y=g^k">, which we call k is the logarithm of y in group G.

**Definition 2**

Let <img src="https://render.githubusercontent.com/render/math?math=m \geq 1">, <img src="https://render.githubusercontent.com/render/math?math=(a,m)=1">, let <img src="https://render.githubusercontent.com/render/math?math=a^d \equiv 1 \pmod m"> be the smallest positive integer d called a to the exponent or order of modulo m, we will generally It is recorded as <img src="https://render.githubusercontent.com/render/math?math=\delta_m(a)">.

**Definition 3**

When <img src="https://render.githubusercontent.com/render/math?math=\delta_m(a)=\varphi(m)">, a is called the original root of modulo m, which is called the original root of m.

## Some properties

**Property 1**

The smallest positive integer <img src="https://render.githubusercontent.com/render/math?math=d"> that makes <img src="https://render.githubusercontent.com/render/math?math=a^d \equiv 1 \pmod m"> true must have <img src="https://render.githubusercontent.com/render/math?math=d \mid \varphi(m)">.

**Property 2**

The necessary and sufficient condition for the existence of the original root of the modulo <img src="https://render.githubusercontent.com/render/math?math=m"> is <img src="https://render.githubusercontent.com/render/math?math=m=2,4,p^{\alpha}, 2p^{\alpha}">, where <img src="https://render.githubusercontent.com/render/math?math=p"> is an odd prime, <img src="https://render.githubusercontent.com/render/math?math=\alpha"> is positive Integer.

## Discrete logarithm problem

Knowing <img src="https://render.githubusercontent.com/render/math?math=g,p,y">, for the equation <img src="https://render.githubusercontent.com/render/math?math=y \equiv g^x \pmod p">, solving <img src="https://render.githubusercontent.com/render/math?math=x"> is a difficult problem. But when <img src="https://render.githubusercontent.com/render/math?math=p"> has certain characteristics, it can be solved. For example, the order of this group is a smooth number.

It is this problem that constitutes a large part of modern cryptography, including Diffie–Hellman key exchange, ElGamal algorithm, ECC, etc.

## Discrete logarithm solution

### Violent

Given <img src="https://render.githubusercontent.com/render/math?math=y \equiv g^x \pmod p">, we can violently enumerate <img src="https://render.githubusercontent.com/render/math?math=x"> to get the true value of <img src="https://render.githubusercontent.com/render/math?math=x">.

### Baby-step giant-step

This method is often referred to as a small step, which uses the idea of an intermediate encounter attack.

We can make <img src="https://render.githubusercontent.com/render/math?math=x=im+j">, where <img src="https://render.githubusercontent.com/render/math?math=m= \lceil \sqrt n \rceil">, then the integers i and j are all in the range 0 to m.

Therefore <img src="https://render.githubusercontent.com/render/math?math=y = g ^ x = g ^ {im + j}"> 

That is <img src="https://render.githubusercontent.com/render/math?math=y(g ^ {-m}) ^ i = g ^ j">

Then we can enumerate all the j and calculate it and store it in a set S, then we enumerate i again, calculate <img src="https://render.githubusercontent.com/render/math?math=y(g ^ {-m}) ^ i">, once we find the calculation The result in the set S shows that we got a collision and got i and j.

This is obviously a way of compromise between time and space. We convert a <img src="https://render.githubusercontent.com/render/math?math=O(n)"> time complexity, <img src="https://render.githubusercontent.com/render/math?math=O(1)"> space complexity algorithm to a <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt n)"> The algorithm of time complexity and space complexity of <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt n)">.

Among them

- Each increment of j means "baby-step", multiplied by <img src="https://render.githubusercontent.com/render/math?math=g"> at a time.
- Each increment of i means "giant-step" , multiplied by <img src="https://render.githubusercontent.com/render/math?math=g ^ {-m}"> at a time.

```python
import math


def bsgs(g, y, p):
    m = int(math.ceil(math.sqrt(p - 1)))
    S = {pow(g, j, p): j for j in range(m)}
    gs = pow(g, p - 1 - m, p)
    for i in range(m):
        if y in S:
            return i * m + S[y]
        y = y * gs % p
    return None
```

### Pollard's ρ algorithm

We can solve the above problem with the time complexity of <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt n)"> and the space complexity of <img src="https://render.githubusercontent.com/render/math?math=O(1)">. Please use Google for your specific principles.

### Pollard's kangaroo algorithm

If we know that the range of x is <img src="https://render.githubusercontent.com/render/math?math=a \leq x \leq b">, then we can solve the above problem with the time complexity of <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt{ba})">. Please use Google for your specific principles.

### Pohlig-Hellman algorithm

Let's assume that the above mentioned group has a rank of <img src="https://render.githubusercontent.com/render/math?math=n"> for the element <img src="https://render.githubusercontent.com/render/math?math=g">, and <img src="https://render.githubusercontent.com/render/math?math=n"> is a smooth number: <img src="https://render.githubusercontent.com/render/math?math=n=\prod_{i=1}^r p_i^{e_i}">.

1. For each <img src="https://render.githubusercontent.com/render/math?math=i \in \{1,\ldots,r\}">:
2. Calculate <img src="https://render.githubusercontent.com/render/math?math=g_i \equiv g ^ {n / p_i ^ {e_i}} \pmod m">. According to the Lagrange theorem, the order of <img src="https://render.githubusercontent.com/render/math?math=g_i"> in the group is <img src="https://render.githubusercontent.com/render/math?math=p_i ^ {e_i}">.
3. Calculate <img src="https://render.githubusercontent.com/render/math?math=y_i \equiv y ^ {n / p_i ^ {e_i}} \equiv g ^ {xn / p_i ^ {e_i}} \equiv g_i ^ {x} \equiv g_i ^ {x \bmod p_i ^ {e_i}} \equiv g_i ^ {x_i} \pmod m">, here we know <img src="https://render.githubusercontent.com/render/math?math=y_i,m,g_i">, and <img src="https://render.githubusercontent.com/render/math?math=x_i"> ranges from <img src="https://render.githubusercontent.com/render/math?math=[0,p_i ^ {e_i})">, by <img src="https://render.githubusercontent.com/render/math?math=n"> is a Smooth numbers, the range is small, so we can quickly find <img src="https://render.githubusercontent.com/render/math?math=x_i"> using methods such as *Pollard's kangaroo algorithm*.
4. According to the above derivation, we can get for <img src="https://render.githubusercontent.com/render/math?math=i \in \{1,\ldots,r\}">, <img src="https://render.githubusercontent.com/render/math?math=x \equiv x_i \pmod {p_i ^ {e_i}}"> , which can be solved by Chinese remainder theorem.

The above process can be briefly described in the following figure:

![Pohlig Hellman Algorithm](../img/Pohlig-Hellman-Diagram.png)

The complexity is <img src="https://render.githubusercontent.com/render/math?math=O \left (\sum_i e_i \left ( \log n+ \sqrt {p_i} \right ) \right )">, and it can be seen that the complexity is still very low.

But when <img src="https://render.githubusercontent.com/render/math?math=n"> is prime, <img src="https://render.githubusercontent.com/render/math?math=m=2n+1">, then the complexity and <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt m)"> are almost indistinguishable.

## 2018 National Championship crackme java

Code show as below

```java
import java.math.BigInteger;
import java.util.Random;


public class Test1 {
    static BigInteger two = new BigInteger("2");
    static BigInteger p = new BigInteger("11360738295177002998495384057893129964980131806509572927886675899422214174408333932150813939357279703161556767193621832795605708456628733877084015367497711");
    static BigInteger h = new BigInteger("7854998893567208831270627233155763658947405610938106998083991389307363085837028364154809577816577515021560985491707606165788274218742692875308216243966916");

    /*
        Alice write the below algorithm for encryption.
        The public key {p, h} is broadcasted to everyone.
        @param val: The plaintext to encrypt.
        We suppose val only contains lowercase letter {a-z} and numeric charactors, and is at most 256 charactors in length.
    */
    public static String pkEnc(String val){
        BigInteger[] ret = new BigInteger[2];
        BigInteger bVal = new BigInteger(val.toLowerCase(), 36);
        BigInteger r = new BigInteger(new Random().nextInt() + "");
        ret[0] = two.modPow(r, p);
        entitled[1] = h.modPow(r, p).multiply(bVal);
        return right[0].toString(36) + "==" + ret[1].toString(36);
    }

    /* 
        Alice write the below algorithm for decryption. x is her private key, which she will never let you know.

    public static String skDec(String val, BigInteger x){
        if(!val.contains("==")){
            return null;
        }
        else {
            BigInteger val0 = new BigInteger(val.split("==")[0], 36);
            BigInteger val1 = new BigInteger(val.split("==")[1], 36);
            BigInteger s = val0.modPow(x,p).modInverse(p);
            return val1.multiply(s).mod(p).toString(36);
        }
    }
    */


    public static void main(String[] args) throws Exception {
        System.out.println("You intercepted the following message, which is sent from Bob to Alice:");
        BigInteger bVal1 = new BigInteger("a9hgrei38ez78hl2kkd6nvookaodyidgti7d9mbvctx3jjniezhlxs1b1xz9m0dzcexwiyhi4nhvazhhj8dwb91e7lbbxa4ieco", 36);
        BigInteger bVal2 = new BigInteger("2q17m8ajs7509yl9iy39g4znf08bw3b33vibipaa1xt5b8lcmgmk6i5w4830yd3fdqfbqaf82386z5odwssyo3t93y91xqd5jb0zbgvkb00fcmo53sa8eblgw6vahl80ykxeylpr4bpv32p7flvhdtwl4cxqzc", 36);
        BigInteger r = new BigInteger(new Random().nextInt() + "");
        System.out.println(r);
        System.out.println(bVal1);
        System.out.println(bVal2);
        System.out.println("a9hgrei38ez78hl2kkd6nvookaodyidgti7d9mbvctx3jjniezhlxs1b1xz9m0dzcexwiyhi4nhvazhhj8dwb91e7lbbxa4ieco==2q17m8ajs7509yl9iy39g4znf08bw3b33vibipaa1xt5b8lcmgmk6i5w4830yd3fdqfbqaf82386z5odwssyo3t93y91xqd5jb0zbgvkb00fcmo53sa8eblgw6vahl80ykxeylpr4bpv32p7flvhdtwl4cxqzc");
        System.out.println("Please figure out the plaintext!");
    }
}
```

It can be found that the range of r is <img src="https://render.githubusercontent.com/render/math?math=[0, 2 ^ {32})">, so we can use the BSGS algorithm as follows

```python
from sage.all import *


c1 = int(
    'a9hgrei38ez78hl2kkd6nvookaodyidgti7d9mbvctx3jjniezhlxs1b1xz9m0dzcexwiyhi4nhvazhhj8dwb91e7lbbxa4ieco',
    36
)
c2 = int(
    '2q17m8ajs7509yl9iy39g4znf08bw3b33vibipaa1xt5b8lcmgmk6i5w4830yd3fdqfbqaf82386z5odwssyo3t93y91xqd5jb0zbgvkb00fcmo53sa8eblgw6vahl80ykxeylpr4bpv32p7flvhdtwl4cxqzc',
    36
)
print(c1, c2)
p = 11360738295177002998495384057893129964980131806509572927886675899422214174408333932150813939357279703161556767193621832795605708456628733877084015367497711
h = 7854998893567208831270627233155763658947405610938106998083991389307363085837028364154809577816577515021560985491707606165788274218742692875308216243966916
const2 = 2
const2 = Mod(const2, p)
c1 = Mod(c1, p)
c2 = Mod(c2, p)
h = Mod(h, p)
print('2', bsgs(const2, c1, bounds=(1, 2 ^ 32)))
r = 152351913
num = long(c2 / (h ** r))
```

## Reference

- Elementary number theory, Pan Chengdong, Pan Chengyu
- <https://ee.stanford.edu/~hellman/publications/28.pdf>
- <https://en.wikipedia.org/wiki/Pohlig%E2%80%93Hellman_algorithm#cite_note-Menezes97p108-2>
- <https://fortenf.org/e/crypto/2017/12/03/survey-of-discrete-log-algos.html>