# VkChatBot
Это расширяемая система для чат-ботов сети Вконтакте, работающая через callback API.   
Используется полнотектовый поиск от sphinx, поддерживаются расширения в виде функций и хуков.


### Установка 
Из папки install импортируем структуру mysql.  
При использовании SphinxSearch в качестве текстового поиска, ставим конфиг из папки install.  
Иначе ставим полнотекстовый индекс в таблице answers на поле output:  ```ALTER TABLE `answers` ADD FULLTEXT(`output`);```  
В config.php настраиваем параметры бд.

### Настройка
Копируем дистрибутив на веб сервер, в Управление сообществом -> Работа с API -> Callback API -> Адрес Вашего сервера 
указываем "path/to/VkChatBot/index.php".  
В settings(mysql) находятся индивидуальные настрокий ботов, которые необходимо получить через вк.
Вставляем туда новую строчку.  
**group_id** - Id группы вконтакте  
**access_token**  - Управление сообществом -> Работа с API -> Создать ключ   
**service_token**  - Токен приложения, не обязателен  
**confirmation_code** - Управление сообществом -> Работа с API -> Callback API -> Строка, которую должен вернуть сервер    
**uniqid** - Управление сообществом -> Работа с API -> Callback API ->Секретный ключ 
**functions** - Набор фунций, которые будут активны. Разделять через ;  
**hooks** - Набор хуков, которые будут выполняться. Разделять через ;  

### Добавление сообщений
В таблицу answers вставляем строки, где input - строки, по которым будет производиться поиск.
Из output случайно выбираются ответы. Разделять ответы можно через ";".  
Если не было найдено ответа, то выбирается сообщение, где input = "Неизвестные сообщения".  
Для исполльзования функции в качестве ответа, нужно в output использовать *FunctionName\*.

### Добавление функций
Новую функцию нужно положить в папку Functions, в settings(mysql) нужно добавить через ";" названия функции.  
Функция представляет собой класс, названый по имени файла. Класс должен содержать метод go(), который будет выполнен.
Доступ к другим элементам производится чрезе глобальные переменные (```global $VK, $SQL, $data;```).

### Добавление хуков
Хуку надо положить в соответствующую папку (Hooks_message_new, Hooks_group_join или  Hooks_group_leave).  
В settings(mysql) нужно добавить через ";" названия хуки. Например, будет так: "OnlySubscribers;add_subscriber;NewHook". 