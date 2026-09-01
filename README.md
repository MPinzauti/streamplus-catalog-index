# streamplus-catalog-index

Indice pubblico, di sola lettura, che mappa i titoli del catalogo di uno
specifico sito di streaming (identificati da un id/slug interno) ai
rispettivi `tmdb_id`/`imdb_id` su [TMDB](https://www.themoviedb.org).

Serve a un'app che usa TMDB come fonte primaria per catalogo/ricerca/
classifiche: per ogni titolo TMDB, un lookup in questo file dice se e dove
è disponibile sul sito sorgente, senza dover interrogare il sito ad ogni
schermata.

Generato e aggiornato periodicamente da uno script offline (non incluso
in questo repo). Nessun contenuto multimediale, nessun contenuto protetto
da copyright: solo metadati (titolo, anno, id di riferimento incrociati).

## File

`sc_index.json`

```jsonc
{
  "schema_version": 2,
  "generated_at": "2026-09-01T00:09:27Z",
  "count": 18333,
  "titles": [
    {
      "id": "3",                 // id interno del sito sorgente
      "slug": "breaking-bad-...",
      "name": "Breaking Bad - ...",
      "type": "tv",               // "movie" | "tv"
      "last_air_date": "2008-01-01",
      "tmdb_id": 1396,             // null se non trovato su TMDB
      "imdb_id": "tt0903747",
      "status": "Ended",
      "seasons": [
        { "number": 1, "episodes_count": 7 }
      ],
      "enriched_at": "2026-08-31T03:52:30Z"
    }
  ]
}
```

## Consumo

Scaricare `sc_index.json` via HTTP GET condizionale
(`If-None-Match`/`If-Modified-Since`) su:

```
https://raw.githubusercontent.com/MPinzauti/streamplus-catalog-index/main/sc_index.json
```

Un `304 Not Modified` significa che l'indice locale è già aggiornato,
niente da riscaricare.
