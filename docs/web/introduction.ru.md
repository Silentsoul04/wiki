---
title: Введение
---

С появлением ряда таких новых Интернет-продуктов, как WEB 2.0, социальные сети и тд. Интернет-приложения, основанные на
WEB-среде, становятся все более обширными. В процессе информатизации предприятия создаются различные приложения на WEB
платформе, WEB бизнес. Стремительное развитие также вызвало серьезную озабоченность хакеров. За этим последовало
появление угроз безопасности WEB. Хакеры используют уязвимость операционной системы и уязвимости программы веб-службы,
чтобы получить контроль над веб-сервером и подделать содержимое веб-страницы. Кража важных внутренних данных, что еще
более серьезно, заключается во внедрении вредоносного кода в веб-страницы, в результате чего посетители веб-сайтов могут
быть скомпрометированы.

В соревновании CTF WEB также является одним из важнейших направлений. Категория WEB включает широкий спектр тем, а точки
знаний фрагментированы и зависят от времени. Он может идти в ногу с текущими горячими точками и быть близок к реальному
бою.

Темы в классе WEB включают, помимо прочего, внедрение SQL, межсайтовые сценарии XSS, подделку межсайтовых запросов CSRF,
загрузку файлов, включение файлов, безопасность фреймворка, общие уязвимости PHP, аудит кода и многое другое.

## SQL-инъекция

Путем внедрения синтаксиса SQL в параметры, контролируемые пользователем, исходная структура SQL разрушается, и
достигается поведение атаки, приводящее к неожиданным результатам при написании программы. Причину можно отнести к
наложению следующих двух причин:

1. Программист пишет оператор SQL, используя конкатенацию строк при взаимодействии приложения и базы данных.
2. Параметры, контролируемые пользователем, недостаточно отфильтрованы для объединения параметров в оператор SQL.

## Атака межсайтового скриптинга XSS

Межсайтовый сценарий сокращается до аббревиатуры каскадных таблиц стилей (CSS), поэтому атака межсайтового сценария
сокращается как XSS. Злоумышленник вставляет вредоносный HTML-код на веб-страницу. Когда пользователь просматривает
страницу, HTML-код, встроенный в Интернет, будет выполняться, тем самым достигая специальной цели злонамеренной атаки на
пользователя.

## Выполнение команды

Когда приложению необходимо вызвать некоторую внешнюю программу для обработки содержимого, используются некоторые
функции, выполняющие системные команды. Например, `system`, `exec`, `shell_exec` и тд. В PHP, когда пользователь может
управлять параметрами в функции выполнения команды, вредоносная системная команда может быть введена в обычную команду,
вызывая атаку выполнения команды. В основном это введение уязвимостей выполнения команд, в основном в PHP, а также
детали Java и других приложений, которые должны быть добавлены.

## Файл содержит

Если пользовательскому вводу клиента разрешено управлять файлами, динамически включаемыми на сервер, это приведет к
выполнению вредоносного кода и раскрытию конфиденциальной информации, в основном включая включение локального файла и
включение удаленного файла.

## Подделка межсайтового запроса CSRF

Подделка межсайтовых запросов (CSRF) - это атака, которая заставляет вошедшего в систему пользователя выполнять
некоторые действия без его ведома. Поскольку злоумышленник не видит ответа на поддельный запрос, CSRF-атака в основном
используется для выполнения действий, а не для кражи пользовательских данных. Когда жертвой является обычный
пользователь, CSRF может переводить средства пользователя, отправлять почту и тд. Без их ведома, но если жертвой
является пользователь с правами администратора, CSRF может угрожать всей WEB-системе.

## Подделка запроса на стороне сервера SSRF

SSRF (подделка запросов на стороне сервера) - это уязвимость системы безопасности, созданная злоумышленником для
формирования запроса, инициированного сервером. Как правило, целью атаки SSRF является внутренняя система, недоступная
из внешней сети.

## Файл загружен

При работе веб-сайта неизбежно обновляются некоторые страницы или содержимое веб-сайта, и тогда требуется функция
загрузки файлов на веб-сайт. Если вы не ограничиваете ограничения или ограничения игнорируются, эту функцию можно
использовать для загрузки исполняемых файлов, сценариев на сервер и дальнейшего падения сервера.

## Нажмите, чтобы захватить

Clickjacking был впервые создан в 2008 году экспертами по интернет-безопасности Робертом Хансеном и Джеремией
Граусманом.

Это своего рода визуальный спуфинг. На веб-стороне iframe вложен в прозрачную и невидимую страницу, так что пользователь
может щелкнуть в том месте, где злоумышленник хочет обманом заставить пользователя щелкнуть, не зная об этом.

Из-за появления кликджекинга существует способ антифреймовой вложенности, потому что кликджекинг требует для атаки
вложенных страниц iframe.

Следующий код является наиболее распространенным примером предотвращения вложения кадров:

```js
if (top.location != location)
    top.location = self.location;
```

## Виртуальный частный сервер VPS

Технология VPS (Virtual Private Server), которая разделяет сервер на высококачественные услуги для нескольких
виртуальных частных серверов. Технология реализации VPS делится на контейнерную технологию и технологию виртуализации. В
контейнере или виртуальной машине каждому VPS можно назначить отдельный общедоступный IP-адрес, отдельную операционную
систему и обеспечить изоляцию между различным дисковым пространством VPS, памятью, ресурсами ЦП, процессами и
конфигурациями системы, имитируя исключительное использование для пользователей и приложений. Опыт использования
вычислительных ресурсов. VPS может переустанавливать операционную систему, устанавливать программы и перезапускать
сервер отдельно, как автономный сервер. VPS предоставляет пользователям свободу управления конфигурациями для
виртуализации предприятия или для аренды ресурсов IDC.

