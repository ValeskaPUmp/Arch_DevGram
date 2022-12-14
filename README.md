## Проект по курсу "Архітектура високонавантажених розподілених систем"

### Схема
![Діаграма](https://github.com/ValeskaPUmp/Arch_DevGram/blob/main/Arch_DiaGram.drawio.png)

### Опис

**Моноліт**
В якості бази даних використана **MySQL**, яка буде мати декілька інстансів на різних континентах для зменшення часу відповіді для користувачів.

#### Таблиці
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
```

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


Фото, які завантажують користувачі зберігаються в **Amazon S3**.
