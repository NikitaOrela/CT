Лабораторная работа 1


1.	Установим  Nginx(веб-сервер, который будет обрабатывать HTTP/HTTPS запросы), OpenSSL(инструмент для создания SSL-сертификатов, необходимых для HTTPS-шифрования) и создадим структуру директорий(/var/www/project_1, /var/www/project_1):

<img width="405" height="38" alt="image" src="https://github.com/user-attachments/assets/aee47706-960d-4b93-9696-53d656b2e430" />

 <img width="452" height="21" alt="image" src="https://github.com/user-attachments/assets/dd8b7c8e-4532-4c27-813d-1c900b676f07" />

 
2.	Создаем SSL-сертификаты:

<img width="452" height="17" alt="image" src="https://github.com/user-attachments/assets/4e6cbdae-1319-4f80-82cd-5634a28373af" />
<img width="452" height="113" alt="image" src="https://github.com/user-attachments/assets/63c6f3f4-313c-45f2-99b0-61c9cf785bbc" />
<img width="452" height="112" alt="image" src="https://github.com/user-attachments/assets/30e586ae-c981-473d-906f-2bc7a6d6fdbf" />
  
 
.key - приватный ключ (хранится секретно)
.crt - публичный сертификат (передается клиентам)


3.	Настраиваем виртуальные хосты:

<img width="380" height="301" alt="image" src="https://github.com/user-attachments/assets/10c44afc-4aec-4eda-8782-5ad48bd87df3" />
<img width="379" height="308" alt="image" src="https://github.com/user-attachments/assets/ec6fc9c6-f7b6-437f-aa3f-32dddebcb468" />

  
listen 80 - слушает HTTP-запросы на порту 80
return 301 - постоянное перенаправление на HTTPS
listen 443 ssl - слушает HTTPS-запросы с SSL
ssl_certificate - указывает на созданные сертификаты
Alias - перенаправляет запросы с одного пути на другой
Проксирование добавил с помощью ии и не до конца разобрался как использовать и убрал из финальной версии 

4.	Создаем тестовые проекты:
1)
  
<img width="452" height="157" alt="image" src="https://github.com/user-attachments/assets/0c5fac39-6257-4e0a-bdef-97441a3e18b0" />
<img width="452" height="27" alt="image" src="https://github.com/user-attachments/assets/139a3970-8d00-4564-b31e-50447c6e6313" />
<img width="452" height="16" alt="image" src="https://github.com/user-attachments/assets/69e51533-7451-4f9a-8815-23f609575bdb" />

2)
<img width="452" height="149" alt="image" src="https://github.com/user-attachments/assets/325ffe82-03fb-4e3a-87b4-28de4da630d4" />
<img width="452" height="16" alt="image" src="https://github.com/user-attachments/assets/1913994e-b6d2-48b4-a683-8b8aa0b1d54f" />
<img width="452" height="16" alt="image" src="https://github.com/user-attachments/assets/439bcd9e-1cbd-423e-bbe6-eefa19055ba7" />
  
Сначала хотел разбить два проекта, чтобы в первом был статический текстовый файл, а во второй json, но не получилось, поэтому решил сделать два текствовых.

5.	Добавляем локальные домены:

<img width="452" height="44" alt="image" src="https://github.com/user-attachments/assets/f594c0b5-d95d-457a-8ac4-5553e3a1a8ee" />

 
6.	Запускаем и проверяем:
<img width="452" height="23" alt="image" src="https://github.com/user-attachments/assets/fdfa1f82-9cb7-495d-85dc-f1446d3b0c22" />

 
Мы столкнулись с ошибкой, что nginx не может найти SSL-сертификаты. Это потому, что я создал сертификаты в /usr/local/etc/nginx/ssl/, а nginx ищет их в /opt/homebrew/etc/nginx/ssl/.
Переместим сертификаты из /usr/local/etc/nginx/ssl/ в /opt/homebrew/etc/nginx/ssl/:

<img width="452" height="34" alt="image" src="https://github.com/user-attachments/assets/788598bf-acb6-4ada-92f6-7398d229db76" />
 
<img width="452" height="103" alt="image" src="https://github.com/user-attachments/assets/914a414e-2492-48d1-8477-3d29c8971619" />
<img width="452" height="33" alt="image" src="https://github.com/user-attachments/assets/98393715-b543-4d54-9396-d95d2d92861b" />

  
Успешно проверяем конфигурацию

<img width="452" height="164" alt="image" src="https://github.com/user-attachments/assets/60c468ef-4fc8-4280-b0b7-a112ac0a7018" />

 
Видим что происходит редирект на безопасное соединение, но при этом:
<img width="452" height="287" alt="image" src="https://github.com/user-attachments/assets/2434fe88-a4cd-45ae-b73d-be3e1d7f41e3" />
 

Тк сертификат подписан не доверенным центром сертификации, то браузер выдает предупреждение это норма для самоподписанного сертификата

