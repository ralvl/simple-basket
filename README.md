Simple Basket
=============
~Current Version:0.3~


## Назначение плагина
Этот плагин реализует простую корзину заказов для сайтов электронной коммерции для сайтов WordPress. Основные возможности плагина:
* Реализация кнопки [купить] в каталоге товаров, количество и типы каталогов не ограничены (см. ниже).
* Реализация корзины заказов.
* Возможность учета доставки, количество планов доставки неограниченно;
* Интеграция с кодом отслеживания Google Analytics. Поддерживается как и старая версия (ga.js), так и новая версия Universal Analytics (analytics.js).
* Полностью настраиваемые шаблоны писем пользователю при заказе и администраторам магазина.
* Возможность локализации плагина на любой язык.

## Установка и активация плагина
Для установки плагина скачайте и скопируйте все файлы в папку вашего WordPress: /wp-content/plugins/simple-basket. Перейдите в панель управления в раздел Плагины и активируйте плагин Simple Basket

## Первоначальная настройка магазина на вашем сайте
### Подготовка каталога товаров
На вашем сайте должен быть каталог товаров или любое перечисление товаров в виде карточек товаров. Карточкой товара может быть запись блога, страница WordPress или запись произвольного типа (Custom Post Type). При этом название записи или страницы является названием товара. Цена товара указывается произвольным мета-полем, например, Цена
Важно, у всех товаров мета-поле цены должно называться одинаково.
В любом месте текста карточки товара вы можете разместить шорткод [buy-now], который сформирует кнопку заказа.
Если вы используете отдельный шаблон для вывода карточки товара, удобнее вместо шоркода в тексте использовать вызов функции плагина в самом шаблоне следующим образом:
```php
<?php if (function_exists('showBuyNowButton')) showBuyNowButton() ?>
```

### Данные о доставке
Если указана доставка (см. параметры) и вы хотите настроить отдельные шаблоны для вывода данных о доставке, используйте следующие имена файлов-шаблонов в своей теме:
* archive-delivery.php - этот шаблон используется для вывода списка способов доставки, если этого файла нет, используется index.php
* single-delivery.php - этот шаблон используется для вывода отдельного способа доставки, этого файла нет, используется single.php или index.php 

### Настройка формы заказа
Для работы плагина необходимо создать страницу с формой заказа. Для создайте новую страницу WordPress с любым именем. В любом месте текста этой страницы разместите следующий шорткод:
[order-form]
Сохраните и опубликуйте страницу.

### Настройка шаблонов писем 
Плагин использует 2 шаблона писем, которые высылаются при совершении заказа пользователю и администраторам сайта. Для создания шаблонов перейдите в Записи и создайте новую запись. Заголовок записи – это тема письма, текст записи – текст письма. И в заголовке и в тексте можно использовать следующие шорткоды:
* [order-code] - код заказа
* [order-customer] - имя пользователя
* [order-email] - E-mail пользователя
* [order-phone] - телефон пользователя
* [order-comment] - комментарий к заказу
* [order-items] - таблица с элементами заказа
Сохраните запись со статусом Черновик (это важно!) и перейдите к свойствам записи:
 
Укажите ярлык записи, например, email-admin для письма администраторам и email-user для письма пользователю.
Аналогично сделайте второй шаблон. 
Важно: шаблоны писем должны иметь статус Черновик.

## Настройка плагина
Перейдите в раздел Параметры --> Корзина и укажите следующие параметры:

#### Параметры корзины
Надпись на кнопке – при необходимости измените текст в кнопке Купить
Страница с формой заказа – укажите URL созданной вами страницы формы заказа

#### Каталог продуктов
Поле цена – укажите при необходимости мета-поле в карточке товара с ценой

#### Доставка
Если вы хотите учитывать доставку, установите галочку «Мой магазин использует оплачиваемые планы доставки». После сохранения параметров необходимо перейти в раздел настроек постоянных ссылок и там нажать кнопку [Сохранить параметры]. В результате в вашем WordPress  появится новый раздел Доставка:
Добавьте необходимые варианты доставки, указывая их стоимость:
В параметрах корзины можно выбрать способ доставки по умолчанию.

#### Google Analytics
Если на вашем сайте используется Google Analytics, вы можете настроить интеграцию магазина с ним. Для этого в Google Analytics перейдите в настройку профиля и выберите там следующий пункт Сайт электронной торговли:
После этого выберите в настройках Корзины тип интеграции
 
#### Письмо-подтверждение заказа
В этой секции необходимо указать созданные вами ранее ярлыки шаблонов писем, например:
* user-email
* user-admin
 
Настройка закончена. Нажмите кнопку Сохранить.

## Проверка работы корзины
Перейдите на любую карточку товара и нажмите на кнопку Купить. Вы должны перейти в корзину заказа, нажмите кнопку Оформить заказ. Вы должны получить письмо-подтверждение. Также заказы собираются в разделе Заказы
 
## AJAX API
В корзине реализован простой API для взаимодействия с сервером из JavaScript. Корзина сама добавляет глобальный объект SimpleBasket, у которого реализованны следующие методы:
* SimpleBasket.getData(callback)
* SimpleBasket.add(id, callback)
где:
* callback - функция вызываемая по завершению работы метода, получает объект параметр data с данными - содержимое корзины
* id - код товара (id страницы с товаром)

Например:
```javascript
// Узнаем содержимое корзины
SimpleBasket.getData(function(data){
	console.log(data);
});
```
*ВНИМАНИЕ!* Все методы объекта SimpleBasket выполняются асинхронно, полученные данные можно читать только в функции callback!



## Декларация разработчика
Данный плагин разрабатывается исключительно для личных целей разработчика. У разработчика нет задач улучшения мира, снижения энтропии Вселенной и накормления голодающих детей Африки.  В связи с этим разработчик ничего лично вам не должен. Вы можете как угодно использовать данный код в любых своих проектах, в любых условиях и так, как вам заблагорассудится. Ссылка или упоминание добрый словом разработчика не обязательны и не требуются, но будут с благодарностью приняты. Но при этом вы должны четко понимать что данный код предоставлен «как есть», а не так «как я хочу». Разработчик не требует от вас никакой платы в явной или неявной форме ни за использование данного плагина, ни за его код и/или идеи в нем заложенные.  Берите и пользуйтесь. Плодитесь и размножайтесь. Аминь!
