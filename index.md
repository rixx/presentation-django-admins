---

---

# Django für Admins

::: notes
- Django-Grundprinzipien, Erfahrungswerte
- Verfügbar online
:::
---

### rixx

::: notes
- Django-Entwickler, contributor zum Django-Projekt, in der Django-Community aktiv unterwegs
- entwickle kleine/mittlere/große Djangoprojekte, leite dadurch auch andere in ihrem Einsatz an
- betreibe Django in Prod, nicht immer meine eigenen Projekte
:::

---

![Django](./django.png)

::: notes
- Web-Framework in Python
- alt (über 10 Jahre)
- professionell: security updates, release-schedule
- stabil: DSF, geld, maintainer

- Funktionsweise:
  - Layer für Datenbank und Filesystem
  - Bekommt Requests, verarbeitet sie, rendert bei Bedarf Templates für die Antwort
- Interaktion: manage.py
:::

---

# Datenbank

<div id='db-logos'>

%( ![Postgres](./postgres.svg) )
%( ![Mariadb](./mariadb.svg) )
%( ![MySQL](./mysql.svg) )
%( ![SQLite](./sqlite.svg) )

</div>

::: notes
Unterstützte DBs
Modelle schreiben
Werden in Migrationen überführt
in Reihenfolge, Migrationen verkacken ist idr rettbar, aber nur mit viel Aufwand
NICHT ERSTELLEN
:::

===

## Dateien

%(statisch) vs %(media)


===

## Dateien

```python{2}
STATIC_ROOT=
MEDIA_ROOT=
```


::: notes
wo kommen sie her, wie gehen sie hin
:::

---

## Installation

===

## Konfiguration

===

## Services

::: notes
gunicorn vs uwsgi
:::


---

## Pflege

backups
updates
cronjobs
sessions

::: notes
- Ressourcen
  - Docs: Kompatibilität/Support
# Code

:::
