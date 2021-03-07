Когда мы импортируем скрипт python, мы сгенерируем соответствующий файл pyc в каталоге, который является постоянной
формой хранения pythoncodeobj для ускорения следующей загрузки.

## Файловая структура

Файл Pyc состоит из трех основных частей

- Первые 4 байта - это Maigg int, который определяет информацию о версии этого pyc.
- Следующие четыре байта по-прежнему представляют собой int, то есть время генерации pyc.
- Сериализованный PyCodeObject, ссылка на
  структуру [include/code.h](<https://github.com/python/cpython/blob/master/Include/code.h>), метод
  сериализации [python/marshal](<https://github.com/python/cpython/blob/master/Python/marshal.c>)

**полный анализ файла pyc может ссылаться на**

- [How the Python program is executed](<http://python.jobbole.com/84599/>)
- [PYC File Format Analysis](<http://kdr2.com/tech/python/pyc-format.html>)

**О co_code**

Строка двоичных потоков, представляющая последовательность инструкций, специально определенную
в [include/opcode.h](<https://github.com/python/cpython/blob/fc7df0e664198cb05cafd972f190a18ca422989c/Include/opcode.h>)
, или обратитесь к [python Opcodes](<http://unpyc.sourceforge.net/Opcodes.html>).

от

- Директива (код операции) делится на параметры и без параметров, которые делятся
  на <https://github.com/python/cpython/blob/fc7df0e664198cb05cafd972f190a18ca422989c/Include/opcode.h#L69>
- параметр (oparg)

Вышеуказанные параметры python3.6 всегда занимают 1 байт. Если инструкция не имеет параметров, она заменяется на `0x00`,
который игнорируется интерпретатором во время выполнения. Это также технический принцип **Stegosaurus**; версия ниже
python3.5. Средняя инструкция не имеет аргументов, но не имеет заполнения `0x00`.

### Пример

- [0ctf-2017:py](<https://github.com/ctfs/write-ups-2017/tree/master/0ctf-quals-2017/reverse/py-137>)
- [Remember a CPython bytecode](<http://0x48.pw/2017/03/20/0x2f/>)

## Утилиты

### [pycdc](<https://github.com/zrax/pycdc>)

!!! info
    Преобразование байт-кода Python в читаемый исходный код Python, включая дизассемблирование (pycads) и декомпиляцию 
    (pycdc)

### [Stegosaurus](<https://bitbucket.org/jherron/stegosaurus/src>)

!!! info 
    Позволяет нам встраивать любую полезную нагрузку в файл байт-кода Python (pyc или pyo). Из-за низкой плотности 
    кодирования процесс встраивания полезной нагрузки не меняет поведение исходного кода и не меняет размер исходного 
    файла.

Принцип состоит в том, чтобы использовать избыточное пространство в файле байт-кода python, чтобы скрыть полный код
полезной нагрузки в этих фрагментированных пространствах.

**Ссылки**: [A steganographic tool for embedding Payload in Python bytecode – Stegosaurus](<http://www.freebuf.com/sectool/129357.html>)

**Задачи**: [WHCTF-2017:Py-Py-Py](<https://www.xctf.org.cn/library/details/whctf-writeup/>)