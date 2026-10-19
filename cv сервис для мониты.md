https://stage.cv.itnap.ru/api-task-manager/staging/ <- Старый тестовый CV сервис
https://stage.kb-monita.ru/images/staging/upload <- Старый тестовый для загрузки фото

https://prod.cv.itnap.ru/api-task-manager/production/ <- Текущий боевой CV сервис
http://84.201.167.157/upload <- Текущий боевой для загрузки фото
http://84.201.173.244/upload <- Текущий боевой для загрузки фото


1. Через приложение Postman (или его аналоги) отправить изображение на url: https://test.monitaproject.ru:8444/image

- В Headers записываем X-UID и X-Token (взять из БД - таблицы employees и access_tokens)

![](file:///C:/Users/filippova.elvina.RW/AppData/Local/Packages/oice_16_974fa576_32c1d314_301f/AC/Temp/msohtmlclip1/01/clip_image001.png)

- Вводим url

- Выбираем тип запроса POST

- Заполнить body: тип form-data, key inputfile, value выбрать файл нужного изображения

- Нажать send

![](file:///C:/Users/filippova.elvina.RW/AppData/Local/Packages/oice_16_974fa576_32c1d314_301f/AC/Temp/msohtmlclip1/01/clip_image003.jpg)

В ответ должны вернуться ссылки на загруженное в s3 изображение вида:

{

    "result": "success",

    "image_url": "2024/12/5/d3/d37a13d5-2123-4069-aa0f-32ca20adde74",

    "full_url": "https://storage.yandexcloud.net/main-stage-kb-monita-images/2024/12/5/d3/d37a13d5-2123-4069-aa0f-32ca20adde74"

}

2. Отправляем в CV сервис задачу на распознавание изображения. url: [http://stage.cv.itnap.ru/api-task-manager/staging/](http://stage.cv.itnap.ru/api-task-manager/staging/)

- В Headers записываем api-key

![](file:///C:/Users/filippova.elvina.RW/AppData/Local/Packages/oice_16_974fa576_32c1d314_301f/AC/Temp/msohtmlclip1/01/clip_image005.jpg)

- Вводим url

- Выбираем тип запроса POST

- Заполнить body: тип raw и в нём JSON:

- В path вводим полученный после отправки image_url и учесть, что запись с id (здесь в примере 129649) должна существовать в БД

{

    "base_url": "https://storage.yandexcloud.net/main-stage-kb-monita-images/",

    "images": [

        {

            "id": 129649,

            "path": "2024/12/5/d3/d37a13d5-2123-4069-aa0f-32ca20adde74",

            "type": "retail",

            "is_cigarettes": false,

            "is_shelf": false

        }

    ],

    "recognize_task_id": 16424828,

    "last_recognize_time": 0.45

}

- Нажать send

В ответ должно вернуться:

{

    "result": "success"

}

![](file:///C:/Users/filippova.elvina.RW/AppData/Local/Packages/oice_16_974fa576_32c1d314_301f/AC/Temp/msohtmlclip1/01/clip_image007.jpg)

3. Смотрим результат распознавания:

- В БД мониты идём в таблицу recognize_images и находим там запись по id (здесь в примере 129649)

- В поле recognize_result будет результат распознавания sku probabilities price итд

- Если продукт на изображении известен CV сервису, то sku должно соответствовать реальности (соответствие названия и sku смотреть в таблице products)

и probability должен быть высоким, например 0.7 и выше, если probability 0.1 - 0.5, то скорее всего такогй товар CV сервису не известен и он вернул что-то наиболее похожее на тот продукт, который был в отправленном изображении