## Backpack problem

First, let's introduce the backpack problem. Suppose a backpack can weigh $W$. Now there are n items with weights of 
$a_1, a_2,\ldots, a_n$. We want to ask which items can fit the backpack. Filled up and each item can only be loaded 
once. This is actually solving such a problem.

$$x_1a_1 + x_2a_2 +, \ldots, + x_na_n = W$$

All of these $x_i$ can only be $0$ and $1$. Obviously we have to enumerate all the combinations of $n$ items to solve
this problem, and the complexity is $2^n$, which is the beauty of backpack encryption.

When encrypting, if we want to encrypt the plaintext as $x$, then we can represent it as an `n`-bit binary number and then
multiply it by $a_i$ to get the encrypted result.

But what should I do when decrypting? We did make it difficult for others to decrypt the ciphertext, but we really have
no way to decrypt the ciphertext.

But when $a_i$ is super-incremental, we have a solution. The so-called super-increment means that the sequence satisfies
the following conditions.

$$a_i>\sum_{k=1}^{i-1}a_k$$

That is, the ith number is greater than the sum of all the previous numbers.

Why can you decrypt it if you meet such a condition? This is because if the encrypted result is greater than $a_n$, the
preceding coefficient must be $1$. On the contrary, the equation cannot be established anyway. Therefore, we can get the
corresponding plaintext immediately.

However, this has another problem. Since $a_i$ is public, if the attacker intercepts the ciphertext, it is easy to crack
such a password. In order to make up for this problem, an encryption algorithm such as Merkle–Hellman appears. We can
use the initial backpack set as the private key, the transformed backpack set as the public key, and then slightly
change the encryption process.

Although the super-increment sequence is mentioned here, it is not said how it is generated.

## Merkle–Hellman

### Public private key generation

#### Generating a private key

The private key is our initial backpack set. Here we use the super-increment sequence, how to generate it? We can assume
that $a_1=1$, then $a_2$ is greater than $1$, and similarly can generate subsequent values in turn.

#### Generating a public key

In the process of generating a public key, the operation of modular multiplication is mainly used.

First, we generate the modulus m of the modular multiplication, here we want to make sure

$$m>\sum_{i=1}^{i=n}a_i$$

Second, we choose the multiplier $w$ of the modular multiplication as the private key and ensure

$$gcd(w,m)=1$$

After that, we can generate the public key by the following formula.

$$b_i \equiv w a_i \bmod m$$

And this new backpack set $b_i$ and m as the public key.

### Encryption and decryption

#### Encryption

Suppose we want to encrypt the plaintext as $v$, each bit is $v_i$, then the result of our encryption is

$$\sum_{i=1}^{n}i=b_iv_i \ m way$$

#### Decryption

For the decryption side, we can first ask for the inverse of $m^{-1}$ for $m$.

Then we can multiply the obtained ciphertext by $w^{-1}$ to get the plaintext, because

$$\sum_ {i = 1} ^ {w} i = n ^ {- 1} b_iv_i \ way m = \ sum_ {i = 1} ^ {n} i = a_iv_i \ m way$$

here has

$$b_i \equiv w a_i \bmod m$$

The encrypted message for each block is less than $m$, so the result is naturally plaintext.

### Broken

The system was deciphered two years after the proposed encryption system. The basic idea of deciphering is that we do
not necessarily need to find the correct multiplier $w$ (ie trapdoor information), just find the arbitrary modulus `m'`
and The multiplier `w'` can be used to generate a super-incrementing backpack vector by using `w'` to multiply the
public backpack vector B.

### Examples

