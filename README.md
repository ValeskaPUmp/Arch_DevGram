# `Проект по курсу "Архітектура високонавантажених розподілених систем"`

## `Схема`
![Діаграма](https://github.com/ValeskaPUmp/Arch_DevGram/blob/main/Arch_DiaGram.drawio.png)

## `Опис`

**Моноліт**

В якості бази даних використана **MySQL**, яка буде мати декілька інстансів на різних континентах для зменшення часу відповіді для користувачів.
Маршрутизація запитів від користувачів відбувається за допомогою RabitMQ. 
перед виконанням запит аналізується на наявність шкідливого коду: SQL ін'єкцій, XSS атак, та інших.
Інстанси баз даних синхронізуються за допомогою Azure Sync Master.

Коли користувач викладає Фотографії, ті завантажуться на amazon web services, а в базу даних потрапляє відповідне посилання. При формуванні Review feed до бази даних йде запит на отримання посилань на фото, після чого з amazonwebservices підвантажуються потрібні фотографії.   

Користувач має можливість залишати вподобайки та коментарі під своїм та фото інших користувачів, на яких він підписаний. До бази даних надходять відповідні запити на створення. Ці дані відображаються для інших користувачів, що переглядають дані фото (вподобайки у вигляді лічильника, коментарі в хронологічному порядку). 

Користувач має можливість переглядати фото інших користувачів в розділі "рекомендації" на яких він ще не підписаний. Рекомендації формуються на основі підписок користувачів, на яких підписаний користувач або за тегами, які користувач вказав при завантажені своїх фото або за геолокацією, яку вказував користувач при завантажені своїх фото. Пріоритет показу того чи іншого фото в рекомендації обчислюється на основі лайків кожного фото (більше лайків - вище пріорітет).

Рекомендації визначаються за допомогою функції в PHP, яка повертає список акаунтів, які можуть бути цікаві користувачеві.

## `Таблиці`
```sql
User {
    user_id
    feed_list
    password
    username,
    info,
    location,
    subscriber_counter,
    publishers
}

Tag {
    tag_id
    tag_name
}

TagsPhotos {
    photo_id
    tag_id 
}

Photos {
    photo_id
    photo_key
    uploaded_by // references user_id
    like_number
    location
    description
    created_at
}

Like {
    photo_id
    user_id
}

Comment {
    comment_id
    comment_text
    commented_by // references user_id
    photo_id
    created_at
}
```


Фото, які завантажують користувачі зберігаються за допомогою **Amazon AWS**.
