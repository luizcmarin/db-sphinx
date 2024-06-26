Извлечение META информации
===============================

Sphinx позволяет получать статистическую информацию о последнем выполненном запросе с помощью инструкции [SHOW META](https://sphinxsearch.com/docs/current.html#sphinxql-show-meta) SphinxQL.
Эта информация обычно используется для получения общего количества строк в индексе без дополнительного запроса `SELECT COUNT (*) ...`.
Хотя вы всегда можете запустить такой запрос вручную, `Yiisoft\Db\Sphinx\Query` позволяет вам делать это автоматически без дополнительных усилий.
Все, что вам нужно сделать, это включить `Yiisoft\Db\Sphinx\Query::showMeta` и использовать `Yiisoft\Db\Sphinx\Query::search()` для извлечения всех строк и метаинформации:

```php
$query = new Query();
$results = $query->from('idx_item')
    ->match('foo')
    ->showMeta(true) // включить автоматический запрос 'SHOW META'
    ->search(); // извлечь все строки и META информацию

$items = $results['hits'];
$meta = $results['meta'];
$totalItemCount = $results['meta']['total'];
```

> Note: Общее количество элементов, которое может быть извлечено из 'meta', ограничено опцией sphinx `max_matches`. Если ваш индекс содержит больше записей, чем значение `max_matches` (обычно - 1000), вы должны либо поднять `max_matches` через [[Query::options]], либо использовать [[Query::count()]], чтобы получить количество записей.
