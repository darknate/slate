---
title: TIM Connect SDK

language_tabs:
  - code


toc_footers:
  - <a href='#'>Запросить API ключ</a>
  - <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
  - errors
  - changelog

search: true
---



<h1 id="1">Введение</h1>

Комплект средств разработки <font color="#158EDC">TIM Connect</font> - это способ взаимодействия с сервисом на основе следующих возможностей:

```json
{
  "resource": "TIM Connect",
  "version": "1.7.1",
  "contact": "support@tim-connect.com"
}
```

- регистрация и авторизация устройства в сервисе;
- получение и обработка PUSH-уведомлений, проходящих через СМСЦ сервиса;
- доступ к архиву сообщений пользователя, проходящих через СМСЦ сервиса;
- визуализация информации, предоставленной в сообщениях;
- доступ к статистическим данным, формируемым пользователями сервиса;
- формирование данных о геолокации и персональных маркеров сообщений (хэштеги).

<img align=center src=/images/splash.png>


<h1 id="2">Краткая сводка</h1>

В рамках использования SDK можно выделить общую структуру данных, которая будет обрабатываться одинаково на всех мобильных платформах устройств. Сервис предоставляет информацию, которая не является закрытой информацией банка с точки зрения дублирования SMS. Однако, предполагается, что данные из PUSH-уведомлений будут хранится во внутренней памяти телефона в зашифрованном виде на уровне приложения, а код обфусцирован (в случае с ОС Android).

<h2 id="2.1">Минимальные требования</h2>
<aside class="warning">
SDK TIM Connect требует выполнение следующих шагов интеграции:
</aside>
1. Прием PUSH-уведомлений с сервера и отображение в центре уведомлений ОС;
2. Сохранение и вывод сообщений в ленте уведомлений внутри приложения;
3. Двусторонний звонок в приложение.
<p>Корректная работа сервисов возможно только при выполнении всех трех шагов интеграции.</p>


<h2 id="2.2">Рекомендации по интеграции</h2>
Выделяются две ключевых схемы интеграции SDK - за зоной авторизации приложения и после нее. Так как SDK дает возможность визуализировать текст, получаемый ранее протоколом SMS, то в принудительном отображении всего контента только после авторизации в мобильном банке нет смысла. В этом случае пользователю дополнительно необходимо будет каждый раз проходить авторизацию в мобильном банке, чтобы посмотреть ленту уведомлений. Создайте отдельный раздел на главном экране приложения, после перехода в который отобразиться контент уведомлений банка. Для предохранения от доступа к устройству третьих лиц используется локальный пароль и/или функции проверки отпечатков пальца. 
<p>Регистрация в сервисе <font color="#158EDC">TIM Connect</font> выполняется двумя путями на выбор: разовой подпиской пользователя на получение PUSH-уведомлений с верификацией номера телефона одноразовым SMS с кодом подтверждения, либо подписью запроса на регистрацию сервером банка перед передачей в <font color="#158EDC">TIM Connect</font>. Примеры обеих способов:</p>


1. В мобильном банке присутствует раздел «Настройки». На этом экране добавляется переключатель с подписью «Получать PUSH-уведомления вместо SMS».

<p><img src=/images/switch.png></p>

При активации пользователю предлагается ввести номер телефона, на который отправляется одноразовый код. В дальнейшем код используется как пароль к сервису TIM и является методом верификации владельца номера телефона. После успешного ввода кода клиент сможет выполнить любой метод из SDK. Если номер телефона заранее известен мобильному приложению, или можно получить его с помощью API банка, то на усмотрение разработчика он передается в метод регистрации SDK без участия пользователя.

