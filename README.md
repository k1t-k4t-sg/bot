## Документация по работе с @DomofonRed_bot

Для доступа обратитесь к администратору

### 1. Доступные параметры при выборе домофона
- `ID` - Идентификатор в системе россдомофон
- `Numbering` - Нумерация квартир
- `AnswerLevel` - Общий уровень поднятия трубки
- `DoorLevel` - Общий уровень нажатия кнопки
- `DoorIime` - Время открытия двери
- `MainDoor=AltDoor` - Состояние замков ***(included-включен) (disconnected-отключен)***
- `Uptime` - Время работы домофона (дата.час:мин:сек)
- `Error` - Сообщение об ошибке в работе домофона, счетчик собирает данные с syslog
- `Label` - ***(только mifare)*** Количество записанных ключей
- `ScanCode` - Код и состояние сканирования ключей, если указан код и параметр ***=>on***, вы можете добавить ключи по набору кода
- `KeyReverse` - ***(только mifare)*** Порядок сканирования ключей (off-обратный), (on-прямой)
- `DoorCode` - Доступна ли функция открытия двери по коду ***(=>on доступен) (=>off не доступен)***
- ![Image alt](https://github.com/stepanger/bot/blob/main/DE.jpg)
  
### 1.1 Параметры при выборе квартиры

- `Apartament` - Номер квартиры
- `Level` - Уровень линии до квартиры
- `KKM` - Кросс данные
- `Active CMS` - Разрешение на выполнение звонков и дублирование звонка на CMS
- `Block CMS` - Блокировка вызова в квартиру
- `PhoneX` - Сопоставление квартиры для службы
- `UP Level` - Индивидуальный уровень поднятия трубки
- `Open Level` - Индивидуальный уровень нажатия кнопки

### 1.2 Доступные функции

- `List` - Список установленных домофонов на сети
- `D-{}` - Выбрать домофон для дальнейшей операции
- `Open` - Открыть основную и дополнительную дверь
- `Full` - Основные переменные домофона
- `Log`  - log журнал
- `Key`  - Включить сканирование ключей
- `Get`  - Запросить последний записанный ключ 
- `L-{}` - Запросить проверку линии до квартиры
- `Dial-{}` - Выполнить тестовый звонок
- `{addr}` - Поиск адреса
- `add-{key}` - Добавить ключ на все панели

### 1.3 Порядок работы с ботом
#### 1.3.1 Выбор панели

Для начала работы введите первые три буквы адреса.

Результатом будет отправлено все похожие адреса и их **ID**

```sh
=> Муз
18948  Музыкина д.4 Под 1 
18949  Музыкина д.4 Под 2 
18950  Музыкина д.4 Под 3 

```

Для выбора панели введите префикс (**D-**) и (**ID номер**) панели 

Например: У адреса Музыкина д.4 Под 1, команда будет **(D-18948)**

```sh
=> D-18948

ID:     18948
Addr:   Музыкина д.4 Под 1
Uptime: 12.14:22:12
Error:  5
Label:  127 key

   Numbering: 1-40
 AnswerLevel: 330
   DoorLevel: 430

    ScanCode: 0=>off
  KeyReverse: off

    DoorCode: 12345=>on
    MainDoor: included
     AltDoor: included

```


Для описания параметров обратитесь к разделу ```1```.
После выбора панели вам доступны функции:
- `Open` - `Full` - `Log`- `Key` - `Get` - `L-{}`- `add-{key}`

#### 1.3.1 Выбор квартиры
По диапазону ```Numbering``` выберете квартиру командой `L-номер квартиры`.

Например, для выбора 1 квартиры по адресу Музыкина д.4 Под 1, наберите: `L-1`

```sh
=> L-1

Apartament: 1
     Level: 562
       KKM: KKM1 E1|D0 = 1.Apar 

Active CMS: on
 Block CMS: off
    Phone1: 1

  UP Level: 330
Open Level: 430

```
Для описания параметров обратитесь к разделу ```1.1```.
После выбора квартиры вам доступны функции:
- `L-{}` - `Dial-квартира` 

### 1.4 Диагностика подключения панели
#### 1.4.1 Параметр ```Uptime```

Данное значение определяет время работы домофона в формате (дата.час:мин:сек) если значение времени слишком маленькое то может указывать на проблему с основным питание, недавней перезагрузкой или замыкание в слаботочной сети. При замыкании, панель уходить в перезагрузку, что может способствовать сбросу настроек

#### 1.4.1 Параметр ```Error```

Сообщения об ошибке собираются с log журнала домофона методом общего подсчета события ```</ ERROR>```,
если вы наблюдаете большое количество зарегистрированных ошибок, запросите команду  ```log``` для диагностики и/или передайте проблему в ОСБ

#### 1.4.1 Параметр ```Label```

Количество записанных ключей, в случае нулевого значения может указывать на сброс настроек домофона, или в базе нет ключей 

#### 1.4.1 Параметр ```MainDoor``` и ```AltDoor```

Указывает на состояние замков, в нормальном режиме работы состояние замков в положение (```included```-включен).
В отключенном состояние параметр будет указывать (```disconnected```-отключен). Обычно отключенные замки задаются программно в новостройках, в остальных случаях передайте проблему в ОСБ

### 1.4 Диагностика подключения трубки
#### 1.4.1 Параметр ``` Level```
Уровень трубки в сети. Нормальным значение варьируется от 200 до 300 с небольшой погрешностью, в остальных случаях трубка не подключена или повреждена магистраль, плохой контакт. При обнаружении уровня 200-300 с отключенной трубкой указывают на параллельное подключение второй трубки,  данная проблема указывает на повреждение магистрали, передайте проблему на ОСБ. 

#### 1.4.1 Параметр ``` KKM```
Данные кроссировки загружаются с пресета домофона, значения ```KKM1``` ```KKM2``` ```KKM3``` указывает какая сотня, используется в вашем подключение. 
#### 1.4.1 Параметр ``` Active CMS```
Разрешает переадресацию вызова на сторонние сервисы, в случае значения OFF. Звонки в приложения поступать не будут
#### 1.4.1 Параметр ``` Block CMS```
Блокировка вызова на трубку и сторонние сервисы. Звонки в приложения поступать не будут
#### 1.4.1 Параметр ``` Phone1 ```
Сопоставление квартиры для сторонних сервисов, если значение не указано, звонки в приложение поступать не будут
#### 1.4.1 Параметр ``` UP Level```
Индивидуальный уровень поднятия трубки
#### 1.4.1 Параметр ``` Open Level```
Индивидуальный уровень нажатия кнопки на трубке
