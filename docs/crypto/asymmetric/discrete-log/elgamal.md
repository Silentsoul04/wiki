[RU](./elgamal.md) | [EN](./elgamal-en.md)

# Эль-Гамаль

## Обзор

Безопасность алгоритма Эль-Гамаля основана на сложности решения задачи дискретного логарифмирования. Она была предложена в 1984 году и также представляет собой криптосистему с двойным ключом, которая может использоваться как для шифрования, так и для цифровой подписи.

Если мы предположим, что p является десятичным простым числом не менее 160 бит, **и p-1 имеет большой простой делитель**, а g является генератором <img src="https://render.githubusercontent.com/render/math?math=Z_p^*"> и <img src="https://render.githubusercontent.com/render/math?math=y \in Z_p^*">. Итак, как найти уникальное целое число x (<img src="https://render.githubusercontent.com/render/math?math=0 \leq x \leq p-2">), которое удовлетворяет <img src="https://render.githubusercontent.com/render/math?math=g^x \equiv y \bmod p">, алгоритмически сложно, здесь x как <img src="https://render.githubusercontent.com/render/math?math=x=log_gy">.

## Фундаментальный

Здесь мы предполагаем, что A хочет отправить сообщение m B.

### Генерация ключей

Основные шаги следующие

1. Трудно выбрать простое число p, достаточно большое для решения задачи дискретного логарифмирования на <img src="https://render.githubusercontent.com/render/math?math=Z_p">.
2. Выберите генератор g <img src="https://render.githubusercontent.com/render/math?math=Z_p^*">.
3. Случайным образом выберите целое число k, <img src="https://render.githubusercontent.com/render/math?math=0 \leq k \leq p-2">, и вычислите <img src="https://render.githubusercontent.com/render/math?math=g^k \equiv y \bmod p">.

Закрытый ключ - {k}, открытый ключ - {p, g, y}.

### Шифрование

A выбирает случайное число <img src="https://render.githubusercontent.com/render/math?math=r \in Z_{p-1}"> и шифрует открытый текст <img src="https://render.githubusercontent.com/render/math?math=E_k(m,r)=(y_1,y_2)">. Где <img src="https://render.githubusercontent.com/render/math?math=y_1 \equiv g^r \bmod p">, <img src="https://render.githubusercontent.com/render/math?math=y_2 \equiv my^r \bmod p">.

### Расшифровка

<img src="https://render.githubusercontent.com/render/math?math=D_k(y_1,y_2)=y_2(y_1^k)^-1 \bmod p \equiv m(g^k)^r(g^{rk})^{-1} \equiv m \bmod p">.

### Трудно

Хотя мы знаем y1, у нас нет возможности узнать соответствующее r.

## 2015 MMA CTF Алиса игра

В качестве примера мы возьмем Alicegame в MMA-CTF-2015 в 2015 году. Первоначально этот вопрос было трудно решить, когда исходный код не был предоставлен, потому что он дает m и дает r, чтобы получить зашифрованный результат, о чем слишком сложно думать.

Кратко проанализируем исходный код. Во-первых, программа изначально сгенерировала pk и sk.

```python
pk, sk = genkey(PBITS)
```

Где функция genkey выглядит следующим образом

```python
import random


def genkey(k):
    p = getPrime(k)
    g = random.randrange(2, p)
    x = random.randrange(1, p-1)
    h = pow(g, x, p)
    pk = (p, g, h)
    sk = (p, x)
    return pk, sk
```

p - простое число позиции k, g - книга в диапазоне (2, p), а x находится в диапазоне (1, p-1). И вычислил <img src="https://render.githubusercontent.com/render/math?math=h \equiv g^x \bmod p">. Видя это, я почти знаю, что это должно быть шифрование Эль-Гамаля для числового поля. Где pk - это открытый ключ, а sk - закрытый ключ.

Затем программа выводит 10 раз m и r. И используйте следующую функцию для шифрования

```python
import random


def encrypt(pk, m, r = None):
    (p, g, h) = pk
    if r is None:
        r = random.randrange(1, p-1)
    c1 = pow(g, r, p)
    c2 = (m * pow(h, r, p)) % p
    return c1, c2
```

Его метод шифрования действительно является шифрованием Эль-Гамаля.

Наконец, программа шифрует флаг. В это время r является самой программой случайным образом.

Анализ, здесь мы можем контролировать m и r за десять раундов, и

<p><img src="https://render.githubusercontent.com/render/math?math=c_1 \equiv g^r \bmod p"></p>

