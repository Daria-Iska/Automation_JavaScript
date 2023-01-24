# Automation_JavaScript

Основы автоматизации тестирования
Задание 1
Автоматизируй тест-кейс для Яндекс.Маршрутов. Найди нужные селекторы на стенде: https://qa-routes.praktikum-services.ru/

[CASE-1] Проверка названия вида транспорта в результате для такси

Шаги:
1. Ввести «Время начала поездки» — 08:00.
2. В поле «Откуда»: Усачева, 3.
3. В поле «Куда»: Комсомольский проспект, 18.
4. Выбрать режим «Свой».
5. Выбрать вид транспорта: такси.

ОР: Текст появившегося результата начинается со слова «Такси».
Решение
Селекторы:

Элемент	Селектор
Поле «Часы»	#form-input-hour
Поле «Минуты»	#form-input-minute
Поле «Откуда»	#form-input-from
Поле «Куда»	#form-input-to
Режим «Свой»	#form-mode-custom
Транспорт «Такси»	#from-type-taxi
Строка результата	#result-time-price
Автотест:

const puppeteer = require('puppeteer');

const URL_TEST = 'https://qa-routes.praktikum-services.ru/';

async function testTaxiResult() {
    console.log('Запуск браузера');
    const browser = await puppeteer.launch({headless: false, slowMo: 100});

    console.log('Создание новой вкладки в браузере');
    const page = await browser.newPage();

    console.log('Переход по ссылке');
    await page.goto(URL_TEST);

    console.log('Шаг 1: ввод часов и минут');
    const hoursInput = await page.$('#form-input-hour');
    await hoursInput.type('08');

    const minutesInput = await page.$('#form-input-minute');
    await minutesInput.type('00');

    console.log('Шаг 2: заполнение поля Откуда');
    const fromInput = await page.$('#form-input-from');
    await fromInput.type('Усачева, 3');

    console.log('Шаг 3: заполнение поля Куда');
    const toInput = await page.$('#form-input-to');
    await toInput.type('Комсомольский проспект, 18');

    console.log('Шаг 4: выбор режима Свой');
    const routeMode = await page.$('#form-mode-custom');
    await routeMode.click();

    console.log('Шаг 5: выбор вида транспорта');
    const typeOfTransport = await page.$('#from-type-taxi');
    await typeOfTransport.click();

    console.log('Ожидание элемента с результатом');
    await page.waitForSelector('#result-time-price')

    console.log('Получение строки с результатом');
    const text = await page.$eval('#result-time-price', element => element.textContent);

    console.log('Проверка условия тест-кейса');
        if (text.startsWith('Такси')) {
        console.log('Успех. Текст содержит: ' + text);
    } else {
          console.log(`Ошибка. Текст не начинается со слова 'Такси'`)
    }

    console.log('Закрытие браузера');
    await browser.close();
}

testTaxiResult();
Наверх

Задание 2
Автоматизируй тест-кейс для ya.ru, применив нужные селекторы.

[CASE-2] Проверка появления результатов поиска

Предусловие:
Перейти на страницу ya.ru.

Шаги:
1. Ввести «Автоматизация тестирования» в поисковую строку.
2. Нажать кнопку «Найти».

ОР: Выполнен переход на страницу выдачи поиска и результат поиска непустой.
Решение
Селекторы:

Элемент	Селектор
Поисковая строка	#text
Кнопка «Найти»	.button[type=submit]
Результат поиска	.serp-item
Автотест:

const puppeteer = require('puppeteer');

async function testYaRu() {
    console.log('Запуск браузера');
    const browser = await puppeteer.launch();

    console.log('Создание новой вкладки в браузере');
    const page = await browser.newPage();

    console.log('Переход на страницу ya.ru');
    await page.goto('https://ya.ru/');

    console.log('Ввод текста "Автоматизация тестирования" в поисковую строку');
    const searchField = await page.$('#text');
    await searchField.type('Автоматизация тестирования');

    console.log('Клик в кнопку "Найти"');
    const searchButton = await page.$('.button[type=submit]');
    await searchButton.click();
    
    console.log('Ожидание перехода в страницу поисковых результатов');
    await page.waitForNavigation();
    
    console.log('Получение элементов результата поиска');
    const result = await page.$('.serp-item');
    
    console.log('Сравнение ОР и ФР');
    if (result == null) {
        console.log('Результаты поиска не найдены');
    } else {
        console.log('Результаты поиска отобразились');
    }
    
    console.log('Закрытие браузера');
    await browser.close();
}

testYaRu();
