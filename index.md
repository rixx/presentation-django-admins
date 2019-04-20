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

<div id="full-image">

![releases](./django_releases.png)

</div>

===

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

%(media) vs %(static)

::: notes
Statisch: `collectstatic`, dann einfach ausliefern -- oder whitenoise
:::

===

## Dateien

```shell
$ python ./manage.py collectstatic

You have requested to collect static files at the destination
location as specified in your settings:

    /home/myproject/src/myproject/src/static.dist

This will overwrite existing files!
Are you sure you want to do this?

Type 'yes' to continue, or 'no' to cancel: yes

496 static files copied to '/home/myproject/src/myproject/src/static.dist', 517 post-processed.
```

---

## Installation: Umgebung

- %(Global)
- %(User environment / `pip install --user`)
- %(Virtualenv / `python -m venv`)
---

## Installation: Nützliche Pfade

- `/usr/lib/python3.6/site-packages/`
- `~/.local/lib/python3.6/site-packages/`
- `/path/to/venv/lib/python3.6/site-packages/`

::: notes
Außerdem ~/.bin
:::

===

## Installation

- %(`pip install myproject`)
- %(`pip install .`)
- %(`pip install -r requirements.txt`)

---

## Konfiguration

- %(`local_settings.py`)
- %(`settings.py`)
- %(`something.cfg`)
- %(ENV)

===

## Konfigurationsvariablen

<div id="config-code">

<fragment>

```python
DEBUG = False
```

</fragment>

<fragment>

```python
SECRET_KEY = 'skjdfölajfalskjfdsaölk…'
```

</fragment>

<fragment>

```python
ALLOWED_HOSTS = ['example.org', '172.0.0.21']
```

</fragment>

<fragment>


```python
DATABASES = {'default': {
  'ENGINE': 'django.db.backends.postgresql',
  'NAME': 'myproject',
  'USER': …
}}
```


</fragment>

<fragment>

```python
STATIC_ROOT = '/var/www/project/static/'
MEDIA_ROOT = '/var/www/project/media/'
```

</fragment>

</div>

===

## Konfigurationsvariablen: Proxy

<div id="config-code">

```python
SECURE_PROXY_SSL_HEADER = ('HTTP_X_FORWARDED_PROTO', 'https')
SECURE_SSL_REDIRECT = False
USE_X_FORWARDED_HOST = True
USE_X_FORWARDED_PORT = True
```

</div>

===

## Bonus: Mail-Debugging

<div id="config-code">

```python
ADMINS = [
    ('Admin1', 'me@admin.org'),
    ('Admin2', 'me@example.org'),
]

EMAIL_HOST = ''
EMAIL_HOST_PASSWORD = ''
EMAIL_HOST_USER = ''
EMAIL_PORT = 25
EMAIL_USE_TLS = True
EMAIL_USE_SSL = False
SERVER_EMAIL = 'sender@example.org'
```

</div>

---

## Services


```shell
pip install gunicorn
```

<p></p>

<div id="config-code">

```ini
[Unit]
Requires=myproject.socket
After=network.target

[Service]
User=myproject
WorkingDirectory=/home/myproject/src
ExecStart=/home/myproject/.local/bin/gunicorn --bind unix:/run/gunicorn/myproject --workers 4 myproject.wsgi
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID
```

</div>

::: notes
nicht uwsgi
:::


---

## Maintenance: Backups

- Datenbank
- Mediafiles
- %(`site-packages`)

===

## Maintenance: Updates

- `pip install (--user) -U …`
- `python ./manage.py migrate`
- `python ./manage.py collectstatic`
- %(`python ./manage.py compress`)
- %(`python ./manage.py compilemessages`)

===

## Maintenance: Cronjobs

- Manage commands
- `python ./manage.py clearsessions`

===

## Ressourcen

[Django Docs!](https://docs.djangoproject.com)

- [Supported versions](https://www.djangoproject.com/download/#supported-versions)
- [Security releases](https://www.djangoproject.com/rss/weblog/)
- [Settings](https://docs.djangoproject.com/en/dev/ref/settings/)