<p><img src="https://render.githubusercontent.com/render/math?math=c_2 \equiv m * h^r \bmod p"></p>

Если мы установим

1. r = 1, m = 1, тогда мы можем получить <img src="https://render.githubusercontent.com/render/math?math=c_1=g, c_2=h">.
2. r = 1, m = -1, тогда мы можем получить <img src="https://render.githubusercontent.com/render/math?math=c_1=g, c_2 = ph">. Тогда мы можем получить простое число p.

Какая польза от простого p? Количество бит в p составляет около 201, что очень много.

Но ах, после того, как он сгенерировал простое число p, это не было проверено. Мы уже говорили, что p-1 должен иметь большой множитель, а если есть маленький простой множитель, то мы можем атаковать. Атака в основном использует алгоритм «маленький шаг - гигантский шаг» и алгоритм Полига-Хеллмана. Если интересно, можете посмотреть. Здесь у самого мудреца есть функция для вычисления дискретного логарифма, которая может справиться с такой ситуацией. См. [discrete_log](<http://doc.sagemath.org/html/en/reference/groups/sage/groups/generic.html>).

Конкретный код выглядит следующим образом, следует отметить, что это потребление памяти относительно велико, не нужно просто запускать виртуальную машину. Еще есть взаимодействие с Нимой, от которого у меня болит голова.

```python
import socket

from Crypto.Util.number import *
from sage.all import *


def get_maxfactor(N):
    f = factor(N)
    print('factor done')
    return f[-1][0]

maxnumber = 1 << 70
i = 0
while 1:
    print('cycle: ', i)
    sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    sock.connect(("localhost", 9999))
    sock.recv(17)
    sock.recv(512)
    sock.sendall("1\n")
    sock.recv(512)
    sock.sendall("1\n")
    data = sock.recv(1024)
    print(data)
    if '\n' in data:
        data = data[:data.index('\n')]
    else:
        sock.recv(1024)
    g, h = eval(data)
    sock.sendall("-1\n")
    sock.recv(512)
    sock.sendall("1\n")
    data = sock.recv(1024)
    print(data)
    if '\n' in data:
        data = data[:data.index('\n')]
    else:
        sock.recv(512)
    g, tmp = eval(data)
    p = tmp + h
    tmp = get_maxfactor(p-1)
    if tmp < maxnumber:
        print('may be success')
        sock.sendall('quit\n')
        data = sock.recv(1024)
        print('receive data: ', data)
        data = data[data.index(":")+1:]
        c1, c2 = eval(data)
        g = Mod(g, p)
        h = Mod(h, p)
        c1 = Mod(c1, p)
        c2 = Mod(c2, p)
        x = discrete_log(h, g)
        print("x = ", x)
        print("Flag: ", long_to_bytes(long(c2 / ( c1 ** x))))
    sock.sendall('quit\n')
    sock.recv(1024)
    sock.close()
    i += 1
```

В конце концов, на компьютере не хватает памяти, она не рассчитывается, а иногда и рушится и запускается несколько раз.

## Код голубой лагалем 2018

Название описано ниже

```python
from Crypto.Util.number import *

from key import FLAG

size = 2048
rand_state = getRandomInteger(size // 2)

def keygen(size):
    q = getPrime(size)
    k = 2
    while 1:
        p = q * k + 1
        if isPrime(p):
            break
        k += 1
    g = 2
    while 1:
        if pow(g, q, p) == 1:
            break
        g += 1
    A = getRandomInteger(size) % q
    B = getRandomInteger(size) % q
    x = getRandomInteger(size) % q
    h = pow(g, x, p)
    return (g, h, A, B, p, q), (x,)

def rand(A, B, M):
    global rand_state
    rand_state, ret = (A * rand_state + B) % M, rand_state
    return right

def encrypt(pubkey, m):
    g, h, A, B, p, q = pubkey
    assert 0 < m <= p
    r = rand(A, B, q)
    c1 = pow(g, r, p)
    c2 = (m * pow(h, r, p)) % p
    return c1, c2

# pubkey, privkey = keygen(size)
m = bytes_to_long(FLAG)
c1, c2 = encrypt(pubkey, m)
c1_, c2_ = encrypt(pubkey, m)
print(pubkey)
print(c1, c2)
print(c1_, c2_)
```

Можно видеть, что алгоритм представляет собой шифрование Эль-Гамаля, которое дает два набора зашифрованных результатов с одним и тем же открытым текстом. Характерной чертой является то, что используемое случайное число r генерируется линейным конгруэнтным генератором, тогда мы знаем

<p><img src="https://render.githubusercontent.com/render/math?math=c2 \equiv m * h^r \bmod p"></p>

<p><img src="https://render.githubusercontent.com/render/math?math=c2\_ \equiv m*h^{(Ar%2BB) \bmod q} \equiv m*h^{Ar%2BB} \bmod p"></p>

тогда

<p><img src="https://render.githubusercontent.com/render/math?math=c2^A*h^B/c2\_ \equiv m^{A-1} \bmod p"></p>

Среди них известны c2, c2_, A, B, h. Тогда мы знаем

<p><img src="https://render.githubusercontent.com/render/math?math=m^{A-1} \equiv t \bmod p"></p>

Мы предполагаем, что нам известен первообразный корень g из p, тогда мы можем считать

<p><img src="https://render.githubusercontent.com/render/math?math=g^x \equiv t"></p>

<p><img src="https://render.githubusercontent.com/render/math?math=g^y \equiv m"></p>

тогда

<p><img src="https://render.githubusercontent.com/render/math?math=g^{y(A-1)}\equiv g^x \bmod p"></p>

тогда

<p><img src="https://render.githubusercontent.com/render/math?math=y(A-1) \equiv x \bmod p-1"></p>

тогда мы знаем

<p><img src="https://render.githubusercontent.com/render/math?math=y(A-1)-k(p-1)=x"></p>

Здесь мы знаем A, p, x, тогда мы можем использовать расширенную теорему Евклида, чтобы найти

<p><img src="https://render.githubusercontent.com/render/math?math=s(A-1)%2Bw(p-1)=gcd(A-1,t-1)"></p>

Если <img src="https://render.githubusercontent.com/render/math?math=gcd(A-1, t-1)=d">, то вычисляем непосредственно

<p><img src="https://render.githubusercontent.com/render/math?math=t^s \equiv m^{s(A-1)} \equiv m^d \bmod p"></p>

Если d = 1, то m известно напрямую.

Если d не 1, то это немного хлопотно.

Эта задача ровно d = 1, поэтому ее легко решить.

```python
import gmpy2


data = open('./transcript.txt').read().split('\n')
g, h, A, B, p, q = eval(data[0])
c1, c2 = eval(data[1])
c1_, c2_ = eval(data[2])
tmp = gmpy2.powmod(c2, A, p) * gmpy2.powmod(h, B, p) * gmpy2.invert(c2_, p)
tmp = tmp % p
print('t=', tmp)
print('A=', A)
print('p=', p)
gg, x, y = gmpy2.gcdext(A - 1, p - 1)
print(gg)
m = gmpy2.powmod(tmp, x, p)
print(hex(m)[2:].decode('hex'))
```

флаг

```shell
➜  2018-CodeBlue-lagalem git:(master) ✗ python exp.py
t= 24200833701856688878756977616650401715079183425722900529883514170904572086655826119242478732147288453761668954561939121426507899982627823151671207325781939341536650446260662452251070281875998376892857074363464032471952373518723746478141532996553854860936891133020681787570469383635252298945995672350873354628222982549233490189069478253457618473798487302495173105238289131448773538891748786125439847903309001198270694350004806890056215413633506973762313723658679532448729713653832387018928329243004507575710557548103815480626921755313420592693751934239155279580621162244859702224854316335659710333994740615748525806865323
A= 22171697832053348372915156043907956018090374461486719823366788630982715459384574553995928805167650346479356982401578161672693725423656918877111472214422442822321625228790031176477006387102261114291881317978365738605597034007565240733234828473235498045060301370063576730214239276663597216959028938702407690674202957249530224200656409763758677312265502252459474165905940522616924153211785956678275565280913390459395819438405830015823251969534345394385537526648860230429494250071276556746938056133344210445379647457181241674557283446678737258648530017213913802458974971453566678233726954727138234790969492546826523537158
p= 36416598149204678746613774367335394418818540686081178949292703167146103769686977098311936910892255381505012076996538695563763728453722792393508239790798417928810924208352785963037070885776153765280985533615624550198273407375650747001758391126814998498088382510133441013074771543464269812056636761840445695357746189203973350947418017496096468209755162029601945293367109584953080901393887040618021500119075628542529750701055865457182596931680189830763274025951607252183893164091069436120579097006203008253591406223666572333518943654621052210438476603030156263623221155480270748529488292790643952121391019941280923396132717
1
CBCTF{183a3ce8ed93df613b002252dfc741b2}
```

## Справка

- <https://www.math.auckland.ac.nz/~sgal018/crypto-book/solns.pdf>