2. Используя второй способ банк заранее верифицирует номер телефона пользователя в приложении мобильного банка (например, при каждом входе в мобильный банк приходит код подтверждения в SMS). таком случае дополнительная верификация номера телефона с еще одним кодом в SMS выглядит непонятно для пользователя. Проще говоря, выбирая второй способ, банк гарантирует, что указанный номер телефона принадлежит владельцу регистрируемого устройства. Для такого типа взаимодействия банк регистрирует пользователя в <font color="#158EDC">TIM Connect</font> без непосредственного участия самого пользователя, выполнив необходимые действия на стороне своего сервера. При включении переключателя, как в первом примере, приложение отправляет полученный от устройства PUSH-токен на сервер банка (а так же остальную необходимую информацию). На этом этапе у банка появляется возможность сохранять PUSH-токены на своей стороне, если это требуется. Сервер банка подписывает запрос шифрованием SHA256 (алгоритм направляется по запросу) и передает получившийся "digest" обратно в приложение: (номер телефона + токен + digest). Приложение передает эти данные в SDK, после чего они отправляются в TIM Connect, где проверяется подпись, и в случае успеха создается новая учетная запись. Этот способ подразумевает минимальные доработки со стороны банка, перекладывая взаимодействие внутрь SDK.
<p>
Примеры визуального отображения сервисов доступы в отдельном разделе «Рекомендации интеграции SDK TIM Connect в мобильные банки».</p>

<p>Схема взаимодействия без участия пользователя</p>
<p><img src=/images/shascheme.png></p>

<h2 id="2.3">Справочник категорий СМС</h2>
Данный справочник используется во всех сообщениях сервиса.

ID | Наименование
-------------- | --------------
1 | Другое
2 | Автомобиль
3 | Бизнес
4 | Все для дома
5 | Домашние животные
6 | Здоровье и красота
7 | Кафе и рестораны
8 | Коммунальные платежи
9 | Кредиты
10 | Налоги и штрафы
11 | Транспорт
12 | Одежда
13 | Путешествия
14 | Переводы
15 | Развелечения
16 | Связь, интернет и TV
17 | Снятие наличных
18 | Супермаркет


<p>*По запросу <font color="#158EDC">TIM Connect</font> может предоставить набор иконок для списка категорий, используемых в собственных приложениях.</p>

<p>Логотипы мест покупок имеют прозрачную основу и подготовлены для отображения на белом фоне экрана. Размер логотипов не превышает 300х300 пикселей, однако возможны редкие исключения для непопулярных мест. Размер холста не всегда имеет одинаковое соотношение сторон, т.е. логотипы не всегда будут квадратные, учитывая прозрачный фон.</p>



<h1 id="3">Android</h1>
<h2 id="3.1">Подготовка проекта</h2>

Первым шагом в Manifest.xml добавляются разрешения.

>Разрешения в Manifest.xml:

```xml
<uses-permission android:name="android.permission.INTERNET"/>
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE">
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE"/>
<uses-permission android:name="android.permission.GET_ACCOUNTS"/>
<uses-permission android:name="android.permission.WAKE_LOCK"/>
<uses-permission android:name="android.permission.VIBRATE"/>
<uses-permission android:name="com.timconnect.sdk.permission.C2D_MESSAGE"/>
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"/>
<permission android:name="com.timconnect.sdk.permission.C2D_MESSAGE"
android:protectionLevel="signature"/>
```


>Код в тег Application в файле Manifest.xml:

```xml

<service
android:name=" название.вашего.пакета.utils.push.GCMIntentService"/>
<receiver android:name="com.google.android.gcm.GCMBroadcastReceiver"
android:permission="com.google.android.c2dm.permission.SEND">
   <intent-filter> 
      <action android:name="com.google.android.c2dm.intent.RECEIVE"/>
       <action android:name="com.google.android.c2dm.intent.REGISTRATION"/>
       <category android:name=" название.вашего.пакета "/>
   </intent-filter>
</receiver>
```
>"название.вашего.пакета"" необходимо заменить на соответствующий packageId Вашего приложения.

<br><br><br>
<p>Затем добавляем в тег Application в файле Manifest.xml добавляем код для работы SDK.</p>

<br><br><br>
Копируем содержимое папки lib  в одноименную папку, находящуюся в директории проекта.
Копируем папку utils в папку «название/вашего/пакета».

>Код в блок dependencies:

