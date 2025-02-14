# opensearch-webinar

### Плагины OpenSearch

#### Казахский анализатор

Настроенный анализатор с лемматизацией для казахского языка. Он заменяет специфические символы на более универсальные аналоги.

#### Конфигурация
```json
{
  "settings": {
    "analysis": {
      "filter": {
        "kazakh_replace_ae": { "type": "pattern_replace", "pattern": "ә", "replacement": "а" },
        "kazakh_replace_ng": { "type": "pattern_replace", "pattern": "ң", "replacement": "н" },
        "kazakh_replace_gh": { "type": "pattern_replace", "pattern": "ғ", "replacement": "г" },
        "kazakh_replace_ue": { "type": "pattern_replace", "pattern": "ү", "replacement": "у" },
        "kazakh_replace_uu": { "type": "pattern_replace", "pattern": "ұ", "replacement": "у" },
        "kazakh_replace_oe": { "type": "pattern_replace", "pattern": "ө", "replacement": "о" },
        "kazakh_replace_q":  { "type": "pattern_replace", "pattern": "қ", "replacement": "к" },
        "kazakh_replace_h":  { "type": "pattern_replace", "pattern": "һ", "replacement": "х" },
        "kazakh_replace_i":  { "type": "pattern_replace", "pattern": "і", "replacement": "и" },
        "yo_ye":            { "type": "pattern_replace", "pattern": "ё", "replacement": "е" }
      },
      "analyzer": {
        "kazakh-lemmer-improved": {
          "type": "custom",
          "tokenizer": "standard",
          "filter": [
            "lowercase",
            "kazakh_replace_ae",
            "kazakh_replace_ng",
            "kazakh_replace_gh",
            "kazakh_replace_ue",
            "kazakh_replace_uu",
            "kazakh_replace_oe",
            "kazakh_replace_q",
            "kazakh_replace_h",
            "kazakh_replace_i",
            "yo_ye",
            "yandex_lemmer"
          ]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "book": {
        "type": "text",
        "analyzer": "kazakh-lemmer-improved"
      }
    }
  }
}
```

#### Создание индекса
```sh
curl --user admin:пароль \
    --cacert ~/.opensearch/root.crt \
    --header 'Content-Type: application/json' \
    --request PUT "https://your-opensearch-url:9200/index-with-kazakh-filters?pretty" \
    --data @config.json
```

#### Добавление документа
```sh
curl --user admin:пароль \
    --cacert ~/.opensearch/root.crt \
    --header 'Content-Type: application/json' \
    --request POST "https://your-opensearch-url:9200/index-with-kazakh-filters/_doc?pretty" \
    --data '{
              "book": "Қазақтың қара сөзі",
              "author": "Абай Құнанбаев"
            }'
```

#### Поиск документа
```sh
curl --user admin:пароль \
    --cacert ~/.opensearch/root.crt \
    --header 'Content-Type: application/json' \
    --request GET 'https://your-opensearch-url:9200/index-with-kazakh-filters/_search?pretty' \
    --data '{
              "query": {
                "query_string": {
                  "query": "book: Казактын кара сози"
                }
              }
            }'
```

