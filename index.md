---

---

</slide>

<slide id="first">

![Django](./django_fuer_admins.png)

::: notes
- Django-Grundprinzipien, Erfahrungswerte
- Für Admins von django-basierten Projekten & Entwickler
- Folien verfügbar online
:::
---

<div id="me">

![rixx](./rixx.png)
### rixx

</div>

🗺 rixx.de · 🐦 @rixxtr · 🐘 @rixx@chaos.social

::: notes
- Django-Entwickler, contributor zum Django-Projekt, in der Django-Community aktiv unterwegs
- entwickle kleine/mittlere/große Djangoprojekte, leite dadurch auch andere in ihrem Einsatz an
- betreibe Django in Prod, nicht immer meine eigenen Projekte
:::

---

![Django](./django.png)

::: notes
Web-Framework in Python, eher alt (über 10 Jahre)<br>
professionell: security updates, release-schedule<br>
stabil: DSF, geld, maintainer<br>

Layer für Datenbank und Filesystem<br>
Bekommt Requests, verarbeitet sie, rendert bei Bedarf Templates für die Antwort<br>
:::

===

## manage.py

```shell
$ python ./manage.py help

Type 'manage.py help <subcommand>' for help on a specific subcommand.
Available subcommands:

[auth]
    changepassword
    createsuperuser
[django]
    check
    compilemessages
    createcachetable
    dbshell
    diffsettings
    dumpdata
    flush
    inspectdb
    loaddata
    makemessages
    makemigrations
    migrate
    …
```

::: notes
Interaktion via runserver.
:::

---

# Datenbank

<div id='db-logos'>

%( ![Postgres](./postgres.svg) )
%( ![Mariadb](./mariadb.svg) )
%( ![MySQL](./mysql.svg) )
%( ![SQLite](./sqlite.svg) )
%( ![Oracle](./oracle.png) )

</div>

::: notes
- Unterstützte DBs
- Modelle sind Tabellen
:::

===

# Migrationen

<div id="migrate-code">

```shell
$ python manage.py migrate

Operations to perform:
  Apply all migrations: auth, authtoken, contenttypes, myapp, sessions
Running migrations:
  No migrations to apply.
```

<fragment>

```shell
  Your models have changes that are not yet reflected in a migration, and so won't be applied.
  Run 'manage.py makemigrations' to make new migrations, and then re-run 'manage.py migrate' to apply them.
```

</fragment>

</div>

::: notes
- Werden in Migrationen überführt
- in Reihenfolge, Migrationen verkacken ist idr rettbar, aber nur mit viel Aufwand

DO NOT RUN MAKEMIGRATIONS. IT's like git push -f
:::

---

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

---

## Konfiguration

---

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

:::
