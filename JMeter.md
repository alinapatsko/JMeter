Установка, настройка и работа JMeter
===================================
**1. Установка Java**

1) **Проверка наличия Java на компьютере.**

В консоли написать команду **java -version**. В ответе можно увидеть версию программы, если она есть.
Если Java не установлена скачать с официального сайта: https://www.oracle.com/java/technologies/javase-downloads.html.

После установки настроить **окружение Java**:
нажать правой клавишей на **Мой компьютер**, выбрать **свойства --> Изменить параметры** (справа) --> **Дополнительно --> Переменные среды** --> в поле **Переменные среды пользователя** нажать кнопку **Создать** --> в поле **Имя переменной** создать переменную среды, чтобы она указывала на местоположение базовой директории, ввести **JAVA_HOME**, в **Значение переменной** ввести путь установки Java (например, E:\\java\...\jdk-15.0.2) > **OK** >
в поле **Системные переменные** найти строку **Path**, нажать кнопку **Изменить** --> в конце строчки поставить ; (без пробела), указать путь к папке bin, где установлена Java (например, E:\\java…\bin) --> **ОК.**
Теперь нужно проверить наличие Java - в консоли написать команду **java -version.**

2) **Установка и запуск JMeter.**

Загрузить последнюю версию JMeter (Binaries) с официального сайта: https://jmeter.apache.org/download_jmeter.cgi.

После загрузки JMeter перейти **каталог bin**, выбрать файл jmeter.bat (можно jmeterw.cmd), запустить файл, загрузится главная страница JMeter.

**2. Работа в JMeter**

1) **Создать Thread Group** – сценарий определенных действий, который выполняет пользователь (поток).

Проверить действия пользователя, которые он выполняет несколько раз. Проверка не требует, чтобы пользователь был зарегистрирован. 
**Test Plan** --> нажать правой кнопкой мыши --> **Add --> Threads (Users) --> Thread Group.** 
После создания **Thread Group** отобразится страница. 
- В поле **Thread Group**, в строке **Name**, дать имя (например, Thread Group), выбрать радиобаттон **Continue**.
- В поле **Thread Properties** в поле **Number of Threads (users)** нужно указать число пользователей, которые будут обращаться по запросу, указанному в HTTP Request Defaults (например, 100).
- В поле **Ramp-up period (seconds)** нужно указать время, в течение которого будут прибавляться пользователи. Позволяет делать плавный и прогнозируемый старт (например, 5).
- В поле **Loop Count** указать количество итераций (повторений) запросов (например, 2). Если поставить галочку на **Infinitе**, тогда запросы будут повторяться бесконечно. 

2) **Создать HTTP Request Defaults** – шапка всех последующих запросов, нужно один раз указать ссылку, порт и метод, чтобы в следующих запросах не указывать их.
**Thread Group** (имя, которое дали) --> нажать правой кнопкой мыши --> **Add --> Config Element --> HTTP Request Defaults.**
В строке **Name** дать имя (например, First). В строке **http [http]** указать протокол. В строке **Server name or IP** указать ссылку или IP-адрес, куда нужно обращаться. В строке **Port Number**указать порт.

3) **Создать запросы.**

**Thread Group** (имя, которое дали) > нажать правой кнопкой мыши --> **Add --> Sampler --> HTTP Request.**
В строке **Name** дать имя (например, The first HTTP request). Из предложенных методов выбрать метод GET. В строке **Path** указать эндпоинт.

4) **Запуск запросов.**

Для того, чтобы увидеть результат выполнения запросов необходимо добавить инструмент **Listener (слушатели)**, он «слушает», что происходит при выполнении сценария.
**Thread Group** (имя, которое дали) --> нажать правой кнопкой мыши --> **Add --> Listener** --> (можно добавлять «слушателей» в процессе работы) -->
- **View Result Tree** - отобразятся потоки. Можно посмотреть содержимое запроса (запросы и ответы).
- **Summary Report** - отобразится статистика. В столбце *Average* отображается среднее время ожидания ответа от сервера в миллисекундах. В столбце *Throughput* пропускная способность сервера (сколько запросов в секунду может обрабатывать сервер). В столбце *Error* отображается процент ошибок. 
- **View Result in Table** - можно посмотреть время каждого запроса.
Теперь нагрузочный тест готов. Чтобы посмотреть результаты – нажать на кнопку **Start**. 

**3. Работа с авторизованными пользователями.**

Перед тем, чтобы настроить плагин Stepping Thread Group (jp@gc), необходимо авторизовать пользователя (получить токен).

1) **Создание Thread Group.**

**Test Plan** --> нажать правой кнопкой мыши --> **Add --> Threads (Users) --> Thread Group**. 
После создания **Thread Group** отобразится страница. В поле **Thread Group**, в строке **Name**, дать название (например, Login), выбрать радиобаттон **Continue**.
В поле **Thread Properties** в строке **Number of Threads (users)**, **Ramp-up period (seconds), Loop Count** нужно в каждой строке указать 1. Нужно для того, чтобы пользователь один раз авторизовался и передал этот токен всем остальным пользователям 1 раз.