```Gradle
compile fileTree(dir: 'libs', include: ['*.jar'])
compile(name:'timConnectSdk', ext:'aar')
{
    transitive = true
}
```

>Код в блок android:

```Gradle
packagingOptions {
exclude ‘META-INF/LICENCE’
exclude ‘META-INF/NOTICE}
sourceSets {
    main 
        jniLibs.srcDirs = ['libs']
    }
}
```

<br><br><br>
В файле build.gradle в блоки dependencies и andorid соответствующий код.
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
Общие моменты по работе с SDK:
Кеширование получаемых данных с серверов <font color="#158EDC">Tim Connect</font> не производится.
Проверить результат запроса к серверу можно вызвав метод success() класса BaseServerResponse.
По умолчанию SDK не выводит нативных PUSH уведомлений, только принимает.
SDK предоставляет ответ от сервера на любой из доступных запросов в формате String.
Для идентификации ошибок на этапе внедрение можно использовать класс Logger. Метод enableLog() включает логирование. Например, этот метод срабатывает в случае успешной и не успешной регистрации устройства в GCM или в случае получения устройством PUSH уведомления.

<br><br>

<p>Ключ API при регистрации в сервисе выдается по запросу.</p>


<h2 id="3.2">Создание ключей в Google Developer Console</h2>

```shell
* 95.213.198.4 
* 95.213.198.5 
* 95.213.130.70 
* 93.190.105.145 
* 95.213.130.66 
* 95.213.130.69 
* 46.182.28.194 
* 46.182.28.195
```

<p>Для создания серверного ключа необходимо перейти по ссылке в <a href=https://console.cloud.google.com/>Google API Console</a> и выбрать проект (или создать новый).</p>


В левом верхнем углу нажать на кнопку меню и выбрать «Диспетчер API», перейти на вкладку «Учетные данные». Нажать «Создать учетные данные», выбрать «Ключ API», выбрать «Ключ Сервера», задать название ключа и IP адреса серверов <font color="#158EDC">TIM Connect</font>.



Получившийся ключ API необходимо передать в <font color="#158EDC">TIM Connect.</font>



<h2 id="3.3">Авторизация на сервере TIM</h2>

>Пример установки ключей и проверки регистрации в GCM:

```java
mTimConnectSdk = TimConnectSdk.getInstance(getApplicationContext());
mTimConnectSdk.setAppkey(APP_KEY);
mTimConnectSdk.setPushSenderId(PUSH_SENDER_ID);

final PushController controller = new PushController(getApplicationContext());
controller.setPushDataLoadedListener(new SimpleJacksonRequestListener<TemplateServerResponse<Message>>()
{
  @Override
  public void onResponse(TemplateServerResponse<Message> response, int statusCode, VolleyError error)
  {
    // when Push notification arrives, sdk will automatically call loadMessageByID method and this listener will be called
  }
});

controller.setDeliveryReportListener(new SimpleJacksonRequestListener<BaseServerResponse>() {
  @Override
  public void onResponse(BaseServerResponse response, int statusCode, VolleyError error) {
  // when Push notification arrives, sdk will automatically call loadMessageByID method. After that, SDK will send delivery report to Tim Connect server
  // and this listener will be called
  }
});

controller.setRegistrationListener(new PushController.GCMRegistrationListener() {
  @Override
  public void onRegistered(String s) {
  }
});

```


Перед авторизацией необходимо удостовериться в том, что устройство зарегистрировано в <font color="#14CB79">GCM</font> и приложение успешно получило PUSH-токен. Следующим шагом необходимо указать <font color="#158EDC">PUSH_SENDER_ID</font> (номер проекта в <font color="#14CB79">Google Console</font>) и <font color="#158EDC">APP_KEY</font>. 

>Метод отправки номера телефона:

```java
mTimConnectSdk.setPhone(PHONE);
mTimConnectSdk.authByPhone(new SimpleJacksonRequestListener<BaseServerResponse>() {
@Overridepublic void onResponse(BaseServerResponse response, int statusCode, VolleyError error) {
}});}
```
>Метод подтверждения номера с помощью SMS:

