[RU](./lattice-reduction.md) | [EN](./lattice-reduction-en.md)

# Ленстра - Ленстра - Ловаш

## Базовое введение

Алгоритм LLL заключается в нахождении набора оснований на решетке, который удовлетворяет следующим эффектам.

![lll-def](../img/lll-def.png)

Кроме того, очень полезны следующие свойства базы, сгенерированной этим методом.

![lll-property](../img/lll-property.png)

## Простое приложение

Здесь я приведу второй пример из статьи LLL. Для n действительных чисел <img src="https://render.githubusercontent.com/render/math?math=\alpha_i,\ldots,\alpha_n"> найдите рациональную линейную аппроксимацию n чисел, т.е. найдите n чисел <img src="https://render.githubusercontent.com/render/math?math=m_i">, так что <img src="https://render.githubusercontent.com/render/math?math=\sum_{i=1}^{n }m_i\alpha_i"> максимально равно 0. Мы можем построить такую ​​матрицу, где <img src="https://render.githubusercontent.com/render/math?math=a_i"> - рациональная аппроксимация <img src="https://render.githubusercontent.com/render/math?math=\alpha_i">.

<p><img src="https://render.githubusercontent.com/render/math?math=A=[\begin{matrix} 1, 0, 0, \cdots, 0, ca_1 \\ 0, 1, 0, \cdots, 0, c a_2 \\ 0, 0, 1, \cdots, 0, c a_3 \\ \vdots, \vdots, \vdots, \ddots, \vdots, \vdots \\ 0, 0, 0, \cdots, 1, c a_n \\ \end{matrix}]"></p>

Матрица равна n*(n + 1), мы можем найти определитель, соответствующий этой решетке, согласно методу нахождения определителя.

<p><img src="https://render.githubusercontent.com/render/math?math=Det(L)=\sqrt{AA^T}"></p>

Далее рассмотрим такую ​​матрицу

<p><img src="https://render.githubusercontent.com/render/math?math=A=[\begin{matrix} 1, 0, 0, \cdots, 0, a_1 \\ 0, 1, 0, \cdots, 0, a_2 \\ 0, 0, 1, \cdots, 0, a_3 \\ \vdots, \vdots, \vdots, \ddots, \vdots, \vdots \\ 0, 0, 0, \cdots, 1,  a_n \\ \end{matrix}]"></p>

Потом

<p><img src="https://render.githubusercontent.com/render/math?math=AA^T=[\begin{matrix} 1%2Ba_1^2, a_1 a_2, a_1 a_, \cdots, a_1 a_n \\ a_2 a_1, 1%2Ba_2^2, a_2 a_3, \cdots, a_2 a_n \\ a_3 a_1, a_3 a_2, 1%2Ba_3^2, \cdots, a_3 a_n \\ \vdots, \vdots, \vdots, \ddots, \vdots \\ a_n a_1, a_n a_2, a_n a_3, \cdots, 1%2Ba_n^2 \\ \end{matrix}]"></p>

Далее, давайте попробуем его от низкоразмерного к высокоразмерному (строго докажите, что вы можете рассмотреть возможность добавления строки и столбца, верхний левый угол равен 1), а определитель решетки

<p><img src="https://render.githubusercontent.com/render/math?math=\sqrt{1%2B\sum_{i=1}^n\alpha_i^2}"></p>

Можно сослаться на следующее доказательство аспиранта Югэ

![lll-application2](../img/lll-application2.png)

Тогда после алгоритма LLL мы можем получить

<p><img src="https://render.githubusercontent.com/render/math?math=||b_1|| \leq 2^{\frac{n-1}{4}} (1%2B\sum_{i=1}^n\alpha_i^2)^{\frac{1}{2(n%2B1)}}"></p>

В общем, последний элемент стремится к 1, когда он открывается n раз, потому что <img src="https://render.githubusercontent.com/render/math?math=a_i"> является константой и обычно не связан с n, поэтому

<p><img src="https://render.githubusercontent.com/render/math?math=||b_1|| \leq 2^{\frac{n-1}{4}}*k"></p>

k относительно невелик. Кроме того, <img src="https://render.githubusercontent.com/render/math?math=b_1"> - это линейная комбинация исходных векторов, тогда

<p><img src="https://render.githubusercontent.com/render/math?math=b_1[n]=\sum_{i=1}^{n}m_ic*a_i=c\sum_{i=1}^{n}m_i*a_i"></p>

Очевидно, что если c достаточно велико, то последующее суммирование должно быть достаточно малым, чтобы удовлетворять вышеуказанным ограничениям.