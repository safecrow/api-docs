# Файлы

К объектам сделка, претензия и доставка (в т.ч. возврат) - можно приложить файлы

### Отображение загруженных файлов

```json
{
  "claim" :
  {
    "attachment" :
    {
      "link" : "/system/claims/attachments/000/000/042/original/screen2.png"
      "name" :"screen2.png",
    }
  }
}
```

Ключ | Значение/Формат значения
--------- | -----------
attachment |JSON объект
attachment.link |	Путь к файлу начиная с домена, префикс /api/v1 добавлять не нужно
attachment.name |	Имя файла
attachment.id | Опциональный id загрузки (там где к объекту может быть несколько загрузок)

## Загрузка файлов

Поле, содержащее файл должно иметь следующую структуру:

> POST /api/v1/orders/1833/claim

```json
{
  "claim" :
  {
    "attachment" :
    {
      "file_name" : "screen2.png",
      "content" : "Base64",
      "content_type" : "image/png"
    }
  }
}
```

Ключ | Значение/Формат значения
--------- | -----------
attachment | JSON объект
attachment.file_name |	Имя файла
attachment.content_type |	[Mime тип файла](#part-6e205da7c20873f3)
attachment.content |	Содержимое файла, закодированное в Base64

## Доступные для загрузки форматы

Mime type | Расшифровка/Расширение
--------- | -----------
application/msword |	Документ MS word (до ms office 2007), .doc
application/pdf |	Документ pdf, .pdf
application/vnd.ms-excel |	Таблица MS Excel (до 2007), .xls
application/vnd.openxmlformats-officedocument.spreadsheetml.sheet |	Таблица MS excel (после 2007), .xlsx
application/vnd.openxmlformats-officedocument.wordprocessingml.document |	Документ MS word (после 2007), .docx
image/gif image/jpeg image/jpg image/pjpeg image/png image/svg+xml image/tiff | Изображение
text/csv |	Структурированные данные с разделителем, .csv
text/plain |	Простой текст, .txt
text/rtf application/rtf | Форматированный текст, .rtf