7.	Проверка работы проектов:  
<img width="306" height="159" alt="image" src="https://github.com/user-attachments/assets/913a138f-89f1-4c79-9739-52ff5ccfb9b0" />
<img width="305" height="231" alt="image" src="https://github.com/user-attachments/assets/d08af3f5-8287-4794-bc7f-4a76a68c7d1b" />
<img width="324" height="245" alt="image" src="https://github.com/user-attachments/assets/07c4c863-0dbd-4c1c-879a-0c81f0b4faaf" />
<img width="329" height="248" alt="image" src="https://github.com/user-attachments/assets/277bb1bc-6fd1-4d4f-8639-189ef0fb2539" />

  
Вывод:
HTTPS с сертификатами - работает на порту 443
HTTP → HTTPS редирект - порт 80 перенаправляет на 443
Alias для путей - /static/ корректно работает
Виртуальные хосты - project-1.local и project-2.local работают независимо
Корректное отображение контента

 
ЛР 1* 
Со звездочкой

Проверим на уязвимости сайт https://gymnasium642.spb.ru/

1. Path Traversal (Обход пути):

Описание уязвимости: Попытка доступа к файлам за пределами корневой директории веб-сервера
Методы проверки:
curl "https://gymnasium642.spb.ru/../../../etc/passwd"
curl "https://gymnasium642.spb.ru/..%2f..%2f..%2fetc%2fpasswd"

<img width="452" height="255" alt="image" src="https://github.com/user-attachments/assets/245ce8bd-662b-4760-b267-0ecee50d4c2d" />

 
<title>Страница не найдена</title>

<img width="452" height="93" alt="image" src="https://github.com/user-attachments/assets/f6802759-d6e9-46ed-9c9e-7c58f7ae2bdb" />
 

Результаты:
Все запросы возвращают ошибки 404 или 400
Сервер корректно обрабатывает попытки обхода пути

Вывод:  Уязвимость не обнаружена

2. Перебор скрытых ресурсов с помощью FFUF:

Описание уязвимости: Поиск скрытых директорий и файлов

Метод проверки:
ffuf -u "https://gymnasium642.spb.ru/FUZZ" -w ~/Desktop/CT/baza.txt -fc 403,404
Взял базу для проверки из интернета

<img width="452" height="300" alt="image" src="https://github.com/user-attachments/assets/649d6b85-1b2a-4ff2-b63a-514d2120d1f2" />
 

Найденные ресурсы:
users/login (Status: 200) - страница входа
user (Status: 200) - пользовательская страница
?wsdl (Status: 200) - WSDL веб-сервиса
Числовые страницы: 9, 11, 2 (Status: 200)
Несколько компонентов с перенаправлениями (301)

Дополнительная проверка:

curl "https://gymnasium642.spb.ru/users/login"
curl "https://gymnasium642.spb.ru/?wsdl"

<img width="452" height="278" alt="image" src="https://github.com/user-attachments/assets/a59a4462-348f-4e89-b4fa-571d20810853" />
 



Результаты:

Страница /users/login доступна, но требует аутентификации 

<img width="372" height="283" alt="image" src="https://github.com/user-attachments/assets/afe413f9-8ab5-4de1-a718-465f86f114bd" />

WSDL endpoint недоступен (таймаут)
Числовые страницы возвращают контент, но не раскрывают критической информации

<img width="284" height="215" alt="image" src="https://github.com/user-attachments/assets/35827ae2-8a22-4f87-b9c6-7354c1338625" />
 

Вывод:  Частично успешно - найдены скрытые ресурсы, но без критичного доступа


3. Анализируем файл robots.txt:
   
<img width="242" height="315" alt="image" src="https://github.com/user-attachments/assets/f4bdbef6-6702-48f7-bcee-356e4af775a8" />
 

Найденные интересные пути:
/my/ - вероятно, папка с личными кабинетами или админкой
/thumb/ - система генерации превью изображений
/d/ - директория с файлами


Проверим указанные пути из файла:

curl "https://gymnasium642.spb.ru/my/"
curl "https://gymnasium642.spb.ru/thumb/"
curl "https://gymnasium642.spb.ru/d/"

<img width="452" height="110" alt="image" src="https://github.com/user-attachments/assets/a0761809-e890-456b-ac98-d50ef9d53c41" />

 
Результаты:

Все пути возвращают 404 Not Found
Система не раскрывает содержимое защищенных директорий

Вывод:  Уязвимость не обнаружена

4. Проверка конфигурационных файлов:


<img width="452" height="44" alt="image" src="https://github.com/user-attachments/assets/91f8b557-fd21-411b-a908-e801516e8dc2" />
 

Результаты:

Все пути возвращают 404 Not Found

Вывод:  Уязвимость не обнаружена


Общий вывод:

Высокая безопасность
Корректная настройка NGINX
Защита от Path Traversal атак
Соблюдение правил robots.txt
Отсутствие открытых конфигурационных файлов

<img width="454" height="34" alt="image" src="https://github.com/user-attachments/assets/11bd0516-2dec-45d5-a395-c056004cc5b6" />
