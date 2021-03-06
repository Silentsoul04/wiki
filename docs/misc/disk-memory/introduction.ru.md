---
title: Введение
---

## Общие инструменты

- EasyRecovery
- MedAnalyze
- FTK
- Elcomsoft Forensic Disk Decryptor
- Volatility

## Диск

Общие форматы разделов диска следующие:

- Windows: FAT12 -> FAT16 -> FAT32 -> NTFS
- Linux: EXT2 -> EXT3 -> EXT4
- Структура мастер-диска FAT
- Удалить файл: первый байт имени файла в таблице каталогов - `e5`.

## VMDK

Файл VMDK по сути является виртуальной версией физического жесткого диска. Аналогичные области заполнения имеются также
в разделах и секторах физического жесткого диска. Мы можем использовать эти заполненные области, чтобы скрыть данные,
которые нам нужно скрыть, чтобы избежать сокрытия. Файлы увеличивают размер файла VMDK (например, прикрепляются
непосредственно к файловому бэкэнду), а также позволяют избежать ошибок виртуальной машины, вызванных изменениями
размера файла VMDK. И файлы VMDK, как правило, большие и подходят для скрытия больших файлов.

## ОЗУ

- Разрешить структуру памяти Windows / Linux / Mac OS X
- Процесс анализа, данные в памяти
- Находите подсказки и идеи на основе подсказок и извлекайте определенные данные из памяти для указанного процесса.

## Ссылки

- [Технология сокрытия данных](<https://www.sciencedirect.com/topics/computer-science/data-hiding-technique>)