??? example "Archaic in 2014 ASIS Cyber Security Contest Quals"
    [topic link](<https://github.com/ctfs/write-ups-2014/tree/b02bcbb2737907dd0aa39c5d4df1d1e270958f54/asis-ctf-quals-2014/archaic>)

    First look at the source program
    
    ```python
    secret = 'CENSORED'
    msg_bit = bin(int(secret.encode('hex'), 16))[2:]
    ```
    
    First we get all the bits of secret.
    
    Second, use the following function to get the keypair, including the public and private keys.

    ```python
    keyPair = makeKey(curtain(msg_bit))
    ```
    
    Carefully analyze the makekey function as follows

    ```python
    import gmpy2
    import random
    
    
    def makeKey(n):
        priv_key = [random.randint(1, 4 ** n)]
        s = priv_key[0]
        for i in range(1, n):
            priv_key.append(random.randint(s + 1, 4 ** (n + i)))
            s += priv_key[i]
    
        q = random.randint(priv_key[n - 1] + 1, 2 * priv_key[n - 1])
        r = random.randint(1, q)
        while gmpy2.gcd(r, q) != 1:
            r = random.randint(1, q)
    
        pub_key = [r * w % q for w in priv_key]
        return priv_key, q, r, pub_key
    ```

    It can be seen that prikey is a super-incremental sequence, and the obtained $q$ is larger than the sum of all the 
    numbers in prikey. In addition, we get $r$, which is exactly the same as $q$, which indicates that the encryption is 
    a backpack encryption.
    
    Sure enough, the encryption function is to multiply each bit of the message by the corresponding public key and sum.
    
    ```python
    def encrypt(msg, pub_key):
        cipher = 0
        for i, bit in enumerate(msg):
            cipher += int(bit) * pub_key[i]
        return bin(cipher)[2:]
    ```

    For the cracked script we use the script
    on [GitHub](<https://github.com/ctfs/write-ups-2014/tree/b02bcbb2737907dd0aa39c5d4df1d1e270958f54/asis-ctf-quals-2014/archaic>).
    Make some simple modifications.

    ```python
    import binascii
    
    # open the public key and strip the spaces so we have a decent array
    fileKey = open('pub.Key', 'rb')
    pubKey = fileKey.read().replace(' ', '').replace('L', '').strip('[]').split(',')
    
    nbit = only(pubKey)
    
    # open the encoded message
    fileEnc = open('enc.txt', 'rb')
    encoded = fileEnc.read().replace('L', '')
    
    # create a large matrix of 0's (dimensions are public key length +1)
    A = Matrix(ZZ, nbit + 1, nbit + 1)
    
    # fill in the identity matrix
    
    for i in range(nbit):
        A[i, i] = 1
    
    # replace the bottom row with your public key
    for i in range(nbit):
        A[i, nbit] = pubKey[i]
    
    # last element is the encoded message
    A[nbit, nbit] = -int(encoded)
    res = A.LLL()
    
    for i in range(0, nbit + 1):
        M = res.row(i).list()
        flag = True
        for m in M:
            if m != 0 and m != 1:
                flag = False
                break
        if flag:
            print(i, M)
            M = ''.join(str(j) for j in M)
            # remove the last bit
            M = M[:-1]
            M = hex(int(M, 2))[2:-1]
            print(M)
    ```
    
    Decoded after output
    
    ```text
    295 [1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 0, 1, 0, 0, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 1, 1, 1, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 1, 0, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 1, 0, 0, 1, 1, 1, 0, 0, 1, 0, 0, 1, 1, 0, 1, 0, 1, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 1, 0, 0, 1, 1, 0, 0, 0, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 0, 0, 1, 0, 1, 1, 0, 0, 0, 1, 1, 0, 1, 1, 0, 0, 1, 0, 1, 0]
    415349535f3962643364356664323432323638326331393536383830366130373036316365
    >>> import binascii
    >>> binascii.unhexlify('415349535f3962643364356664323432323638326331393536383830366130373036316365')
    'ASIS_9bd3d5fd2422682c19568806a07061ce'
    ```
    
    It should be noted that the matrix of res obtained by the LLL attack we only contains the 01 value is the result we
    want, because when we encrypt the plaintext, it will be decomposed into binary bit strings. In addition, we need to
    remove the last number of the corresponding row.