2) **Создать HTTP Request Defaults** – указать ссылку, порт и метод, чтобы в следующих запросах не указывать их.

**Thread Group** (например, Login) --> нажать правой кнопкой мыши --> **Add --> Config Element --> HTTP Request Defaults**.
В строке **Name** дать название (например, First). В строке **http [http]** указать протокол. В строке **Server name or IP** указать ссылку или IP-адрес, куда нужно обращаться. В строке **Port Number** указать порт.

3) **Создание запроса**.

**Thread Group** (например, Login) --> нажать правой кнопкой мыши --> **Add --> Sampler --> HTTP Request**.
В строке **Name** дать имя (например, login). Из предложенных методов выбрать метод POST. В строке **Path** указать эндпоинт.
В параметрах запроса прописать **login и password** пользователя.
На странице запроса **login --> Add** (внизу):
Name: login, Value: alinappp 
Name: password, Value: qwer123

4) **Создание переменной окружения, токена и добавление токена в переменную окружения**.

В этом же запросе создать **JSON Extractor**. Для того, чтобы создать переменную.
**Thread Group** (например, Login) --> нажать правой кнопкой мыши --> **Add --> Post Processors --> JSON Extractor**.
В строке **Name** дать название (например, JSON Extractor).
В строке **Names of created variables** – создать переменную (token).
В строке **JSON Path expressions** – адрес переменной ($.token).
В строке **Match No. (0 or Random)** – сколько раз вызвать переменную (1).

Теперь нужно **передать JSON в переменную окружения**.
**Thread Group** (например, Login) --> нажать правой кнопкой мыши --> **Add --> Assertions --> BeenShell Assertions**.
Написать код, который добавит токен в переменную окружения:
```
${__setProperty(token,${token})}
```

**4. Установка и настройка плагина Stepping Thread Group (jp@gc)**

Скачать плагин https://jmeter-plugins.org/wiki/PluginInstall/.
После загрузки поместить файл в apache-jmeter-5.4.1/lib/ext, затем перезапустить JMeter. 

1) **Добавить плагин Stepping Thread Group для дальнейшей работы (нужен для создания ступенчатых тестов)**

**Test Plan** --> нажать правой кнопкой мыши --> **Add --> Threads (Users) --> jp@gc – Stepping Thread Group**
После добавления **jp@gc – Stepping Thread Group** отобразится страница, поля которой в дальнейшем нужно заполнить данными.

2) **Создать HTTP Request Defaults**. 

**jp@gc – Stepping Thread Group** --> нажать правой кнопкой мыши --> **Add --> Config Element --> HTTP Request Defaults**.
В строке **Name** дать название (например, First). В строке **http [http]** указать протокол. В строке **Server name or IP** указать ссылку или IP-адрес, куда нужно обращаться. В строке **Port Number** указать порт.
Либо можно скопировать и вставить всю строку с предыдущей работы.

3) **Создать BeanShell PreProcessor – прописать токен.**

В поле **BeanShell PreProcessor** написать код, который пропишет токен:
```
String auth_token = props.get("token"); 
vars.put("token", auth_token);
```
4) **Создать запросы**.

**jp@gc – Stepping Thread Group** --> нажать правой кнопкой мыши --> **Add --> Sampler --> HTTP Request**.
В строке **Name** дать название (например, new_data). Из предложенных методов выбрать метод POST. В строке **Path** указать эндпоинт. 
Далее на странице этого запроса нужно прописать переменные. 
На странице запроса **new_data --> Add** (внизу) --> (например, создать 4 переменные) -->
Name: auth_token, Value: ${token} (вызов окружения из токена)
Name: name, Value: Alina
Name: age, Value: 25
Name: salary, Value: 500
Еще можно добавить несколько GET запросов (для проверки).

5) **Заполнить поле jp@gc – Stepping Thread Group данными для проведения ступенчатых тестов**

Выбрать радиобаттон **Continue**.
- В строке **This group will start (…) threads** – написать количество потоков (пользователей), которые группа запустит (например, 200)
- В строке **First, wait for (…) seconds** – написать количество секунд, после которых указанное количество пользователей начнет старт (сначала ждать какое-то количество секунд) (например, 5)
- **В строке Then start (…) threads** – написать сколько потоков запустятся, после того как пройдет то количество секунд, которые указаны в строке First, wait for (…) seconds (например, 10)
- В строке **Next, Add (…) threads every** – написать количество потоков (например, 20), которые запустятся через какое-то количество секунд (…) seconds (например, 10)
- В строке **Using ramp-up (…) seconds** – написать за какое время запустить несколько потоков, которые будут указаны в строке Next, Add (например, 1)
- В строке **Then hold load for (…) seconds** – написать сколько времени сервер будет держать нагрузку (сколько по времени будет длится график на прямой линии) (например, 600)
- В строке **Finally, stop (…) then every (…) seconds**. Ввести значение по сколько потоков и через какое временя будет снижать нагрузка, после того как пройдет время, которое было указано в строке **Then hold load for (…) seconds** (например, 5 и 1).
После создания всех инструментов и запросов для проверки тестов нажать кнопку **Start**.

Сохранять файлы нужно в формате **smx**.
