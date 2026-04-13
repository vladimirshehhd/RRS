# Этап 6. Диаграммы последовательностей

**Файл:** `docs/06_sequence_diagrams.md`

## 1. Пользователь открывает сайт -> загрузка новостей -> отображение

```text

Browser -> NewsController: GET /
NewsController -> FeedService: get_feed(default_source, force_refresh=false)
FeedService -> ISettingsStorage: load_settings()
FeedService -> IFeedRepository: load_last_snapshot(source_id)
alt кэш актуален
    IFeedRepository -> FeedService: FeedSnapshot
else кэш устарел или пуст
    FeedService -> IRssProvider: fetch_feed(source)
    IRssProvider -> External RSS: HTTP GET RSS
    External RSS -> IRssProvider: XML / RSS
    IRssProvider -> FeedService: RssFeedDTO
    FeedService -> FeedService: map DTO -> Domain, remove duplicates, sort by date
    FeedService -> IFeedRepository: save_snapshot(snapshot)
end
FeedService -> NewsController: FeedSnapshot
NewsController -> Browser: HTML с новостями
Browser -> Пользователь: отображение списка новостей
```