Аренда ресурсов IDC, предоставляемых провайдером VPS. Разница в аппаратном программном обеспечении VPS, используемом
разными поставщиками VPS, и различными стратегиями продаж, а также опыт VPS. Особенно, когда поставщик VPS перепродан,
производительность VPS сильно пострадает при перегрузке физического сервера. Условно говоря, контейнерная технология
более эффективна и дороже, чем оборудование для технологии виртуальных машин, поэтому цена контейнерного VPS, как
правило, ниже, чем цена VPS для виртуальных машин.

## Условный конкурс

Уязвимость с условным конфликтом — уязвимость на стороне сервера. Поскольку при обработке запросов от разных
пользователей серверная сторона обрабатывает одновременно, такие проблемы могут возникнуть, если параллельная обработка
является неправильной или логическая последовательность связанных операций является необоснованной.

## XXE

XXE Injection — внедрение внешней сущности XML, которая представляет собой атаку путем инъекции внешней сущности XML.
Уязвимости — это проблемы безопасности, возникающие при обработке незащищенных данных внешнего объекта.

В стандарте XML 1.0 понятие сущностей определяется в структуре документа XML. Сущности могут вызываться в документе по
предварительному определению, а идентификатор объекта может обращаться к локальному или удаленному контенту. Если
«загрязнение» вводится в процесс Источники после обработки XML-документов, это может привести к проблемам безопасности,
таким как утечка информации.

## XSCH

Из-за халатности веб-разработчиков в процессе разработки с использованием Flash, Silverlight и тд. Правильная настройка
файла междоменной политики (crossdomain.xml) не вызвала проблем. Например:

```xml

<cross-domain-policy>
    <allow-access-from domain=“*”/>
</cross-domain-policy>
```

Поскольку файл междоменной политики настроен как *, это означает, что любой домен Flash может взаимодействовать с ним,
что может инициировать запросы и получать данные.

## С превышением правомочий (отсутствует доступ к функциональному уровню)

Несанкционированная уязвимость — это распространенная уязвимость безопасности в веб-приложениях. Его угроза заключается
в том, что учетная запись может управлять пользовательскими данными тахеометра. Конечно, эти данные ограничены данными,
соответствующими функции уязвимости. Причина уязвимости с ультра-полномочиями в основном связана с тем, что разработчик
чрезмерно доверяет данным, запрошенным клиентом при добавлении, удалении, изменении и запросе данных, и пропускает
полномочия. Таким образом, тестирование избыточной авторизации — это процесс тщательного планирования с разработчиками.

## Раскрытие конфиденциальной информации

Конфиденциальная информация относится к информации, которая не известна общественности, имеет фактическую и
потенциальную ценность для использования и безвредна для общества, бизнеса или отдельных лиц из-за потери, неправильного
использования или несанкционированного доступа. В том числе: информация о личной конфиденциальности, информация о
бизнес-операциях, финансовая информация, информация о персонале, информация об эксплуатации и техническом обслуживании
ИТ. Утечки включают Github, библиотеку Baidu, код Google, каталоги веб-сайтов и многое другое.

## Неправильная конфигурация безопасности

Неверная конфигурация безопасности: иногда использование конфигурации безопасности по умолчанию может сделать ваше
приложение уязвимым для множественных атак. Важно использовать лучшую конфигурацию безопасности, доступную для
развернутых приложений, веб-серверов, серверов баз данных, операционных систем, библиотек кода и всех компонентов,
связанных с приложениями.

## WAF

Система защиты веб-приложений (также известная как: система предотвращения вторжений на уровне веб-приложений.
Английский язык: брандмауэр веб-приложений, именуемый: WAF). Воспользуйтесь преимуществом международно признанного
заявления: брандмауэр веб-приложений — это продукт, который специально защищает веб-приложения, реализуя серию политик
безопасности для HTTP / HTTPS.

## IDS

IDS — это аббревиатура от английского Intrusion Detection Systems, что в переводе означает «система обнаружения
вторжений». Говоря профессионально, в соответствии с определенной политикой безопасности, с помощью программного и
аппаратного обеспечения отслеживается состояние сети и работы системы, и в максимально возможной степени обнаруживаются
различные попытки атак, атаки или результаты атак, чтобы обеспечить конфиденциальность, целостность и доступность
ресурсов сетевой системы. Чтобы создать образную метафору: если брандмауэр — это дверной замок здания, то IDS — это
система мониторинга в этом здании. Как только вор забирается в здание или инсайдер ведет себя вне пределов его границ,
только система мониторинга в режиме реального времени может обнаружить ситуацию и выдать предупреждение.

## IPS

Система предотвращения вторжений (IPS) — это средство обеспечения безопасности компьютерной сети, которое дополняет
антивирусные программы и фильтры пакетов (шлюзы приложений). Система предотвращения вторжений — это устройство
безопасности компьютерной сети, которое может отслеживать поведение передачи сетевых данных в сети или сетевом
устройстве и может мгновенно прерывать, корректировать или изолировать некоторые ненормальные или опасные режимы
передачи данных в сети.