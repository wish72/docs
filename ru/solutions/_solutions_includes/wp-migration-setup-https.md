## Настройте доступ по защищённому протоколу HTTPS {#setup-https}

Этот шаг необязателен, вы можете его пропустить или вернуться к нему позже.

Современные веб-сайты обычно используют протокол HTTPS, который надёжно защищает данные (например, пароли) от кражи в процессе их передачи между сайтом и браузером пользователя. Защищённый сайт также получает более высокий рейтинг в поиске.

Для настройки HTTPS необходимо иметь так называемый SSL-сертификат и ключ сертификата. Обычно ключ сертификата тем или иным способом создаёт администратор веб-сайта, а SSL-сертификат, соответствующий этому ключу, получают от какого-либо из широко известных центров сертификации.

Общедоступный сервис [Let’s Encrypt](https://letsencrypt.org/) позволяет получить SSL-сертификат для любого сайта бесплатно и автоматичеcки. Мы покажем, как установить на сайт сертификат Let’s Encrypt.

{% note alert %}

Если ваш старый сайт на WordPress не был защищён HTTPS, настроить HTTPS только на сервере Apache _будет недостаточно_. Дополнительно потребуется внести изменения в настройки и, вероятно, _в содержание сайта_ WordPress, чтобы учесть изменение адреса сайта с `http://` на `https://`. Вам может понадобиться другая инструкция, в которой объясняется, как сделать все необходимые изменения на вашем сайте в этом случае.

{% endnote %}

Инструкции по установке сертификатов Let’s Encrypt различаются для разных версий и различных дистрибутивов операционной системы сервера. В настоящее время образ сервера {{ yandex-cloud }} с предустановленным стеком LAMP использует дистрибутив Linux Ubuntu версии 18 LTS, что можно видеть после подключения к серверу в его приветственном сообщении:

```
Welcome to Ubuntu 18.04.5 LTS (GNU/Linux 4.15.0-55-generic x86_64)
```
Для настройки защиты HTTPS с сертификатом Let’s Encrypt в Linux Ubuntu 18:

1. Установите программу `certbot`:

   ```bash
   sudo apt install snapd
   sudo snap install core; sudo snap refresh core
   sudo snap install --classic certbot
   sudo ln -s /snap/bin/certbot /usr/bin/certbot
   ```

   - В ответ на сообщение об установке новых пакетов ПО `NEW packages will be installed` с вопросом о продолжении установки `Do you want to continue? [Y/n]` нажмите `Enter`.

2. Запустите `certbot` для получения SSL-сертификата и настройки защиты HTTPS в веб-сервере Apache:

   ```bash
   sudo certbot --apache
   ```

   - В ответ на просьбу `Enter email address` введите ваш email, для получения оперативных уведомлений от сервиса.
   - Подтвердите согласие с условиями сервиса (`Please read the Terms of Service`), нажав `Y`.
   - Нажмите `Y` или `N` в ответ на вопрос о согласии получать электронную рассылку от организации Electronic Frontier Foundation.

   - В ответ на вопрос о том, для каких доменных имён вы хотите получить сертификат, нажмите `Enter`, если хотите получить сертификат для всех перечисленных имён:
   ```
   Which names would you like to activate HTTPS for?
   1: u1389147.trial.reg.site
   2: www.u1389147.trial.reg.site
   ```

   В случае успешной настройки вы увидите примерно следующий результат:
   ```
   Requesting a certificate for u1389147.trial.reg.site and www.u1389147.trial.reg.site
   Performing the following challenges:
   http-01 challenge for u1389147.trial.reg.site
   http-01 challenge for www.u1389147.trial.reg.site
   Waiting for verification...
   Cleaning up challenges
   Created an SSL vhost at /etc/apache2/sites-available/000-default-le-ssl.conf
   Enabled Apache socache_shmcb module
   Enabled Apache ssl module
   Deploying Certificate to VirtualHost /etc/apache2/sites-available/000-default-le-ssl.conf
   Enabling available site: /etc/apache2/sites-available/000-default-le-ssl.conf
   Deploying Certificate to VirtualHost /etc/apache2/sites-available/000-default-le-ssl.conf
   Added an HTTP->HTTPS rewrite in addition to other RewriteRules; you may wish to check for overall consistency.
   Redirecting vhost in /etc/apache2/sites-enabled/000-default.conf to ssl vhost in /etc/apache2/sites-available/000-default-le-ssl.conf

   Congratulations! You have successfully enabled https://u1389147.trial.reg.site and https://www.u1389147.trial.reg.site
   ```

Программа `certbot` автоматически создаёт ключ сертификата, получает SSL-сертификат, создаёт и настраивает в веб-сервере новый виртуальный сайт, аналогичный указанному вами, но защищённый протоколом HTTPS, и настраивает перенаправление обращений от незащищённого к защищенному сайту.

В случае успеха, открывая ваш сайт, вы будете видеть возле адресной строки в браузере значок, обозначающий, что с сайтом установлено защищённое соединение.

{% note info %}

Автоматическая настройка протокола HTTPS с сертификатом Let's Encrypt требует, чтобы ваш сайт _отвечал на запросы из интернета_, по всем доменным именам, которые вы выбрали для защиты протоколом HTTPS. Поэтому вы сможете сделать эту настройку _только после того_, как внесёте изменения в DNS, и они вступят в силу.

{% endnote %}
