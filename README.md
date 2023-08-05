# ~ Storm Bridge ~ (rev0.1)

### Мультиинтерфейсный кроссплатформенный преобразователь USB на базе чипа FT232H.  

 ![StormBridgePCB](img/0.png)
 
## Содержание:  
- [Обзор основных функций](#основные-функции)  
- [Схема устройства](Shematic/Bridge.pdf)  
- [Назначение боковых микропереключателей и разъемов](#микропереключатели-и-разъемы)
- Инструкция по эксплуатации Linux
  - [Прошивка SPI-Flash памяти с помощью flashrom v 1.3+](#прошивка-spi-flash-памяти-с-помощью-flashrom-v-13)
  - [Прошивка и отладка микроконтроллеров через OpenOCD](#прошивка-и-отладка-микроконтроллеров-через-openocd)
  - Опрос I2C устройства с помощью библиотеки PyFTDI  
- Полезные ссылки и файлы


## Основные функции  
Возможность высокоскоростной аппаратной работы с интерфейсами UART, SPI, I2C, JTAG/SWD, GPIO.   
Имеется возможность принудительного сброса устройства без извлечения USB коннекторов.  
Присутствует индикация основных режимов работы устройства.  
Связь с хостом осуществляется через разъем USB-TypeC.  
Компактные размеры 44x44x15мм.  

UART:
- Скорость работы до 12Мбод/с. Поддержка 7/8 битный обмен, с 1/2 стоповыми битами.
- Возможность обмена по UART через изолятор с выбором референсного напряжения обмена через пин "VCC" колодки "UART TTL".
- Референсное напряжение изолированной стороны может варьироваться от 5.5 до 3 вольт (до 2.8 в модификации с изолятором ISO7221CDR).
- Возможность принудительного выбора напряжения обмена между 3.3 и 5 вольт и принудительного выключения гальванической развязки, принудительного выключения микросхемы изолятора.
- Возможность обмена по UART на пониженном напряжении через преобразователь логических уровней, выбор напряжения обмена может выбираться автоматически через пин VCC референсного напряжения колодки "LOW VOLTAGE SPI", либо выбираться принудительно между 2.5 и 1.8 вольт.
- Так же имеются расширенные сигналы UART такие, как RTS / CTS / DTR / DSR / DCD доступные на колодке "GPIO" работающие на напряжении 3.3в (толерантны к 5в).

SPI:
- Возможность обмена по интерфейсу SPI частоте до 30МГц.
- Напряжение сигнальных линий через колодку "GPIO" - 3.3в (толерантны к 5в).
- Возможность внутрисхемного программирования микросхем памяти SPI с низковольтными логическими уровнями. 
- Для работы с низкими логическими уровнями в устройстве предусмотрен двунаправленный преобразователь логических уровней. 
- Выбор референсного напряжения может происходить автоматически через пин "VCC" колодки "LOW VOLTAGE SPI", в диапазоне от 1.2 до 3 вольт.
- Возможность принудительного выбора напряжения обмена на колодке "LOW VOLTAGE SPI" между 1.8 и 2.5 вольт.
- Предусмотрена схема автоматического перевода сигнальных линий преобразователя в высокоимпедансное состояние, в те моменты, когда не производится запись, для исключения влияния устройства на работу внутрисхемной памяти.

I2C:
- Возможность обмена по интерфейсу I2С на напряжении от 1.2 до 5 вольт.
- Возможность принудительной подтяжки линий SDA / SCL к напряжению 3.3 вольта.

JTAG/SWD:
- Возможность внутрисхемного программирования и отладки микроконтроллеров имеющих возможность работать через интерфейс SWD и JTAG (TCK,TDI,TDO,TMS) через GDB/OpenOCD. 
- Краткий список поддерживаемых устройств приведен на сайте OpenOCD и выглядит следующим образом:
>Debug targets:
ARM: AArch64, ARM11, ARM7, ARM9, Cortex-A/R (v7-A/R), Cortex-M (ARMv{6/7/8}-M),
FA526, Feroceon/Dragonite, XScale.
ARCv2, AVR32, DSP563xx, DSP5680xx, EnSilica eSi-RISC, EJTAG (MIPS32, MIPS64),
ESP32, ESP32-S2, ESP32-S3, Intel Quark, LS102x-SAP, RISC-V, ST STM8,
Xtensa.  
Flash drivers:  
ADUC702x, AT91SAM, AT91SAM9 (NAND), ATH79, ATmega128RFA1, Atmel SAM, AVR, CFI,
DSP5680xx, EFM32, EM357, eSi-RISC, eSi-TSMC, EZR32HG, FM3, FM4, Freedom E SPI,
GD32, i.MX31, Kinetis, LPC8xx/LPC1xxx/LPC2xxx/LPC541xx, LPC2900, LPC3180, LPC32xx,
LPCSPIFI, Marvell QSPI, MAX32, Milandr, MXC, NIIET, nRF51, nRF52, NuMicro,
NUC910, Nuvoton NPCX, onsemi RSL10, Orion/Kirkwood, PIC32mx, PSoC4/5LP/6,
Raspberry RP2040, Renesas RPC HF and SH QSPI,
S3C24xx, S3C6400, SiM3x, SiFive Freedom E, Stellaris, ST BlueNRG, STM32,
STM32 QUAD/OCTO-SPI for Flash/FRAM/EEPROM, STMSMI, STR7x, STR9x, SWM050,
TI CC13xx, TI CC26xx, TI CC32xx, TI MSP432, Winner Micro w600, Xilinx XCF,
XMC1xxx, XMC4xxx.  

GPIO:
- Возможность использовать порты ввода вывода AD0-AD7 на колодке GPIO в качестве портов общего назначения.  
  

## Микропереключатели и разъемы  
  
 ![StormBridgeDIP](img/4.png)
  

### **SW:1-10** 
микропереключатели выбора работы устройства. ***Положение активации - сдвинутое вниз, к плате***.  
  

### **Разъем P1**
Служит для подключения SPI и других интерфейсов через преобразователь уровней.  
Изначально выводы данного разъема спроектированы таким образом, чтобы не мешать работе подключенных к нему устройств и находятся в высокоимпедансном состоянии.  
Активация данного разъема может происходить автоматически, с помощью низкого уровня напряжения на выводе GPIO-AD7 (Доп. параметр для flashrom v1.3+ - *gpiol3=L* ).
Имеется возможность принудительной активации разъема с помощью микропереключателя SW:3.  
Выбор напряжения обмена происходит автоматически и равно напряжению приложенному к пину VCC в диапазоне от 1.2 до 3 вольт.
Также можно не подключать пин VCC, а выбрать напряжение обмена принудительно с помощью микропереключателей SW:1(1.8v), либо SW:2 (2.5v).
При включенном положении данных переключателей пин VCC становится выходом и может питать подключенное устройство заданным напряжением (ток не более 100мА).

:warning: **Осторожно** :exclamation: :
- Одновременное включение SW:1 и SW:2 на работающем устройстве, может привести к его поломке.
- Подача напряжения из вне на пин VCC при включенных SW:1, или SW:2, также может привести к поломке устройства.

Соответствие пинов разъема GPIO:
| SPI  | GPIO |
| ---- | ---- |
| SCLK | AD0  |
| MOSI | AD1  |
| MISO | AD2  |
| CS_0 | AD3  |  
  

### **Разъем P2**
Служит для подключения изолированного UART.  
Активация данного разъема происходит с помощью микропереключателя SW:4.
Выбор напряжения обмена происходит автоматически и равно напряжению приложенному к пину VCC в диапазоне от 3 до 5 вольт.

Также можно не подключать пин VCC, а выбрать напряжение обмена принудительно с помощью микропереключателей SW:5(3.3v), либо SW:6 (5v). При включенном положении данных переключателей пин VCC становится выходом и может питать подключенное устройство заданным напряжением (ток не более 100мА для 3.3v и не более 400мА для 5v).  
:exclamation: В данном режиме теряется гальваническая изоляция, а микропереключатель SW:7(GND) должен быть активирован в обязательном порядке.

:warning: **Осторожно** :exclamation: :
- Одновременное включение SW:5 и SW:6 на работающем устройстве, может привести к его поломке. 
- Разъем может активироваться от фантомного питания, через пины RX-TX с внешней стороны и даже передавать информацию с поднятым переключателем SW:4. Корректная работа в таком режиме не может быть гарантирована.

Соответствие пинов разъема GPIO:
| UART | GPIO |
| ---- | ---- |
| TX   | AD0  |
| RX   | AD1  |  

:exclamation: пропускание сигналов ограничено направленностью изолятора
  

### **Разъем P3**
Служит для подключения любых поддерживаемых интерфейсов. (UART, SPI, I2C, JTAG, GPIO).  
Уровни напряжений на данном разъеме 3.3 вольта, толерантны к 5 вольтам. Для удобства подключений, каждый функциональный пин данного разъема продублирован пином GND (ряд у края платы).  
Для некоторых режимов работы требуется соединение между собой контактов AD1 и AD2. Для удобства, данный функционал предусмотрен переключателем SW:10(SDA2).  
Для работы интерфейса I2C предусмотрена подтяжка к напряжению 3.3 вольта, которая активируется с помощью переключателей SW:8(SCL) и SW:9(SDA1).  
Для питания внешних устройств предусмотрены пины 3.3 и 5 вольт (ток не более 100мА для 3.3v и не более 400мА для 5v).  

:warning: **Осторожно** :exclamation: :
- пин AD7 подтянут к 3.3в через 51кОм, а так же служит катодом для светодиода "Flash".

Функциональное назначение:  
| GPIO   | UART   | SPI    | JTAG   | I2C    |
| ------ | ------ | ------ | ------ | ------ |
| AD0    | TX     | SCLK   | TCK    | SCK    |
| AD1    | RX     | MOSI   | TDI    | SDA/O  |
| AD2    | RTS    | MISO   | TDO    | SDA/I  |
| AD3    | CTS    | CS     | TMS    |        |
| AD4    | DTR    | CS1    |        |        |
| AD5    | DSR    | CS2    |        |        |
| AD6    | DCD    | CS3    |        |        |
| AD7    | RI     | CS4    | RCLK   | RSCK   |


# Инструкция по эксплуатации в Linux

:warning: ***Предполагается что пользователь, следующий данной инструкции, имеет базовые представления о том как пользоваться консолью Linux, в том числе с повышенными привилегиями, устанавливать пакеты в своей системе, пользоваться поисковыми системами для устранения возможных сообщений об ошибках.***

## Прошивка SPI-Flash памяти с помощью flashrom v 1.3+

Для начала нужно собрать ***flashrom*** версии не ниже **1.3** из [исходного кода](https://github.com/flashrom/flashrom), перед сборкой в системе необходимо иметь библиотеки ***libftdi*** и ***libusb***.  
Если в системе уже есть *flashrom* другой версии, для удобной работы можно создать симлинк собранной версии программы, но с другим именем в директории ```/usr/bin```, например команда ```# ln -s flashrom /usr/bin/flashrom-dev```, запущенная в директории с собранной программой, создаст симлинк и позволит использовать команду ```flashrom-dev``` из любого каталога.
Подключение к чипу памяти производится согласно [назначению разъемов](#микропереключатели-и-разъемы).
Команда для проверки наличия обмена с чипом памяти, с автоматической активацией преобразователя логических уровней:
```
flashrom-dev -p ft2232_spi:type=232H,divisor=2,gpiol3=L
```
где:
```-p ft2232_spi:type=232H,divisor=2,gpiol3=L``` - выбор программатора и параметров обмена;  
параметр ```divisor=2``` - настройка частоты обмена по формуле ```60_Мгц / divisor```, **divisor** может быть только целым четным числом от **2 до 2^17 (= 131072)**. Стабильная частота для конкретного чипа и шлейфа подбирается опытным путем, рекомендуемо для внутрисхемной прошивки выбирать **divisor** не меньше 6 (то есть 10_Мгц и меньше);  
параметр ```gpiol3=L``` - активация преобразователя логических уровней, через **gpio AD7**.  

:fire: Для работы с чипами рекомендуется ознакомиться с [инструкцией на flashrom](https://github.com/flashrom/flashrom/blob/master/doc/classic_cli_manpage.rst), среди удобных функций рекомендуется обратить внимание на возможность работы с регионами памяти через файл layout.  

Иногда для корректной работы нужно указать непосредственно название чипа памяти, о чем подскажет вывод в консоль, например:  
```
Found Micron/Numonyx/ST flash chip "N25Q512..1G" (65536 kB, SPI) on ft2232_spi.
Found Micron flash chip "MT25QU512" (65536 kB, SPI) on ft2232_spi.
Multiple flash chip definitions match the detected chip(s): "N25Q512..1G", "MT25QU512"
Please specify which chip definition to use with the -c <chipname> option.
```
в данном случае следует выбрать чип из предложенных, например ```... -с MT25QU512```

пример команды для прошивки с верификацией:
```flashrom-dev -p ft2232_spi:type=232H,divisor=10,gpiol3=L -c "MT25QU256" -w e01-lvds.bin --progress```
где ```e01-lvds.bin``` файл прошивки, а ключ ```--progress``` позволяет видеть прогресс загрузки  

## Прошивка и отладка микроконтроллеров через OpenOCD

*Для примера выбрана IDE VS-Code и микроконтроллер STM32F103C8T6. IDE и контроллер могут быть любыми, которые поддерживают отладку через OpenOCD.*  
*Начальный код сгенерирован STM32CubeMX*  
*Действия производились в ArchLinux на ядре 6.4.8-arch1-1 (64-бита), с установленным пакетным менеджером* **yay**  

```
sudo pacman -Sy openocd $(pacman -Ssq arm-none-eabi) clang ninja cmake llvm meson
yay -Sy visual-studio-code-bin stm32cubemx
```
Далее устанавливаются расширения в VS-Code:  
```
ms-vscode.cpptools-extension-pack
marus25.cortex-debug
mcu-debug.memory-view
rioj7.command-variable
actboy168.tasks
cschlosser.doxdocgen
```
### Начальный код генерируется в CubeMX:
**file -> new project ->**  
**(в строке поиска открывшегося селектора) commercial part number: stm32f103c8t6**  
в правом нижнем углу появляются результаты поиска, выделяется нужный МК, далее вкладка **docs & resources**, находится **system view description**, скачивается архив с svd файлами, которые понадобятся для проекта.  
Далее кнопка **start project** -> *конфигурация МК в зависимости от отладочной платы, или проекта.*  
Для удобства отладки и снятия необходимости переводить плату в "загрузочный" режим сразу настраивается отладочный интерфейс **system core -> sys -> debug: serial wire**.  
После настройки интерфейсов, модулей и используемых частот, вкладка **project manager ->** вкладка **project**  
**project name: projectname**  
**project location: .../stm32projects**  
**toolchain/ide: makefile**  
Вкладка **code generator -> stm32cube mcu packages and embedded software packs**  
тут можно выбрать копировать файлы библиотек в каталог, или использовать ссылки на библиотеки.  
Далее кнопка **generate code**. На этом CubeMX можно закрыть.  

### Настройка задач отладки в VS-Code:
В каталог с проектом .../stm32projects/projectname копируется скачанный ранее файл svd под нужную серию МК (stm32f103.svd).  
В нем же создается подкаталог ```.vscode```, внутри которого создаются файлы ```launch.json``` и ```tasks.json``` со следующим содержимым:  

<details>
<summary>launch.json</summary>

```json
{
	"version": "2.0.0",
	"configurations": [
		{
			"name": "OCD-Debug",
			"type": "cortex-debug",
			"request": "launch",
			"cwd": "${workspaceRoot}",
			"servertype": "openocd",
			"executable": "./build/${workspaceFolderBasename}.elf",
			"svdFile": "STM32F103.svd",
			"configFiles": [
				"interface/ftdi/ft232h-module-swd.cfg",
				"target/stm32f1x.cfg"
			],
			"runToEntryPoint": "main",
			"showDevDebugOutput": "none",
			"preLaunchTask": "Build"
		},
		{
			"name": "OCD-Flash",
			"type": "cortex-debug",
			"request": "launch",
			"cwd": "${workspaceRoot}",
			"servertype": "openocd",
			"executable": "./build/${workspaceFolderBasename}.elf",
			"svdFile": "STM32F103.svd",
			"configFiles": [
				"interface/ftdi/ft232h-module-swd.cfg",
				"target/stm32f1x.cfg"
			],
			"preLaunchTask": "Write firmware"
		}
	]
}

```
</details>

<details>
<summary>tasks.json</summary>

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "Build",
            "type": "shell",
            "group": "build",
            "command": "make",
            "problemMatcher": []
        },
        {
            "label": "Clean",
            "type": "shell",
            "group": "build",
            "command": "make clean",
            "problemMatcher": []
        },
        {
            "label": "Write firmware",
            "type": "shell",
            "command": "make clean \n make \n openocd -f interface/ftdi/ft232h-module-swd.cfg -f target/stm32f1x.cfg -c 'program ./build/${workspaceFolderBasename}.bin verify reset exit' ",
            "problemMatcher": []
        }
    ]
}
```
</details>

После этого каталог с проектом открывается в vscode через контекстное меню. Подтверждается доверие авторам файлов в каталоге.  

### Подключение МК и отладка
Подключение МК к устройству производится согласно [назначению разъемов](#микропереключатели-и-разъемы).
В случае с STM32F1 это SWD интерфейс:  
| GPIO    | STM32 |
| ------- | ----- |
| AD0     | SWCLK |
| AD1/AD2 | SWDIO |
| 3.3v    | 3.3v  |
| GND     | GND   |  

Чтобы объединить **AD1** и **AD2** активируется микропереключатель **SW:10(SDA2)**.  

После этого устройство подключается к хосту.  
В VS-Code во вкладке **Запуск и отладка** должны быть доступны созданные ранее задачи: **OCD-Debug** и **OCD-Flash**.  
* ***OCD-Debug*** - стандартная сборка и отладка проекта.  
* ***OCD-Flash*** - может пригодиться для прошивки МК без отладки, но сессию все равно придется закрывать вручную.  