```java
mTimConnectSdk.setActivationCode(code);
mTimConnectSdk.confirmPhone(new SimpleJacksonRequestListener<SessionResponse>() {
@Overridepublic void onResponse(SessionResponse response, int statusCode, VolleyError error) {}});
```

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

Для регистрация доступны два запроса: с кодом подтверждения по SMS и без. 

Регистрация с кодом подтверждения по SMS проходит в 2 этапа:
* сперва необходимо отправить номер телефона:
* затем подтвердить номер телефона с помощью кода, полученного в SMS:

<br><br>

Подобная авторизация более не требуется - механизм сохранения необходимых файлов cookie реализован внутри SDK.

<br><br><br><br><br><br><br><br><br><br><br><br><br><br>


> Инициализация метода authWithoutSmsL:

```java
inal AuthWithoutSms data = new AuthWithoutSms();
data.phone_number = "phone_number_here";
data.digest = "sha256_digest";

data.device = new Device();
data.device.app_key = "application_key";
data.device.token = "GCM_token";
```

>Вызов метода для регистрации без SMS:

```java
mTimConnectSdk.authWithoutSms(data, new SimpleJacksonRequestListener<BaseServerResponse>() {
           @Override
           public void onResponse(BaseServerResponse response, int statusCode, VolleyError error) {
               Logger.e("auth");
           }
       });
```


Регистрация без кода подтверждения в SMS выполняется одним методом. Сначала необходимо инициализировать объект <code>AuthWithoutSms</code>, а затем вызвать одноименный метод SDK, передав объект в качестве параметра.



<aside class="notice">
Обратите внимание, что генерация digest должна проходить вне мобильного приложения, иначе алгоритм и секретный код может быть скомпрометирован.
</aside>


<h2 id=3.4>Вывод уведомлений</h2>

>Пример вывода нативного уведомления:

```java

public static final void displayNotification(final Message message)
  {
      final Intent intent = getNotificationStartIntent(context, message);
      final PendingIntent pIntent = PendingIntent.getActivity(context, 0, intent, PendingIntent.FLAG_ONE_SHOT);
        final Notification.Builder builder = new Notification.Builder(context)
            .setContentText(message.message)
            .setTicker(message.message)
            .setContentIntent(pIntent)
            .setDefaults(Notification.DEFAULT_ALL);


          builder.setSmallIcon(R.drawable.ic_notif_logo);
          if (Build.VERSION.SDK_INT > Build.VERSION_CODES.ICE_CREAM_SANDWICH_MR1)
          {
            builder.setPriority(Notification.PRIORITY_MAX);
            noti = builder.build();
          } else {
            noti = builder.getNotification();
          }
  }

```

Уведомления выводятся в качестве <font color="#158EDC">Notification</font>.

Чтобы организовать вывод уведомлений на экран, необходимо использовать класс <font color="#158EDC">PushController</font>. Наследовав класс и переопределив метод <font color="#158EDC">displayNotification()</font>, можно вывести нативное уведомление как показано в примере.

>Класс CallsController:

```java
mController = mTimConnectSdk.getCallsController();
mController.setUserPhone("79636125131");
mController.setDestinationPhone("74953693038");
mController.setCallStateListener(new CallsController.CallStateListener()
```
<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

Для работы со звонками необходимо использовать класс <font color="#158EDC">CallsController</font>.
Перед началом работы необходимо передать номер телефона текущего пользователя, номер телефона, на который необходимо звонить, а так же можно задать слушателя, с помощью которого можно отслеживать состояние звонка.





<h2 id="3.5">Описание методов SDK</h2>

Класс <font color="#158EDC">TimConnectSdk</font> предоставляет методы:


<font color="#158EDC">requestMessagesQueue()</font>
Получение сообщений, которые находятся в очереди (новые сообщения, для которых не был отправлен deliveryReport);

> Пример requestMessagesByDates:

