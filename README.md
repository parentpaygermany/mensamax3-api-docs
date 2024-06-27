# Bauen der Dokumentation

## Bauen vom Slate Dockerfile

In diesem Verzeichnis:

```sh
docker-compose up

# in anderem Konsolentab, selber Ordner
docker-compose exec docs bundle exec middleman build
```

Die Build-Ausgabe sollte dann in `./build` liegen, der ganze Ordner kann dann deployed werden.


