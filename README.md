postgres with chinese textsearch support just for my miniflux compatibility

add zhparser and pg_jieba

## build

```bash
git pull
Docker build -t name:tag .
```

## migrate from exist database

1. In `postgresql.conf`, set `default_text_search_config` to `'chinese'`
2. In `psql`, connect to database and run:
   
```sql
UPDATE entries SET document_vectors = setweight(to_tsvector(left(coalesce(title, ''), 500000)), 'A') || setweight(to_tsvector(left(coalesce(content, ''), 500000)), 'B');
```
(This comes from [miniflux](https://github.com/miniflux/v2/blob/5c4df786deae53039e3820ad9d612f35b7c9bc91/internal/storage/entry.go#L81). But also split the link in content to words, looking for better way.)