```java
final ByDatesParams params = new ByDatesParams();
params.start_date = "2015-01-01";
params.end_date = "2030-01-01";
params.delivered = "true";
```

<font color="#158EDC">requestMessagesByDates()</font>
Получение истории сообщений. С помощью объекта ByDatesParams можно указать границы временного интервала. 

<font color="#158EDC">loadMessageById()</font>
Получение сообщения по ID. В «колбэке» возвращает объект типа Message.

<font color="#158EDC">tryAuthUser()</font>
Запрос обновит сессию для дальнейшей работы API TIM Connect.

<font color="#158EDC">setHashtag()</font>
Устанавливает hastag для сообщения по его ID. Первым параметром необходимо передать ID сообщения, вторым в формате String - сам хэштег. Для удаления хэштега необходимо передать пустую строку “”.

>Пример unregisterDevice:

```java
mApi.unregisterDevice(new SimpleJacksonRequestListener<BaseServerResponse>()
      {
        @Override
        public void onResponse(BaseServerResponse response, int statusCode, VolleyError error)
        {
            if (response != null && response.success())
                Logger.e("device unregistered");
        }
      });
 ```

<font color="#158EDC">setGeolocation()</font>
Установить геолокацию для сообщения. Первым параметром необходимо передать ID сообщения, вторым - latitude, третьим - longitude.

<font color="#158EDC">changeCategory()</font>
Изменяет категорию сообщения. Первым параметром необходимо передать ID сообщения, вторым - id категории в формате int.

<font color="#158EDC">sendDeliveryReport()</font>
Помечает сообщение как доставленное. Есть возможность либо 1 сообщение (в виде id), либо список сообщений с типом Message.

<font color="#158EDC">unregisterDevice()</font>
Удаляет устройство. Сервис не будет отправлять уведомления на девайс и не позволит выполнить какие-либо запросы в API.

>Пример checkAuth:

```java
mApi.checkAuth(new SimpleJacksonRequestListener<Boolean>()
        {
            @Override
            public void onResponse(Boolean status, int i, VolleyError volleyError)
            {
                if (status != null)
                    Logger.e("authorized? " + status);
            }
        });
```

<aside class="notice">После того как вызовете этот метод, необходимо вручную у объекта Auth сбросить авторизацию – для этого есть метод clearAuth(). Метод сотрет авторизацию из локального хранилища.</aside>

<font color="#158EDC">checkAuth()</font>
Проверяет, авторизовано ли данное устройство в сервисе. 





<h2 id="3.6">FAQ</h2>


Если не срабатывает

