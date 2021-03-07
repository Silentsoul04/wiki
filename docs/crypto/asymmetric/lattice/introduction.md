[RU](./introduction.md) | [EN](./introduction-en.md)

# Базовое введение

## Определение решетки

Решетка представляет собой линейную комбинацию всех целочисленных коэффициентов n (<img src="https://render.githubusercontent.com/render/math?math=m\geq n">) линейно независимых векторов <img src="https://render.githubusercontent.com/render/math?math=b_i(1\leq i \leq n)"> m-мерного евклидова пространства <img src="https://render.githubusercontent.com/render/math?math=R^m">, т.е. <img src="https://render.githubusercontent.com/render/math?math=L(B)=\{\sum_{i=1}^{n}x_ib_i:x_i \in Z, 1 \leq i \leq n\}">

Здесь B - набор из n векторов, мы называем

- Эти `n` векторы представляют собой набор базисов решетки `L`.
- Ранг решетки `L` равен `n`.
- Количество битов в `L` равно `m`.

Если `m = n`, то мы называем этот формат полным рангом.

Конечно, вместо <img src="https://render.githubusercontent.com/render/math?math=R^m"> в пространстве могут быть другие группы.

## Основное определение в решетках

### Последовательный минимум

Пусть решетка `L` является решеткой в ​​m-мерном евклидовом пространстве <img src="https://render.githubusercontent.com/render/math?math=R^m"> с рангом `n`, тогда непрерывная минимальная длина `L` (последовательные минимумы) равна <img src="https://render.githubusercontent.com/render/math?math=\lambda_1,\ldots,\lambda_n \in R">, где для любого <img src="https://render.githubusercontent.com/render/math?math=1 \leq i\leq n, \lambda_i"> - минимальное значение, удовлетворяющее тому, что для `i` линейно независимых векторов <img src="https://render.githubusercontent.com/render/math?math=v_i, ||v_j||\leq \lambda_i,1\leq j\leq i">.

Очевидно, мы имеем <img src="https://render.githubusercontent.com/render/math?math=\lambda_i \leq \lambda_j ,\forall i < j">.

## Расчет сложных задач в решетке.

**Задача кратчайшего вектора (SVP)**: для решетки `L` и ее базового вектора `B` найти ненулевой вектор `v` в решетке `L` такой, что для любого другого ненулевого вектора `u` в решетке <img src="https://render.githubusercontent.com/render/math?math=||v|| \leq ||u||">.

**<img src="https://render.githubusercontent.com/render/math?math=\gamma">-Приближенная задача наикратчайшего вектора (SVP - <img src="https://render.githubusercontent.com/render/math?math=\gamma">)**: для фиксированного `L` найти ненулевой вектор `v` в решетке `L` такой, что для любого другого ненулевого вектора `u` в решетке <img src="https://render.githubusercontent.com/render/math?math=||v|| \leq \gamma||u||">

**Задача последовательных минимумов (SMP)**: для решетки `L` ранга `n` найти `n` линейно независимых векторов <img src="https://render.githubusercontent.com/render/math?math=s_i"> в решетке `L`, удовлетворяющих условиям <img src="https://render.githubusercontent.com/render/math?math=\lambda_i(L)=||s_i||, 1 \leq i \leq n">.

**Задача кратчайших независимых векторов (SIVP)**: для решетки `L` ранга `n` найти `n` линейных независимых векторов <img src="https://render.githubusercontent.com/render/math?math=s_i"> в решетке `L`, удовлетворяющих <img src="https://render.githubusercontent.com/render/math?math=||s_i|| \leq \lambda_n(L), 1 \leq i \leq n">.

**Задача о единственном кратчайшем векторе (uSVP - <img src="https://render.githubusercontent.com/render/math?math=\gamma">)**: для фиксированного `L`, удовлетворяющего <img src="https://render.githubusercontent.com/render/math?math=\lambda_2(L) > \gamma \lambda_1(L)">, найти кратчайший вектор ячейки.

**Задача ближайшего вектора (CVP)**: для решетки `L` и целевого вектора <img src="https://render.githubusercontent.com/render/math?math=t \in R^m"> найдите ненулевой вектор `v` в решетке такой, что для любого ненулевого вектора `u` в решетке выполняется условие <img src="https://render.githubusercontent.com/render/math?math=||vt|| \leq ||ut||">.