[RU](./discrete-log.md) | [EN](./discrete-log-en.md)

# Дискретный логарифм

## Основное определение

Когда мы поймем дискретные логарифмы, давайте сначала рассмотрим несколько основных определений.

**Определение 1**

В группе G, g является генератором G, то есть каждый элемент в группе G может быть записан как <img src="https://render.githubusercontent.com/render/math?math=y=g^k">, которую мы называем k логарифмом y в группе G.

**Определение 2**

Пусть <img src="https://render.githubusercontent.com/render/math?math=m \geq 1">, <img src="https://render.githubusercontent.com/render/math?math=(a,m)=1">, пусть <img src="https://render.githubusercontent.com/render/math?math=a^d \equiv 1 \pmod m"> будет наименьшим положительным целым числом d, называемым а до степени или порядка по модулю m, мы обычно будем записывать это как <img src="https://render.githubusercontent.com/render/math?math=\delta_m(a)">.

**Определение 3**

Когда <img src="https://render.githubusercontent.com/render/math?math=\delta_m(a)=\varphi(m)">, a называется исходным корнем по модулю m, который называется исходным корнем из m.

## Некоторые свойства

**Свойство 1**

Наименьшее положительное целое число <img src="https://render.githubusercontent.com/render/math?math=d"> , которое делает <img src="https://render.githubusercontent.com/render/math?math=a^d \equiv 1 \pmod m"> истинным, должно иметь <img src="https://render.githubusercontent.com/render/math?math=d \mid \varphi(m)">.

**Свойство 2**

Необходимым и достаточным условием существования исходного корня по модулю <img src="https://render.githubusercontent.com/render/math?math=m"> является<img src="https://render.githubusercontent.com/render/math?math=m=2,4,p^{\alpha}, 2p^{\alpha}">, где <img src="https://render.githubusercontent.com/render/math?math=p"> - нечетное простое число, <img src="https://render.githubusercontent.com/render/math?math=\alpha"> положительное целое число.

## Проблема дискретного логарифмирования

Зная <img src="https://render.githubusercontent.com/render/math?math=g,p,y"> уравнения <img src="https://render.githubusercontent.com/render/math?math=y \equiv g^x \pmod p">, найти решение <img src="https://render.githubusercontent.com/render/math?math=x"> - трудная задача. Но когда <img src="https://render.githubusercontent.com/render/math?math=p"> есть определенные характеристики, это решаемо. Например, порядок этой группы - плавное число.

Именно эта проблема составляет большую часть современной криптографии, включая обмен ключами Диффи – Хеллмана, алгоритм Эль-Гамаля, ECC и тд.

## Решение с дискретным логарифмом

### Жестокий

Учитывая <img src="https://render.githubusercontent.com/render/math?math=y \equiv g^x \pmod p">, что мы можем резко перечислить <img src="https://render.githubusercontent.com/render/math?math=x">, чтобы получить истинное значение <img src="https://render.githubusercontent.com/render/math?math=x">.

### Бэби-степ гигантский шаг

Этот метод часто называют малым шагом, в котором используется идея промежуточной встречной атаки.

Мы можем сделать <img src="https://render.githubusercontent.com/render/math?math=x=im+j">, где <img src="https://render.githubusercontent.com/render/math?math=m= \lceil \sqrt n \rceil">, тогда целые числа i и j находятся в диапазоне от 0 до m.

Следовательно <img src="https://render.githubusercontent.com/render/math?math=y = g ^ x = g ^ {im + j}"> 

То есть <img src="https://render.githubusercontent.com/render/math?math=y(g ^ {-m}) ^ i = g ^ j">

Затем мы можем перечислить все j, вычислить его и сохранить в наборе S, затем мы снова перечислим i, вычислим <img src="https://render.githubusercontent.com/render/math?math=y(g ^ {-m}) ^ i">, как только найдем вычисление. Результат в наборе S показывает, что мы получили коллизию и получили i и j.

Очевидно, что это способ компромисса между временем и пространством. Мы преобразовываем алгоритм временной сложности <img src="https://render.githubusercontent.com/render/math?math=O(n)"> и пространственной сложности <img src="https://render.githubusercontent.com/render/math?math=O(1)"> в алгоритм временной сложности <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt n)"> и пространственной сложности <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt n)">.

Из их