<code>controller.setRegistrationListener(new PushController.GCMRegistrationListener()</code>

* проверить, правильно ли указаны все ключи;
* проверить, правильно ли указаны все данные в манифесте;
* включить логи, вызвав Logger.enableLog(), и посмотреть лог в консоли.

<br>

<p>Для регистрации в GCM есть слушатель на случай ошибки?</p>
Если использовать <code>controller.setRegistrationListener(new PushController.GCMRegistrationListener()</code>, то там есть такой слушатель.



<h1 id="4">iOS</h1>

Минимальная версия iOS 8.0. Все данные в callback-блоках функций SDK (сообщения и пр.) представлены в JSON формате. Все запросы вызываются методами объекта - <font color="#158EDC">singletone:TimAPISDK</font>.

Ключ API при регистрации в сервисе выдается по запросу.

<h2 id="4.1">Требования</h2>
Для использования PUSH-уведомлений необходимо создать PUSH сертификат в Developer аккаунте Apple для используемого приложения. Данный сертификат необходимо передать в TIM Connect для подписи отправляемых уведомлений и сообщить bundleid приложения (например, com.timconnect.Tim). В случае отсутствия данного сертификата получение PUSH-уведомлений с SDK невозможна.

<h2 id="4.2">Создание сертификата</h2>
Сертификат создается в <a href=https://developer.apple.com/> Apple Developer Console </a>. В разделе <font color="#14CB79">Certificates, Identifiers & Profiles</font> нужно создать <font color="#14CB79">Production</font> сертификат <font color="#14CB79">Apple Push Notification service SSL (Sandbox & Production)</font> и выбрать APP ID приложения. Полученный сертификат скачать и открыть в Keychain. Во вкладке My Certificates нужно развернуть сертификат, выделить всё содержимое и с помощью контекстного меню экспортировать в .p12 файл. Затем конвертировать в .pem командой:

<code>openssl pkcs12 -in имя_сертификата.p12 -out имя_сертификата.pem -nodes -clcerts</code>

<img width=320 src=/images/iossert.png>

Можно воспользоваться утилитой <a href=https://github.com/KnuffApp/Knuff>Knuff</a>, которая выполнит все эти действия автоматически.

<h2 id="4.3">Запрос регистрации</h2>

Для доступа к контенту клиенту необходимо пройти процедуру регистрации. Для регистрация доступны два запроса: с кодом подтверждения по SMS и без. 

<img src=/images/reg.png>

>Пример ответа (responseJson):

```json
{
"result": "success", 
"message": "Sent SMS activation code to 89991234567"
}
```

Регистрация с кодом подтверждения по SMS происходит в 2 этапа: сначала делается запрос на регистрацию указанного номера телефона, затем клиент подтверждает аутентификацию одноразовым паролем, высланным на его номер телефона. В случае успешного запроса регистрации клиент получит SMS с кодом подтверждения (4 цифры):


<code>registationWithPhone:(NSString *)phoneNumber
           <br> block:(void (^)(NSDictionary *responseJson,NSError *error))block;
</code>


В ответ сервер создаст пароль, который будет сохранен в связке ключей для последующей авторизации. Авторизация вызывается автоматически, используя сохраненные данные:<br>


<code>
confirmWithPhone:(NSString *)phoneNumber<br>
code:(NSString *)smsCode<br>
block:(void (^)(BOOL success, NSError *error))block;
</code>


<font color="#158EDC">phoneNumer</font> - телефон переданный в первом запросе и <font color="#158EDC">smsCode</font> полученный с ввода юзером на экране авторизации. В блоке есть переменная <font color="#158EDC">success</font>, которая сообщает о статусе авторизации после ответа от сервера.


Регистрация без кода подтверждения по SMS выполняется методом:

>Пример ответа от сервера (JSON):

```json
{
  "result": "success",
  "message": "Registration succsesful",
  "sessionid": "c4vddk63g5wen7arfhjqlc7t7m7b7b7b",
  "credentials": "16HYnQYFD0dtlb7XoG4G4G4egQmc9hwFvD61MNkFl"
}
```

<code>
registerWithoutSms:(NSString*)digest<br>
phoneNumber:(NSString*)phoneNumber<br>
block:(void (^)(NSDictionary *responseJson, NSError *error))block;
</code>


<font color="#158EDC">digest</font> – подпись для верификации запроса на стороне TIM Connect;
<font color="#158EDC">phoneNumer</font> – номер телефона пользователя;

<br><br>
Алгоритм формирования подписи <font color="#158EDC">digest</font> (SHA256) предоставляется по запросу.


<h2 id="4.4">Обработка сообщений</h2>

```json
{ 
        "category": "14",
        "geotag":{
            "latitude": "55.74722",
            "longitude": "37.53691"
                  },
        "hashtag": "Try",
        "id": "94419856",
        "message": "Hello, world!",
        "receiver": "79991234567",
        "sender": "TIMCONNECT",
        "timestamp": "2016-03-03 13:31:34+03:00"
}
 ```


Модель представлена в виде JSON. Содержит:<br>


* Id – уникальный идентификатор сообщения (BIGINT)
* sender – отправитель (номер телефона или идентификатор отправителя)
* receiver – получатель (номер телефона)
* category – категория СМС по умолчанию с учетом статистики
* message – тело СМС сообщения
* timestamp – время добавления СМС сообщения в очередь
* hashtag – текст хэштега
* geotag – широта и долгота локации
* category – категория


<h2 id="4.5">Обработка очереди новых сообщений</h2>
Блок функции возвращает массив новых сообщений, не подтвержденных клиентом как полученные. 

<code>getMessagesQueueWithBlock:(void (^)(NSArray *messages, NSError *error))block;</code>

<font color="#158EDC">NSArray *messages</font> - содержит в себе сообщения в JSON формате(см. выше)


<aside class="notice">Рекомендуемый алгоритм: запрос новых сообщений -> сохранение в базе данных -> подтверждение доставки (confirmMessages[ids]).
</aside>




<h2 id="4.6">История сообщений</h2>

Блок функции возвращает массив прочитанных сообщений в интервале между заданными датами.<br>
Подтверждение доставки не требуется, так как данные сообщения уже были получены пользователем.


<aside class="notice">Рекомендуемый алгоритм: запрос сообщений -> сохранение в базе данных. Рекомендуется использовать исключительно для восстановления архива сообщений при начальной установке приложения. </aside>



<code>getMessagesFromDate:(NSDate *)fromDate toDate:(NSDate *)toDate<br>
         withBlock:(void (^)(NSArray *messages, NSError *error))block;</code>

<font color="#158EDC">NSArray *messages</font> - содержит в себе сообщения в <font color="#14CB79">JSON</font> формате (см. выше).

В случае необходимости получить список всех сообщений, можно воспользоваться методом:<br>

<code>restoreMessagesWithBlock:(void (^)(NSArray *messages, NSError *error))block;</code>

<font color="#158EDC">NSArray *messages</font> - содержит в себе сообщения в <font color="#14CB79">JSON</font> формате (см. выше).


<h2 id="4.7">Запрос конкретного сообщения</h2>
При необходимости получения информации о конкретном сообщении можно воспользоваться методом:<br>

<code>getMessageWithId:(NSNumber *)messageId<br>
block:(void (^)(NSDictionary *message, NSError *error))block;</code>

<font color="#158EDC">NSArray *messages</font> - содержит в себе сообщения в <font color="#14CB79">JSON</font> формате (см. выше).


<h2 id="4.8">Подтверждение доставки</h2>
При получении новых сообщений (из очереди новых сообщений или при помощи PUSH-уведомления) клиент должен подтвердить получение этих сообщений. После получения подтверждения, сервис помещает сообщения из очереди в архив. Отправка подтверждения о доставки – критично важный этап обработки уведомлений, так как если сервер не получил отчет о доставке в заданный сервисом период, отправляется дублирующее SMS.<br>

>Пример ответа (responseJson:

```json
{"result": "success", "message": ""}
```

Для подтверждения доставки одного сообщения используется следующий метод:

<br>
<code>confirmMessageDelivery:(NSInteger)messageID
block:(void (^)(NSDictionary *responseJson, NSError *error))block
</code>

<br><br><br>

>Пример ответа (responseJson):

```json
{"result": "success", "message": ""}
```

Для подтверждения двух и более сообщений используется метод:<br>

<code>confirmMessages:(NSArray *)messagesIDs
block:(void (^)(NSDictionary *responseJson, NSError *error))block;</code>

Тип <font color="#158EDC">messagesIDs</font> – «int array».


<h2 id="4.9">Изменение категории сообщения</h2>

>Пример ответа (responseJson)

```json
{"result": "success", "message": ""}
```

Для изменения типа категории для сообщения используется метод:<br>

<code>updateMessageCategory:(NSInteger)messageID
newCategoryID:(NSInteger)categoryID
block:(void (^)(NSDictionary *responseJson, NSError *error))block
</code>




<h2 id="5.10">Сохранение/изменение гео-локации</h2>

>Пример ответа (responseJson)

```json
{"result": "success", "message": ""}
```

Для изменения геолокации для сообщения используется метод:<br>

<code>(void)updateMessageGeotag:(float) latitude<br>
longtitude:(float) longtitude<br>
messageID:(NSInteger) messageID<br>
block:(void (^)(NSDictionary *responseJson, NSError *error))block
</code>


<h2 id="5.11">Сохранение/изменение "хэштега"</h2>

>Пример ответа (responseJson)

```json
{"result": "success", "message": ""}
```

Для изменения или установки «хэштега» для сообщения используется метод:<br>

<code>updateMessageHashtag:(NSInteger)messageID<br>
hashtag:(NSString *)hashtag<br>
block:(void (^)(NSDictionary *responseJson, NSError *error))block
</code>


<h2 id="5.12">Проверка статуса регистрации</h2>

>Пример ответа (responseJson)

```
isAuth == YES or NO 
```

Проверяет, авторизовано ли данное устройство в сервисе. <br>

<code>(void)checkAuthState:(void (^)(BOOL isAuth, NSError *error))block;</code>


<h2 id="5.13">Удаление девайса</h2>

>Пример ответа (responseJson)

```json
{"result": "success", "message": ""}
```

Удаляет устройство. Сервис не будет отправлять уведомления на девайс и не позволит выполнить какие-либо запросы в API.<br>

<code>(void)logOut:(void (^)(NSDictionary *responseJson, NSError *error))block;</code>


<h2 id="5.14">VoIP-Сервис</h2>
Для совершения исходящего или принятия входящего звонка необходимо зарегистрироваться на VOIP-сервере.<br>

Получение одноразового пароля для регистрации:<br>
<code>(void) getPasswordForCall:(void (^)(NSDictionary *responseJson, NSError *error))block;</code><br>
Регистрация:<br>
<code>(void) registerInVoIPService:(NSString*) password<br>
block:(void (^)(BOOL registered))block;
</code><br>
Данный метод возвращает состояние регистрации на VOIP-сервере. После успешной регистрации можно принять или совершить звонок. <br><br>

Для принятия звонка необходимо использовать метод делегата, который вызывается при входящем звонке:
<code>(void) incommingCallHandle;</code><br>
Отнаследовавшись от него, можно принять или отклонить звонок, используя методы:<br>
<code>(void) answerIncommingCall;<br>
(void) hangUpIncommingCall;</code><br>

Метод, который включает в себя регистрацию и совершение звонка на необходимый номер:<br>
<code>(void) startCallOnNumberWithRegister:(NSString *) number;</code><br><br><br><br>

Номер телефона передается в формате «74953693038».<br>
При успешном выполнении метода начнется звонок на PSTN-телефонию с указанным номером.


<h2 id="5.15">Звонок из Web в приложение</h2>
Служебная функция, которая позволяет осуществить звонок из веб-браузера в приложение.

>Пример содержания dict:

```json
{"SIN": "", "aps": {"alert": "0", "content-available": 1}, 
"call_info": 
{"caller": "login", "asteriskpass":"password"}
}
```

Перейдите по адресу <a href=https://smpp.tim-connect.com/callclient/index.html>https://smpp.tim-connect.com/callclient/index.html</a> и введите номер телефона клиента. Логин и пароль для звонка выдается по запросу. 

Через несколько секунд придет PUSH-уведомление о входящем звонке в метод делегата:
<code>(void) incommingPushPayload:(NSDictionary *) dict;</code>
Следующим шагом необходимо зарегистрироваться на VOIP-сервере, используя логику описанную в предыдущем разделе, и принять звонок. Так же можно сразу использовать пароль для регистрации из payload’а PUSH-уведомления.

<h1 id="6">Дополнительная информация</h1>

При перерегистрации пользователю приходит SMS-оповещение:<br><br>
<aside class="success">«Внимание! Мобильное приложение на устройстве, привязанном к номеру 79261234567, было переустановлено! Если это сделали не Вы, обратитесь в службу поддержки 8-495-369-30-38»</aside><br><br>

<p>Если система считает пользователя активным на данный момент, то при повторной его регистрации отправляется на номер телефона указанное SMS. Имя отправителя используется конкретного клиента, текст сообщения конфигурируется администратором TIM Connect.</p>

<aside class="notice">Для изменения API ключа сервиса TIM для зарегистрированного клиента (номера телефона) необходимо заново выполнить процедуру регистрации.</aside>