---
title: JPG (JPEG)
---

## Файловая структура

- JPEG - это формат сжатия с потерями. Информация о пикселях сохраняется как файл в формате JPEG, а затем считывается.
  Некоторые значения пикселей немного изменятся. Есть параметр качества, который при сохранении можно выбрать от 0 до
  100. Чем больше параметр, тем точнее будет изображение, но тем больше будет размер изображения. В общем, достаточно
  выбрать 70 или 80.
- JPEG не имеет информации о прозрачности

Базовая структура данных JPG бывает двух основных типов: «сегментные» и данные изображения с кодировкой сжатия.

| Название               | Байты | Данные | Описание                                                                                  |
| :--------------------: | :---: | :----: | :---------------------------------------------------------------------------------------: |
| Segment Identification | 1     | FF     | Start ID of each new segment                                                              |
| Segment Type           | 1     |        | Type Encoding (called Tag)                                                                |
| Segment length         | 2     |        | Includes segment content and segment length itself, excluding segment ID and segment type |
| Section content        | 2     |        | ≤65533 bytes                                                                              |

- Некоторые сегменты не имеют описания длины и содержимого, только идентификация сегмента и тип сегмента. И заголовок, и
  конец файла принадлежат этому сегменту.
- Независимо от того, сколько `FF` разрешено между сегментами и сегментами, эти `FF` называются «байтами заполнения» и
  должны игнорироваться.

Некоторые распространенные типы сегментов

![jpgformat](../../assets/img/pictures/jpgformat.png)

`0xffd8` и `0xffd9` - это флаги для начала файла JPG.

## Утилиты

### [Stegdetect](<https://github.com/redNixon/stegdetect>)

Стеганографический инструмент для оценки частотных коэффициентов DCT файлов JPEG с помощью методов статистического
анализа, который может быть обнаружен JSteg, JPHide, OutGuess, невидимая информация, скрытая стеганографическими
инструментами, такими как Secrets, F5, appendX и Camouflage, а также имеет встроенную скрытую информацию в методах
Jphide, outguess и jsteg-shell на основе словарного метода криптографии грубой силы.

```shell
-q Отображает только изображения, которые могут содержать скрытый контент.
-n Включает проверку функции заголовка JPEG для уменьшения количества ложных срабатываний. Если этот параметр включен, все файлы с аннотированными областями будут обрабатываться так, как если бы они не были внедрены. Если номер версии в идентификаторе JFIF файла JPEG отличается от 1.1, обнаружение OutGuess отключено.
-s Изменяет чувствительность алгоритма обнаружения. По умолчанию это значение равно 1. Степень совпадения результата обнаружения прямо пропорциональна чувствительности алгоритма обнаружения. Чем больше значение чувствительности алгоритма, тем больше вероятность, что обнаруженный подозрительный файл содержит конфиденциальную информацию.
-d Выводит отладочную информацию с номерами строк.
-t Установить, какие стеганографические инструменты следует обнаруживать (задание обнаружения по умолчаниюi), можно установить следующие параметры:
j Убедитесь, что информация изображения встроена в jsteg.
o Определить, скрыта ли информация в изображении.
p Определяет, встроена ли информация изображения в jphide.
i Определяет, скрыта ли информация в изображении невидимыми секретами.
```

### [JPHS](<http://linux01.gwdg.de/~alatham/stego.html>)

JPHS, программа для сокрытия информации для изображений JPEG, представляет собой инструмент, разработанный Алланом
Лэтэмом для реализации шифрования информации и обнаружения извлечения для сжатых с потерями файлов JPEG на системных
платформах Windows и Linux. Программное обеспечение в основном содержит две программы JPHIDE и JPSEEK. Программа JPHIDE
в основном реализует функцию сокрытия информации о шифровании файла в изображение JPEG. Программа JPSEEK в основном
реализует файл информации об обнаружении и извлечении изображения JPEG, полученный путем шифрования и скрытия с помощью
программы JPHIDE. Программа JPHSWIN в версии JPHS для Windows имеет графический интерфейс и функции JPHIDE и JPSEEK.

### [SilentEye](<http://silenteye.v1kings.io/>)

SilentEye - это кроссплатформенное приложение, позволяющее легко использовать стеганографию, в данном случае скрывая
сообщения в изображениях или звуках. Он обеспечивает довольно приятный интерфейс и простую интеграцию нового алгоритма
стеганографии и процесса криптографии с помощью системы плагинов.