- Каждое приращение j означает «детский шаг», умноженное на <img src="https://render.githubusercontent.com/render/math?math=g">  за раз.
- Каждое приращение i означает «гигантский шаг», умноженный на <img src="https://render.githubusercontent.com/render/math?math=g ^ {-m}"> за раз.

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

### Алгоритм Полларда ρ

Мы можем решить указанную выше проблему с временной сложностью <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt n)"> и пространственной сложностью <img src="https://render.githubusercontent.com/render/math?math=O(1)">. Пожалуйста, используйте Google для своих конкретных принципов.

### Алгоритм кенгуру Полларда

Если мы знаем, что диапазон x равен <img src="https://render.githubusercontent.com/render/math?math=a \leq x \leq b">, то мы можем решить указанную выше проблему с временной сложностью <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt{ba})">. Пожалуйста, используйте Google для своих конкретных принципов.

### Алгоритм Полига-Хеллмана

Предположим, что вышеупомянутая группа имеет ранг <img src="https://render.githubusercontent.com/render/math?math=n"> элемента <img src="https://render.githubusercontent.com/render/math?math=g"> и <img src="https://render.githubusercontent.com/render/math?math=n"> является гладким числом: <img src="https://render.githubusercontent.com/render/math?math=n=\prod_{i=1}^r p_i^{e_i}">.

1. Для каждого <img src="https://render.githubusercontent.com/render/math?math=i \in \{1,\ldots,r\}">:
2. Рассчитайте <img src="https://render.githubusercontent.com/render/math?math=g_i \equiv g ^ {n / p_i ^ {e_i}} \pmod m">. По теореме Лагранжа порядок <img src="https://render.githubusercontent.com/render/math?math=g_i"> в группе равен <img src="https://render.githubusercontent.com/render/math?math=p_i ^ {e_i}">.
3. Вычислить <img src="https://render.githubusercontent.com/render/math?math=y_i \equiv y ^ {n / p_i ^ {e_i}} \equiv g ^ {xn / p_i ^ {e_i}} \equiv g_i ^ {x} \equiv g_i ^ {x \bmod p_i ^ {e_i}} \equiv g_i ^ {x_i} \pmod m">, здесь мы знаем <img src="https://render.githubusercontent.com/render/math?math=y_i,m,g_i">, и <img src="https://render.githubusercontent.com/render/math?math=x_i"> варьируется от <img src="https://render.githubusercontent.com/render/math?math=[0,p_i ^ {e_i})">, с помощью гладких чисел <img src="https://render.githubusercontent.com/render/math?math=n"> , диапазон небольшой, поэтому мы можем быстро найти <img src="https://render.githubusercontent.com/render/math?math=x_i"> используя такие методы, как **алгоритм кенгуру Полларда**
4. Согласно приведенному выше выводу, мы можем получить для <img src="https://render.githubusercontent.com/render/math?math=i \in \{1,\ldots,r\}">, <img src="https://render.githubusercontent.com/render/math?math=x \equiv x_i \pmod {p_i ^ {e_i}}">, которое может быть решено с помощью китайской теоремы об остатках.

Вышеупомянутый процесс можно кратко описать на следующем рисунке:

![Pohlig Hellman Algorithm](../img/Pohlig-Hellman-Diagram.png)

Сложность есть <img src="https://render.githubusercontent.com/render/math?math=O \left (\sum_i e_i \left ( \log n+ \sqrt {p_i} \right ) \right )">, и видно, что сложность еще очень низкая.

Но когда <img src="https://render.githubusercontent.com/render/math?math=n"> простое, то <img src="https://render.githubusercontent.com/render/math?math=m=2n+1"> и сложности <img src="https://render.githubusercontent.com/render/math?math=O(\sqrt m)"> почти не различимы.

## Национальный чемпионат 2018 crackme java

Код показан ниже

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
Можно найти, что диапазон r равен <img src="https://render.githubusercontent.com/render/math?math=[0, 2 ^ {32})">, поэтому мы можем использовать алгоритм BSGS следующим образом

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

## Справка

- Элементарная теория чисел, Пан Чэндун, Пан Чэнъюй
- <https://ee.stanford.edu/~hellman/publications/28.pdf>
- <https://en.wikipedia.org/wiki/Pohlig%E2%80%93Hellman_algorithm#cite_note-Menezes97p108-2>
- <https://fortenf.org/e/crypto/2017/12/03/survey-of-discrete-log-algos.html>
