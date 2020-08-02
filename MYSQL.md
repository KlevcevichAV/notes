# MySQL
---------------------
#### Содержание
+ [Создание базы данных](#createDatabase)
+ [Работа с таблицами](#workWithATable)
  + [Создание таблицы](#tableCreation)
  + [Изменение структуры таблицы](#changingTheStructureOfATable)
  + [Другие команды для работы с таблицами](#anotherCommands)
+ [Ввод данных в таблицу](#dataInput)
  + [Загрузка данных из файла](#loadingDataFromFile)
  + [Вставка отдельных строк](#insertRows)
+ [Извлечение данных из таблицы](#retrievingDataFromATable)
  + [Простые запросы](#simpleQueries)
  + [Условия отбора](#selectionConditions)
  + [Объединение таблиц](#concatenationOfTables)
  + [Вложенные запросы](#nestedQueries)
  + [Объединение результатов запросов](#combiningQueryResults)
  + [Выгрузка данных из файла](#unloadingDataFromAFile)
+ [Изменение данных](#dataChange)

<a name="createDatabase"><h2>Создание базы данных</h2></a>
Чтобы создать базу данных, выполним команду
CREATE DATABASE <Имя базы данных>;

Если вам по каким-либо причинам нужно для новой базы данных установить кодировку по умолчанию, отличную от кодировки, указанной при настройке MySQL, то при создании базы данных вы можете указать нужную кодировку (character set) и/или правило сравнения (сортировки) символьных значений:
CREATE DATABASE <Имя базы данных>
CHARACTER SET <Имя кодировки>
COLLATE <Имя правила сравнения>;

CREATE DATABASE SalesDept
CHARACTER SET cp1251 COLLATE cp1251_general_ci;

+ Чтобы просмотреть список используемых в MySQL кодировок, выполним команду SHOW CHARACTER SET;

+ а чтобы увидеть список правил сравнения символьных значений – команду SHOW COLLATION;

+ Изменить кодировку и/или правило сравнения символьных значений для базы данных вы можете с помощью команды
ALTER DATABASE <Имя базы данных>
CHARACTER SET <Имя кодировки>
COLLATE <Имя правила сравнения>;

+ Чтобы удалить ненужную или ошибочно созданную базу данных, выполните команду
DROP DATABASE <Имя базы данных>;

+ Одну из баз, созданных на данном сервере MySQL, вы можете выбрать в качестве текущей базы данных с помощью команды
USE <Имя базы данных>;

+ Чтобы увидеть список всех баз, существующих на данном сервере MySQL, выполните команду
SHOW DATABASES;

+ Даже если вы еще не создали ни одной базы данных, в полученном списке вы увидите
три системных базы данных
  + INFORMATION_SCHEMA
  + mysql
  + test

<a name="workWithATable"><h2>Работа с таблицами</h2></a>

<a name="tableCreation"><h3>Создание таблицы</h3></a>
+ Команда создания таблицы
CREATE TABLE <Имя таблицы>
(<Имя столбца 1> <Тип столбца 1> [<Свойства столбца 1>],
<Имя столбца 2> <Тип столбца 2> [<Свойства столбца 2>],
...
[<Информация о ключевых столбцах и индексах>])
[<Опциональные свойства таблицы>];

CREATE TABLE Customers
(id SERIAL,
name VARCHAR(100),
phone VARCHAR(20),
address VARCHAR(150),
rating INT,
PRIMARY KEY (id))
ENGINE InnoDB CHARACTER SET utf8;

 id – идентификатор записи. Этому столбцу вы назначили тип SERIAL, позволяющий автоматически нумеровать строки таблицы. Ключевое слово SERIAL расшифровывается как BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE. Это означает, что В. Гольцман. «MySQL 5.0. Библиотека программиста» 59 в столбец можно вводить большие целые (BIGINT) положительные (UNSIGNED) числа, при этом автоматически контролируется отсутствие неопределенных и повторяющихся значений (NOT NULL UNIQUE). Если при добавлении строки в таблицу вы не укажете значение для этого столбца, то программа MySQL внесет в этот столбец очередной порядковый номер (AUTO_INCREMENT).

NULL – это константа, указывающая на отсутствие значения.Не следует путать NULL с пустой строкой («») или числом 0. Значения NULL обрабатываются особым образом: большинство функций и операторов возвращают NULL, если один из аргументов равен NULL. Например, результат сравнения 1 = 1 – истинное значение (TRUE), а результат сравнения NULL = NULL – неопределенное значение (NULL), то есть два неопределенных значения не считаются равными.

 Nam – имя клиента, phone – номер телефона и address – адрес. Вы присвоили этим столбцам тип VARCHAR, поскольку они будут содержать символьные значения. В скобках указывается максимально допустимое количество символов в значении столбца.

Rating – рейтинг. Тип INT означает, что столбец будет содержать обычные целые числа.

В-третьих, вы указали, что столбец id будет первичным ключом таблицы, включив в команду создания таблицы определение PRIMARY KEY (id).

В-четвертых, вы задали для этой таблицы два опциональных параметра. Параметр ENGINE определяет тип таблицы. 

Параметр CHARACTER SET определяет кодировку по умолчанию для данных в таблице.
Поскольку вы не задали кодировку отдельно для столбцов name, phone и address, данные в этих столбцах будут храниться в кодировке UTF-8, которая назначена в качестве кодировки по умолчанию для таблицы Customers.

CREATE TABLE Products
(id SERIAL,
description VARCHAR(100),
details TEXT,
price DECIMAL(8,2),
PRIMARY KEY (id))
ENGINE InnoDB CHARACTER SET utf8;

Столбец details(описание) имеет тип TEXT. Этот тип удобно использовать вместо типа VARCHAR, если столбец будет содержать длинные значения: суммарная длина значений всех столбцов с типом VARCHAR ограничена 65 535 байтами для каждой таблицы, а на общую длину столбцов с типом TEXT ограничений нет. Недостатком типа TEXT является невозможность включать такие столбцы во внешний ключ таблицы, то есть создавать связь между таблицами на основе этих столбцов.

Столбец price (цена) имеет тип DECIMAL, предназначенный для хранения денежных сумм и других значений, для которых важно избежать ошибок округления. В скобках мы указали два числа: первое из них определяет максимальное количество цифр в значении столбца, второе – максимальное количество цифр после десятичного разделителя. 

CREATE TABLE Orders
(id SERIAL,
date DATE,
product_id BIGINT UNSIGNED NOT NULL,
qty INT UNSIGNED,
amount DECIMAL(10,2),
customer_id BIGINT UNSIGNED,
PRIMARY KEY (id),
FOREIGN KEY (product_id) REFERENCES Products (id)
ON DELETE RESTRICT ON UPDATE CASCADE,
FOREIGN KEY (customer_id) REFERENCES Customers (id)
ON DELETE RESTRICT ON UPDATE CASCADE)
ENGINE InnoDB CHARACTER SET utf8;


 Так, конструкция FOREIGN KEY (customer_id) REFERENCES Customers (id) означает, что в столбце customer_id могут содержаться только значения из столбца id таблицы Customers и неопределенные значения (NULL), а остальные значения запрещены. Для столбца product_id мы задали аналогичное ограничение и присвоили этому столбцу свойство NOT NULL, чтобы запретить регистрировать заказы с неопределенным товаром. 
Правило ON DELETE RESTRICT означает, что нельзя удалить запись о клиенте, если у этого клиента есть зарегистрированный заказ, и нельзя удалить запись о товаре, если этот товар был кем-то заказан. Правило ON UPDATE CASCADE означает, что при изменении номера клиента в таблице Customers или номера товара в таблице Products соответствующие изменения вносятся и в таблицу Orders


#### Типы данных в MySQL

+ числовые типы данных
  + BIT[(<Количествобитов>)]
		Битовое число, содержащее заданное количество битов. Если количество битов не указано, число состоит из одного бита.
  + TINYINT
		Целое число в диапазоне либо от -128 до 127, либо (если указано свойство UNSIGNED) от 0 до 255.
  + BOOL или BOOLEAN
		Являются синонимами к типу данных TINYINT(1) (число в скобках – это количество отображаемых цифр). При этом ненулевое значение рассматривается как истинное (TRUE), нулевое – как ложное (FALSE).
  + SMALLINT
		Целое число в диапазоне либо от -32 768 до 32 767, либо (если указано свойство UNSIGNED) от 0 до 65 535.
  + MEDIUMINT
		Целое число в диапазоне либо от -8 388 608 до 8 388 607, либо (если указано свойство UNSIGNED) от 0 до 16 777 215.
  + INT или INTEGER
		Целое число в диапазоне либо от -2 147 483 648 до 2 147 483 647, либо (если указано свойство UNSIGNED) от 0 до 4 294 967 295.
  + BIGINT
		Целое число в диапазоне либо от -9 223 372 036 854 775 808 до 9 223 372 036 854 775 807, либо (если указано свойство UNSIGNED) от 0 до 18 446 744 073 70 9 551 615.
  + SERIAL
		Синоним выражения BIGINT UNSIGNED NOT NULL AUTO_INCREMENT UNIQUE (большое целое число без знака, принимающее автоматически увеличиваемые уникальные значения; значения NULL запрещены). Используется для автоматической генерации уникальных значений в столбце первичного ключа.
  + FLOAT
		Число с плавающей точкой в диапазоне от -3,40282346638 до -1,175494351-38 и от 1,175494351-38 до 3,40282346638 (а также значение 0) с точностью около 7 значащих цифр (точность зависит от возможностей вашего компьютера).
  + DOUBLE, DOUBLE PRECISION или REAL
		Число с плавающей точкой в диапазоне от -1,7976931348623157308 до -2,2250738585072014-308 и от 2,2250738585072014-308 до 1,797693134862315738 (а также значение 0) с точностью около 15 значащих цифр (точность зависит от возможностей вашего компьютера).
  + FLOAT(<Точность>)
		При значении точности от 0 до 24 этот тип данных эквивалентен типу FLOAT, при значении от 25 до 53 – типу DOUBLE.
  + DECIMAL, DEC, NUMERIC или FIXED
		Точное (неокругляемое) число с фиксированной точкой. Может содержать до 65 значащих цифр и до 30 цифр после десятичного разделителя (по умолчанию – 10 значащих цифр и 0 после десятичного разделителя).
	Три свойства, которые можно указать для числовых столбцов:
		• UNSIGNED – данное свойство означает, что в столбце запрещены отрицательные (со знаком «-») значения. Указывать это свойство можно для любых столбцов с числовым типом данных, кроме BIT, BOOL (BOOLEAN) и SERIAL. Для целочисленных столбцов при добавлении свойства UNSIGNED максимально допустимое значение столбца увеличивается вдвое.
		• ZEROFILL – данное свойство означает, что значения при отображении будут дополнены нулями. Целые числа дополняются нулями слева в соответствии с указанным количеством отображаемых цифр, десятичные – слева и справа в соответствии с указанными точностью и шкалой. Например, если столбец определен как DOUBLE(10,5) ZEROFILL, то значение «12.23» отображается как «0012.23000». Кроме того, данное свойство запрещает отрицательные значения, как и свойство UNSIGNED. Указывать свойство ZEROFILL можно для любых столбцов с числовым типом данных, кроме BIT, BOOL (BOOLEAN) и SERIAL.
		• AUTO_INCREMENT – данное свойство обеспечивает автоматическую нумерацию строк таблицы. Это означает, что при добавлении в столбец неопределенного (NULL) или нулевого значения оно автоматически заменяется следующим номером, на единицу больше предыдущего (нумерация по умолчанию начинается с единицы, установить другой начальный номер можно с помощью соответствующего свойства таблицы). Указывать это свойство можно для любых столбцов с числовым типом данных, кроме BIT и DECIMAL (DEC, NUMERIC, FIXED). В таблице может быть только один столбец с таким свойством, и для него должен быть создан ключ или индекс (об этом вы узнаете в пункте «Ключевые столбцы и индексы»).

+  типы данных, используемые при хранении даты и времени
  + DATE
		Дата в формате «YYYY-MM-DD», в диапазоне от «0000-01-01» до «9999-12-31».
  + DATETIME
		Дата и время в формате «YYYY-MM-DD HH:MM:SS» в диапазоне от «0000–0101 00:00:00» до «9999-12-31 23:59:59»
  + TIMESTAMP
		Отметка времени в формате «YYYY-MM-DD HH:MM:SS» в диапазоне от «1970-01-01 00:00:00» до некоторой даты в 2038 г. При добавлении или изменении строки таблицы в столбце с типом TIMESTAMP автоматически устанавливается дата и время выполнения операции (если значение этого столбца не указано явно или указано неопределенное значение).
  + TIME
		Время в формате «HH:MM:SS» в диапазоне от «-838:59:59» до «838:59:59»
  + YEAR, YEAR(2), YEAR(4)
		Год в формате «YYYY» или «YY» (если количество цифр не указано, используется формат «YYYY»). Диапазон значений – от 1901 до 2155, если используется формат «YYYY», или от 70 (соответствует 1970 г.) до 69 (соответствует 2069 г.), если используется формат «YY».

+  символьные типы
  + CHAR(<Количество символов>) или NATIONAL CHAR(<Количество символов>)
		Символьная строка фиксированной длины. В таком столбце всегда хранится указанное количество символов, при необходимости значение дополняется справа пробелами. Вы можете задать количество символов от 0 до 255. Если количество символов не задано, используется длина строки по умолчанию – 1 символ. Тип данных NATIONAL CHAR отличается от CHAR тем, что для столбцов с типом NATIONAL CHAR используется кодировка UTF-8, в то время как для столбцов с типом CHAR можно указать любую кодировку, поддерживаемую MySQL.
  + VARCHAR(<Максимальное количество символов>) или NATIONAL
VARCHAR(<Максимальное количество символов>).
		Символьная строка переменной длины, содержащая не более указанного количества символов. Вы можете указать максимальное количество символов от 0 до 65 535, но не более 65 535 байтов в сумме для всех столбцов таблицы с типом CHAR, VARCHAR, BINARY или VARBINARY. Тип данных NATIONAL VARCHAR отличается от VARCHAR тем, что для столбцов с типом NATIONAL VARCHAR используется кодировка UTF-8, в то время как для столбцов с типом VARCHAR можно указать любую кодировку, поддерживаемую MySQL.
  + BINARY(<Количество байтов>)
		Байтовая (бинарная) строка фиксированной длины. Этот тип аналогичен типу CHAR, только строка содержит не символы, а байты, и значение меньшей длины дополняется справа не пробелами, а нулевыми байтами.
  + VARBINARY(<Максимальное количество байтов>)
		Байтовая (бинарная) строка переменной длины. Этот тип аналогичен типу VARCHAR, только строка содержит не символы, а байты.
  + TINYBLOB
		Байтовая (бинарная) строка переменной длины. Максимальная длина – 255 байтов.
  + TINYTEXT
		Символьная строка переменной длины. Максимальная длина – 255 байтов (не символов!).
  + BLOB[(<Максимальное количество байтов>)]
		Байтовая (бинарная) строка переменной длины. Если количество байтов не указано, то значение столбца ограничено 65 535 байтами. Если количество байтов указано, то создается столбец с типом данных TINYBLOB, BLOB, MEDIUMBLOB или LONGBLOB: выбирается тип данных с наименьшим размером, достаточным для хранения этого количества байтов.
  + TEXT[(<Максимальное количество символов>)]
		Символьная строка переменной длины. Если количество символов не указано, то значение столбца ограничено 65 535 байтами. Если количество символов указано, то создается столбец с типом данных TINYTEXT, TEXT, MEDIUMTEXT или LONGTEXT: выбирается тип данных с наименьшим размером, достаточным для хранения этого количества символов.
  + MEDIUMBLOB
		Байтовая (бинарная) строка переменной длины. Максимальная длина – 16 777 215 байтов.
  + MEDIUMTEXT
		Символьная строка переменной длины. Максимальная длина – 16 777 215 байтов.
  + LONGBLOB
		Байтовая (бинарная) строка переменной длины. Максимальная длина – не более 4 294 967 295 байтов (4 Гбайт), в зависимости от используемого протокола взаимодействия с сервером MySQL и доступных системных ресурсов.
  + LONGTEXT
		Символьная строка переменной длины. Максимальная длина – не более 4 294 967 295 байтов (4 Гбайт), в зависимости от используемого протокола взаимодействия с сервером MySQL и доступных системных ресурсов.
  + ENUM('<Значение 1>', '<Значение 2>',...)
		Строка, содержащая ровно один элемент из заданного списка. Например, если столбец определен как ENUM('a','b'), то допустимыми значениями этого столбца являются значения a, b и NULL (а также пустая строка «», которая может появиться при попытке вставки некорректного значения в данный столбец; о добавлении строк в таблицу и о возможных вариантах обработки некорректных значений пойдет речь в подразделе «Вставка отдельных строк»). В список вы можете включить до 65 535 элементов.
  +  SET('<Значение 1>', '<Значение 2>',...)
		Строка, содержащая любой набор элементов из заданного списка (в том числе пустой). Например, если столбец определен как SET('a','b'), то он может содержать значения «» (пустая строка), a, b, a,b и NULL. В список вы можете включить до 64 элементов. Элементы списка не должны содержать запятых. Каждый из элементов может присутствовать в значении столбца только один раз, причем элементы могут следовать только в том порядке, в котором они перечислены в списке. Например, при вставке значений a,b,a,b и b,a они автоматически преобразуются в значение a,b.
Чтобы имена клиентов хранились в кодировке CP-1251, тогда как кодировкой по умолчанию для таблицы Customers (Клиенты) является UTF-8, столбец name (имя) можно определить следующим образом: 
name VARCHAR(100) 
CHARACTER SET cp1251 COLLATE cp1251_general_ci

#### Свойства столбцов

+ NOT NULL
		Это свойство указывает, что в данном столбце не допускаются неопределенные значения (NULL).Если для столбца задано свойство NOT NULL, то, в частности, NULL не может использоваться в качестве значения по умолчанию для этого столбца. Значение по умолчанию, отличное от NULL, вы можете задать с помощью свойства DEFAULT <Значение>.Если же вы задали для столбца свойство NOT NULL, но не задали значение по умолчанию и не указали значение для этого столбца при вставке строки в таблицу, то поведение программы MySQL зависит от того, в каком режиме вы работаете
+ NULL
		Данное свойство указывает, что в столбце разрешены неопределенные значения(NULL). Задавать это свойство имеет смысл только для столбцов с типом TIMESTAMP, которые по умолчанию не допускают неопределенных значений. Остальные типы столбцов допускают неопределенные значения, если только для них не задано свойство NOT NULL.

+ DEFAULT <Значение>
		Данное свойство определяет значение по умолчанию для столбца, которое используется, если при вставке строки в таблицу значение столбца не задано явно. Значением по умолчанию может быть только константа; исключение составляют столбцы с типом TIMESTAMP, для которых в качестве значения по умолчанию можно задать переменную величину CURRENT_TIMESTAMP (текущую дату и время). Нельзя установить значение по умолчанию для столбцов с типом TINYBLOB, TINYTEXT, BLOB, TEXT, MEDIUMBLOB,MEDIUM-TEXT, LONGBLOB и LONGTEXT, а также для числовых столбцов, для которых задано свойство AUTO_INCREMENT. Кроме того, нельзя использовать неопределенное значение по умолчанию (NULL), если для столбца задано свойство NOT NULL.

		Например, чтобы задать для поля phone (телефон) таблицы Customers (Клиенты) значение по умолчанию, равное пустой строке, можно определить это поле следующим образом:
phone VARCHAR(20) DEFAULT ''

+ COMMENT 'Текст комментария'
		Произвольное текстовое описание столбца длиной до 255 символов. Например, описание для поля rating (рейтинг) таблицы Customers (Клиенты) можно задать следующим образом:
rating INT COMMENT 'Рейтинг клиента'


<a name="changingTheStructureOfATable"><h3>Изменение структуры таблицы</h3></a>

+ Добавить столбец
ALTER TABLE <Имя таблицы>
ADD <Имя столбца> <Тип столбца> [<Свойства столбца>]
[FIRST или AFTER <Имя предшествующего столбца>];

		В этой команде мы указываем имя таблицы, в которую добавляется столбец, а также имя и тип добавляемого столбца. Кроме того, можно определить место нового столбца среди уже существующих: добавляемый столбец может стать первым (FIRST) или следовать после указанного предшествующего столбца (AFTER). Если место столбца не задано, он становится последним столбцом таблицы.

ALTER TABLE Products ADD store VARCHAR(100) AFTER details;

+ Добавить первичный ключ
ALTER TABLE <Имя таблицы>
ADD [CONSTRAINT <Имя ключа>]
PRIMARY KEY (<Список столбцов>);

		При добавлении в таблицу первичного ключа мы указываем имя таблицы, в которую нужно добавить ключ, и имена столбцов, которые будут образовывать первичный ключ (эти столбцы должны уже существовать в таблице). 

ALTER TABLE Orders ADD PRIMARY KEY (id);

+  Добавить внешний ключ

ALTER TABLE <Имя таблицы>
ADD [CONSTRAINT <Имя внешнего ключа>]
FOREIGN KEY [<Имя индекса>] (<Список столбцов>)
REFERENCES <Имя родительской таблицы>
(<Список столбцов первичного ключа родительской таблицы>)
[<Правила поддержания целостности связи>];

		При добавлении в таблицу внешнего ключа мы указываем имя таблицы, в которую нужно добавить ключ, имена столбцов, которые будут образовывать внешний ключ (эти столбцы должны уже существовать в таблице), а также имя родительской таблицы (на которую будет ссылаться данная таблица) и имена столбцов, образующих первичный ключ в родительской таблице. В случае необходимости можно также задать имя создаваемого внешнего ключа, имя индекса, автоматически добавляемого для столбцов внешнего ключа, и правила поддержания целостности связи при удалении и изменении строк родительской таблицы.

ALTER TABLE Orders
ADD FOREIGN KEY (customer_id) REFERENCES Customers (id)
ON DELETE RESTRICT ON UPDATE CASCADE);

+ Добавить в таблицу обычный индекс

ALTER TABLE <Имя таблицы>
ADD INDEX [<Имя индекса>] (<Список столбцов>);
Добавить уникальный индекс – с помощью команды
ALTER TABLE <Имя таблицы>
ADD [CONSTRAINT <Имя ограничения>]
UNIQUE (<Список столбцов>);

+ Добавить полнотекстовый индекс 
ALTER TABLE <Имя таблицы>
ALTER TABLE <Имя таблицы>
ADD FULLTEXT [<Имя индекса>] (<Список столбцов>);

		При добавлении в таблицу индекса мы указываем имя таблицы, в которую нужно добавить индекс, и имена столбцов, включенных в индекс. В случае необходимости можно также задать имя индекса. Более подробно об индексах было сказано в пункте «Ключевые столбцы и индексы».

ALTER TABLE Products ADD INDEX (store);

+ Изменить столбец таблицы

ALTER TABLE <Имя таблицы>
CHANGE <Прежнее имя столбца >
<Новое имя столбца>
<Новый тип столбца> [<Свойства столбца>]
[FIRST или AFTER <Имя предшествующего столбца>];

		В этой команде мы указываем имя таблицы, текущее имя столбца, новое имя столбца (которое может совпадать с текущим), новый тип столбца (который также может совпадать с текущим типом), а также, при необходимости, свойства столбца (старые свойства при выполнении команды CHANGE удаляются). Кроме того, можно определить место столбца среди остальных столбцов таблицы: изменяемый столбец может стать первым (FIRST) или следовать после указанного предшествующего столбца (AFTER).

ALTER TABLE Products CHANGE store warehouse CHAR(100);

+ Удалить столбец таблицы

ALTER TABLE <Имя таблицы> DROP <Имя столбца>;

При удалении столбца он удаляется также из всех индексов, в которые он был включен
(в отличие от первичного и внешнего ключа, которые требуется удалить прежде, чем удалять
входящие в них столбцы). Если при этом в индексе не остается столбцов, то индекс также
автоматически удаляется.

ALTER TABLE Products DROP warehouse;

+ Удалить первичный ключ

ALTER TABLE <Имя таблицы> DROP PRIMARY KEY;

ALTER TABLE Orders DROP PRIMARY KEY;

+ Удалить внешний ключ

ALTER TABLE <Имя таблицы>
DROP FOREIGN KEY <Имя внешнего ключа>;

		В этой команде необходимо указать имя внешнего ключа. Если вы не задали имя внешнего ключа при его создании, то имя было присвоено автоматически и узнать его можно с помощью команды SHOW CREATE TABLE.

ALTER TABLE Orders DROP FOREIGN KEY orders_ibfk_1;
(здесь orders_ibfk_1 – имя внешнего ключа, автоматически присвоенное ему при создании).

+ Удалить индекс (обычный, уникальный или полнотекстовый)

ALTER TABLE <Имя таблицы> DROP INDEX <Имя индекса>;

		В этой команде необходимо указать имя индекса. Если вы не задали имя индекса при его создании, то имя было присвоено автоматически и узнать его можно с помощью команды SHOW CREATE TABLE

ALTER TABLE Customers DROP INDEX name;

+ Включить и отключить обновление неуникальных индексов в таблице с типом MyISAM

ALTER TABLE <Имя таблицы> DISABLE KEYS;
		Эта команда временно отключает обновление неуникальных индексов, что позволяет увеличить быстродействие операций добавления строк в таблицу.

ALTER TABLE <Имя таблицы> ENABLE KEYS;
Эта команда позволяет восстановить индексы после добавления строк

+ Переименовать таблицу вы можете с помощью команды

ALTER TABLE <Имя таблицы> RENAME <Новое имя таблицы>;

+ Упорядочить строки таблицы по значениям одного или нескольких столбцов

ALTER TABLE <Имя таблицы>
ORDER BY <Имя столбца 1> [ASC или DESC],
[<Имя столбца 2> [ASC или DESC],...];

		Упорядочение строк позволяет ускорить последующие операции сортировки по значениям указанных столбцов. Однако порядок строк в таблице может нарушиться после операций добавления и удаления строк. По умолчанию строки таблицы упорядочиваются по возрастанию значений. Вы можете выбрать направление сортировки, указав ключевое слово ASC (по возрастанию) или DESC (по убыванию).

+ Задать опциональные свойства таблицы 

ALTER TABLE <Имя таблицы> <Опциональное свойство таблицы>;
Например, изменить тип таблицы можно с помощью команды
ALTER TABLE <Имя таблицы> ENGINE <Новый тип таблицы>;

+ Изменить кодировку и правило сравнения символьных значений, используемых по умолчанию для новых символьных столбцов таблицы, можно, как любое другое опциональное свойство таблицы

ALTER TABLE <Имя таблицы>
CHARACTER SET <Имя кодировки>
[COLLATE <Имя правила сравнения>];

ALTER TABLE Products CHARACTER SET cp1251;

<a name="anotherCommands"><h3>Другие команды для работы с таблицами</h3></a>

+ Получить детальную информацию о конкретной таблице

DESCRIBE <Имя таблицы>;
или
SHOW CREATE TABLE <Имя таблицы>;

+ Просмотреть список таблиц текущей базы данных

SHOW TABLES;

+ Удалить таблицу

DROP TABLE <Имя таблицы>;

<a name="dataInput"><h2>ВВод данных в таблицу</h2></a>

<a name="loadingDataFromFile"><h3>Загрузка данных из файла</h3></a>
+ Загрузка данных из файла

		В созданном txt файле введите данные, используя для отделения значений друг от друга клавишу Tab, а для перехода на следующую строку – клавишу Enter. А для загрузки данных из созданного файла выполните команду
LOAD DATA LOCAL INFILE '/path/to/file.txt'
INTO TABLE Customers
CHARACTER SET cp1251;

		Однако если вам нужно загрузить в таблицу данные из текстового файла, который был создан в другом формате (например, выгружен из другой базы данных), могут потребоваться и другие параметры. Полностью команда LOAD DATA имеет следующий вид:

LOAD DATA [LOCAL] INFILE 'Путь и имя файла'
[REPLACE или IGNORE]
INTO TABLE <Имя таблицы>
CHARACTER SET <Имя кодировки>
[
FIELDS
[TERMINATED BY <Разделитель значений в строке>]
[[OPTIONALLY]
ENCLOSED BY <Символ, в который заключены значения>]
[ESCAPED BY <Экранирующий символ>]
]
[
LINES
[STARTING BY <Префикс строки>]
[TERMINATED BY <Разделитель строк>]
]
[IGNORE <Количество строк в начале файла> LINES]
[(<Список столбцов>)];

В этой команде вы можете использовать следующие параметры.
		• LOCAL – укажите этот параметр, если файл с данными находится на клиентском компьютере (то есть на том компьютере, где работает клиентское приложение, в котором вы и вводите эту команду). Если файл расположен на компьютере, где работает сервер MySQL, параметр LOCAL указывать не нужно.

		• REPLACE или IGNORE – укажите один из этих параметров, чтобы сообщить программе MySQL, как нужно обрабатывать загружаемую строку, если в таблице уже есть строка с таким же значением первичного ключа или уникального индекса. Если указан параметр REPLACE, то существующая в таблице строка заменяется новой. Если указан параметр IGNORE, новая строка в таблицу не загружается

		• CHARACTER SET <Имя кодировки> – укажите кодировку данных в файле. Этот параметр используется для корректного преобразования кодировок. Предполагается, что все данные в файле имеют одну и ту же кодировку.

		• FIELDS – укажите этот параметр, чтобы сообщить программе MySQL, в каком формате заданы значения в файле:

		– TERMINATED BY <Разделитель значений в строке> – укажем символ, разделяющий значения в строке файла. Например, если значения разделены запятыми, укажем параметр TERMINATED BY, если значения разделены символами табуляции – TERMINATED BY \t', если значения разделены косой чертой – TERMINATED BY /;
		– ENCLOSED BY <Символ, в который заключены значения> – укажем символ, которым обрамляются значения. Например, если все значения заключены в одинарные кавычки, укажем ENCLOSED BY \, если в одинарные кавычки заключены только символьные значения, укажем OPTIONALLY ENCLOSED BY \, а если никакие значения не обрамляются никакими символами, укажем ENCLOSED BY или вообще опустим этот параметр;
		– ESCAPED BY <Экранирующий символ> – укажем экранирующий символ (его также называют escape-символом). Этот символ сообщает программе MySQL, что следующий за ним символ нужно интерпретировать особым образом. А именно, обычный символ, следующий после экранирующего, будет рассматриваться как специальный символ, а специальный символ, наоборот, – как обычный символ
		Если параметр FIELDS не указан, программа MySQL считает, что значения в файле разделяются табуляцией и не заключаются ни в какие кавычки, а в качестве экранирующего символа используется обратная косая черта

		• LINES – укажите этот параметр, чтобы сообщить программе MySQL, в каком формате заданы строки в файле:
		– STARTING BY <Префикс строки> – укажем последовательность символов в начале каждой строки, которая должна игнорироваться программой вместе со всеми предшествующими символами. После префикса должны начинаться значения;
		– TERMINATED BY <Разделитель строк> – укажем символ, которым заканчиваются строки. Например, если строки заканчиваются символом перевода строки, укажем параметр TERMINATED BY \n', если символом возврата каретки – укажем TERMINATED BY \r', если сочетанием этих символов – укажем TERMINATED BY \r\n', если нулевым байтом – укажем TERMINATED BY \0

		Если параметр LINES не указан, программа MySQL считает, что строки в файле не имеют префикса и заканчиваются символом перевода строки «\n».

		• IGNORE <Количество строк в начале файла> LINES – укажите этот параметр, если первые несколько строк в файле не содержат значений (иными словами, являются заголовком) и при загрузке их нужно пропустить.


		• (<Список столбцов>) – перечислите столбцы таблицы, в которые будут загружаться данные. Это необходимо, если файл содержит данные не для всех столбцов таблицы или порядок следования значений в файле отличается от порядка столбцов в таблице

<a name="insertRows"><h3>Вставка отдельных строк</h3></a>

INSERT [INTO] <Имя таблицы>
[(<Список столбцов>)]
VALUES
(<Список значений 1>),
(<Список значений 2>),
...
(<Список значений N>);

	В команде INSERT используются следующие основные параметры.
• Имя таблицы, в которую добавляются строки.
• Список имен столбцов, для которых будут заданы значения. Если значения будут заданы для всех столбцов таблицы, то приводить список столбцов необязательно

<a name="retrievingDataFromATable"><h2>Извлечение данных из таблиц</h2></a>

<a name="simpleQueries"><h3>Простые запросы</h3></a>


  + Вывод всех данных, содержащихся в таблице
SELECT * FROM <Имя таблицы>;
		Вместо звездочки можно указать список столбцов таблицы, из которых нужно получить информацию.
SELECT name,phone,rating FROM Customers;
		Получать с помощью запроса можно не только значения столбцов, но и значения, вычисленные с помощью выражений.
SELECT name,phone,rating/1000 FROM CUSTOMERS;
		С помощью запросов можно также вычислять значения без обращения к какой-либо таблице.
Например, запрос
SELECT 2*2;
возвращает результат 4
  + Чтобы исключить повторения из результата запроса, добавьте в текст запроса ключевое слово DISTINCT
SELECT DISTINCT rating FROM Customers;

  + Чтобы упорядочить строки, выведенные запросом, по значениям одного из столбцов, добавьте в текст запроса выражение
ORDER BY <Имя столбца> [ASC или DESC]
		Ключевое слово ASC означает, что сортировка выполняется по возрастанию, DESC – по убыванию значений. Если ни то, ни другое слово не указано, выполняется сортировка по возрастанию. Кроме того, для сортировки можно использовать сразу несколько столбцов, тогда строки будут отсортированы по значениям первого из столбцов, строки с одинаковым значением в первом столбце будут отсортированы по значениям второго из столбцов и т. д.
SELECT name,phone,rating FROM Customers
ORDER BY rating DESC, name;

<a name="selectionConditions"><h3>Условия отбора</h3></a>

		Чтобы выбрать из таблицы строки, удовлетворяющие какому-либо критерию, добавьте в текст запроса выражение
WHERE <Условие отбора>
Например, запрос
SELECT name,phone,rating FROM Customers WHERE rating = 1000;
возвращает только те строки, в которых значение рейтинга равно 1000 
		В условии отбора можно использовать любые операторы и функции языка SQL, в том числе логические операторы AND и OR для создания составных условий отбора
Например, запрос
SELECT name,phone,rating FROM Customers
WHERE name LIKE 'ООО%' OR rating>1000
ORDER BY rating DESC;
		выводит информацию о тех клиентах, чье имя начинается с «ООО», а также о тех, чей рейтинг превосходит 1000, упорядочивая строки в порядке убывания значения рейтинга

<a name="concatenationOfTables"><h3>Объединение таблиц</h3></a>

		Получить информацию из нескольких таблиц вы можете, указав в запросе список столбцов и список таблиц, из которых нужно получить информацию:
SELECT <Список столбцов> FROM <Список таблиц>
WHERE <Условие отбора>;

		Например, если требуется вывести информацию о всех заказанных товарах за определенную дату с указанием имен и адресов заказчиков, выполните команду
SELECT name,address,product_id,qty
FROM Customers, Orders
WHERE Customers.id = customer_id AND date = '2007-12-12'

<a name="nestedQueries"><h3>Вложенные запросы</h3></a>

Например, получить список имен клиентов, когда-либо заказывавших товар № 5, можно с помощью вложенного запроса:
SELECT name FROM Customers
WHERE id IN
(SELECT DISTINCT customer_id FROM Orders
WHERE product_id = 5);

Здесь вложенный запрос получают из таблицы Orders (Заказы) номера клиентов, заказавших товар № 5. Для обработки результатов подзапроса мы применили оператор IN, который возвращает истинное значение (TRUE), если элемент слева от оператора совпадает с одним из элементов списка справа от оператора. В данном случае оператор IN проверяет, содержится ли номер клиента (значение столбца id) в списке номеров, выданных подзапросом. Таким образом, внешний запрос выводит имена тех клиентов, номера которых получены в результате подзапроса

<a name="combiningQueryResults"><h3>Объединение результатов запросов</h3></a>

Чтобы объединить несколько запросов в одну SQL-команду и, соответственно, объединить результаты запросов, используется ключевое слово UNION. При объединении результатов автоматически
удаляются повторяющиеся строки; чтобы запретить удаление повторяющихся строк, вместо слова UNION нужно использовать выражение UNION ALL. Наконец, строки объединенного запроса можно упорядочить с помощью выражения ORDER BY. В качестве примера рассмотрим запрос, выводящий информацию о заказах с наибольшей и наименьшей суммой заказа:
SELECT * FROM Orders
WHERE amount = (SELECT MAX(amount) FROM Orders)
UNION
SELECT * FROM Orders
WHERE amount = (SELECT MIN(amount) FROM Orders)
ORDER BY 1;

<a name="unloadingDataFromAFile"><h3>Выгрузка данных в файл</h3></a>

Чтобы результат запроса был сохранен в файл, добавьте в команду SELECT выражение INTO OUTFILE 'Путь и имя файла' [FILEDS ...] [LINES ...]

SELECT * from Customers INTO OUTFILE 'C:/data/Customers.txt';

<a name="dataChange"><h2>Изменение данных</h2></a>

UPDATE, которая позволяет установить новые значения в одной или нескольких строках, например, следующим образом:
UPDATE <Имя таблицы>
SET <Имя столбца 1> = <Значение 1>,
...,
<Имя столбца N> = <Значение N>
[WHERE <Условие отбора>]
[ORDER BY <Имя столбца> [ASC или DESC]]
[LIMIT <Количество строк>];
Например, если у клиента по фамилии Крылов изменился номер телефона, то обновить информацию в базе данных можно с помощью команды

UPDATE Customers SET phone = '444-25-27' WHERE id = 536;

Например, удвоить рейтинги для всех клиентов можно с помощью команды
UPDATE Customers SET rating = rating*2;

WHERE <Условие отбора> – укажите условие отбора, чтобы изменения были применены только к тем строкам таблицы, которые удовлетворяют этому условию. Если условие отбора не задано, изменения будут применены ко всем строкам таблицы. 

ORDER BY <Имя столбца> [ASC или DESC] – при необходимости вы можете указать, в каком порядке применять изменения к строкам таблицы. Обычно порядок применения изменений не влияет на результат выполнения операции. Однако в некоторых случаях последовательность действий может быть важна. 

 LIMIT <Количество строк> – при необходимости вы можете указать максимальное количество строк таблицы, которые могут быть изменены командой UPDATE. Если это количество измененных строк достигнуто, операция завершается, даже если в таблице еще остались строки, которые удовлетворяют условию отбора и не были изменены

REPLACE, осуществляющая либо добавление, либо замещение строк таблицы. Она имеет те же параметры, что и команда INSERT (см. подраздел «Вставка отдельных строк»):
REPLACE [INTO] <Имя таблицы>
[(<Список столбцов>)]
VALUES
(<Список значений 1>),
(<Список значений 2>),
...
(<Список значений N>);

команда удаления строк таблицы:
DELETE FROM <Имя таблицы>
[WHERE <Условие отбора>]
[ORDER BY <Имя столбца> [ASC или DESC]]
[LIMIT <Количество строк>];
Например, информацию о клиенте по фамилии Петров вы можете удалить из таблицы Customers с помощью команды
DELETE FROM Customers WHERE id = 534;