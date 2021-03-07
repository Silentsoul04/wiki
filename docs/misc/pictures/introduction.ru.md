---
title: Введение
---

Файлы изображений являются хорошим источником хакерской культуры, поэтому в соревнованиях CTF часто появляются различные
файлы изображений.

Файлы изображений имеют множество сложных форматов и могут использоваться для разнообразного анализа и дешифрования,
включая метаданные, потерю информации и сжатие без потерь, проверку, стеганографию или кодирование визуальных данных,
все из которых являются важными направлениями в Misc. . Есть много точек знаний (включая основные форматы файлов,
распространенное программное обеспечение для стеганографии и стеганографии), а некоторые места требуют глубокого
понимания.

## Метаданные

??? info 
    Метаданные, также известные как данные-посредники, данные ретрансляции, - это данные о данных, в основном 
    информация, описывающая свойства данных, используемые для поддержки, такие как место хранения, исторические данные,
    поиск ресурсов, файл и многое другое.

Скрытая информация в метаданных - это самый простой метод в игре, обычно используемый для сокрытия некоторой ключевой
информации подсказки или некоторой важной информации, такой как пароль.

Вы можете просмотреть этот тип метаданных, щелкнув правой кнопкой мыши свойство - или используя команду `strings`. Как
правило, некоторая скрытая информация (странные строки) часто появляется в заголовке или трейлере.

Затем мы вводим команду `identify`, которая используется для получения формата и характеристик одного или нескольких
файлов изображений.

`-format` используется для указания отображаемой информации, а гибкое использование его параметра `-format` может
значительно облегчить решение
проблем. [format specific meaning of each parameter](<https://www.imagemagick.org/script/escape.php>)

### Пример

??? example "Break In 2017 - Mysterious GIF"
    [Ссылка](<https://github.com/ctfs/write-ups-2017/tree/master/breakin-ctf-2017/misc/Mysterious-GIF>)

    Одна из трудностей этой проблемы - обнаружение и извлечение метаданных в GIF. Во-первых, `strings` могут наблюдать
    аномальные точки.

    ```text
    GIF89a
       !!!"""###$$$%%%&&&'''((()))***+++,,,---...///000111222333444555666777888999:::;;;<<<===>>>???@@@AAABBBCCCDDDEEEFFFGGGHHHIIIJJJKKKLLLMMMNNNOOOPPPQQQRRRSSSTTTUUUVVVWWWXXXYYYZZZ[[[\\\]]]^^^___```aaabbbcccdddeeefffggghhhiiijjjkkklllmmmnnnooopppqqqrrrssstttuuuvvvwwwxxxyyyzzz{{{|||}}}~~~
    
    4d494945767749424144414e42676b71686b6947397730424151454641415343424b6b776767536c41674541416f4942415144644d4e624c3571565769435172
    NETSCAPE2.0
    
    ImageMagick
    
    ...
    ```

    Шестнадцатеричная строка здесь фактически скрыта в области метаданных GIF.
    
    Следующий шаг - извлечение, можно выбрать Python, но удобнее использовать `identify`

    ```shell
    root in ~/Desktop/tmp λ identify -format "%s %c \n" Question.gif
    
    0 4d494945767749424144414e42676b71686b6947397730424151454641415343424b6b776767536c41674541416f4942415144644d4e624c3571565769435172
    1 5832773639712f377933536849507565707478664177525162524f72653330633655772f6f4b3877655a547834346d30414c6f75685634364b63514a6b687271
    ...
    24 484b7735432b667741586c4649746d30396145565458772b787a4c4a623253723667415450574d35715661756278667362356d58482f77443969434c684a536f
    25 724b3052485a6b745062457335797444737142486435504646773d3d
    ```

    Другие процессы здесь не описаны, обратитесь к разделу «Запись» по ссылке.

## Преобразование значения пикселей

Посмотрите на данные в этом файле, что вы можете придумать?

```text
255,255,255,255,255...........
```

Это строка значений RGB, попробуйте преобразовать его в картинку

```python
from PIL import Image
from random import shuffle
import numpy as np


def pad_message(m, p):
    m_length = len(m)
    p_length = 1024 - m_length
    p_l = len(p)
    left_p = int(p_length / 2)

    pad_left = ''
    pad_right = ''
    for i in range(left_p):
        pad_left += p[i % p_l]
        pad_right += p[i % p_l]

    if p_length % 2 == 1:
        pad_right += p[(left_p + 1) % p_l]

    return pad_left + m + pad_right


def create_image_list(l, cols, rep):
    row_list = []
    i_list = []
    for i in range(len(l)):
        if (i + 1) % cols == 0:  # last column
            for j in range(rep):
                row_list.append(l[i])
                i_list.append(row_list)
            row_list = []
        else:
            for j in range(rep):
                row_list.append(l[i])
    return i_list


text = 'flag_here'
padding = 'owmybrain'
nums = list(map(ord, text))
diffs = [nums[0]] + [b - a for a, b in zip(nums[:-1], nums[1:])]
bf = ''.join([('-' if x < 0 else '+') * abs(x) + '.' for x in diffs])
message = pad_message(bf, padding)
counter = 0
rgb_list = []

for r in range(0, 256, 16):
    for g in range(0, 256, 32):
        for b in range(0, 256, 32):
            rgb_list.append([r, g, b, ord(message[counter])])
            counter += 1
shuffle(rgb_list)
img_list = create_image_list(rgb_list, 32, 16)
img = Image.fromarray(np.uint8(img_list)).convert('RGBA')
img.save('flag.png')
```

А если наоборот, извлеките значения RGB из изображения, а затем сравните значения RGB, чтобы получить окончательный
флаг.

Большинство этих тем представляют собой изображения некоторых блоков пикселей, как показано ниже.

![brainfun](../../assets/img/pictures/brainfun.png)

Похожие темы:

- [CSAW-2016-quals:Forensic/Barinfun](<https://github.com/ctfs/write-ups-2016/tree/master/csaw-ctf-2016-quals/forensics/brainfun-50>)
- [breakin-ctf-2017:A-dance-partner](<https://github.com/ctfs/write-ups-2017/tree/master/breakin-ctf-2017/misc/A-dance-partner>)