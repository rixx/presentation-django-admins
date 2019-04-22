---

---

<small>

Press s to see the speaker notes.

<br>

[Find the recording here.](https://media.ccc.de/v/eh19-198-django-fr-admins)

</small>

</slide>

<slide id="first">

![Django](./django_fuer_admins.png)

::: notes
Die Präsentation geht auf Django-Grundprinzipen und Erfahrungswerte im Deployment ein.

Sie ist primär für Admins von Django-basierten Projekten, aber auch für Entwickelnde gedacht.
:::
---

<div id="me">

![rixx](./rixx.png)
### rixx

</div>

🗺 rixx.de · 🐦 @rixxtr · 🐘 @rixx@chaos.social

::: notes

Ich bin rixx. Ich bin ein Django-Entwickler (habe kleine/mittlere/große Projekte entwickelt) und betreibe eigene und
fremde Django-Projekte in Produktion. Ich bin auch aktiv in der Django-Community, habe zu Django selber (und zum
Ökosystem) beigetragen, und 2018 die DjangoCon Europe in Heidelberg ausgerichtet.
:::

---

![Django](./django.png)

::: notes
Django ist ein sehr stabiles, schon gut abgehangenes (+10 Jahre) Web-Framework in Python. Durch sein Alter ist viel
Stabilität gewährleistet, so gibt es etwa eine Stiftung hinter Django, die DSF, die ein bis zwei Fellows einstellt, die
sich um Releases und das Management der Issues und Pull Requests kümmern.
:::

===

<div id="full-image">

![releases](./django_releases.png)

</div>

::: notes
Auch die Updates und der Support für einzelne Versionen, sowie für Abhängigkeiten wie Datenbankversionen sind klar
geregelt. LTS-Versionen sind für drei Jahre unterstützt. Sicherheitsupdates werden über eine private Adresse und ein
routiniertes Team geregelt.
:::
===

![Django](./django.png)

::: notes
Wie für Web-Frameworks üblich, bietet Django für die Entwickelnden ein Layer zur Interaktion mit HTTP-Requests, und
außerem sehr viel Funktionalität etwa zur Input-Validation, zur Interaktion mit Dateisystemen, oder zum rendern von HTML
in Templates. Der größte Verkaufspunkt ist aber das Datenbank-Layer (das ORM).
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
Interaktionen mit Django passieren primär via `python manage.py <befehl>`. Der `help`-Befehl dokumentiert die Befehle
gut. Sehr nützlich z.B.: `manage.py dbshell`, liest die Verbindungskonfig zur Datenbank und öffnet dann eine
Datenbankshell.
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
Für weitere DBs gibt es Adapter-Bibliotheken (z.B. MSSQL), die aber oft etwas weniger mächtig sind als der
Funktionsumfang in Django. Django normalisiert soweit es geht Unterschiede zwischen Datenbanken weg – dh der Umstand,
dass die meisten Entwickelnden der Bequemlichkeit halber gegen SQLite testen, kann in Produktion zu Überraschungen
sorgen. Persönliche Empfehlung, aus Prinzip: Postgres.
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
Entwickelnde schreiben "Modelle", die Tabellen in Datenbanken entsprechen. Migrationen (in
`migrations/<zahl>_<titel>.py`) bilden Änderungen an Modellen ab, und werden von Django beim Ausführen von `migrate` in
den passenden SQL-Dialekt übersetzt und ausgeführt. `showmigrations` zeigt den aktuellen Stand. Fehler sind fixbar
(insbesondere dank `--fake`), aber nervig.

DO NOT RUN MAKEMIGRATIONS. It's like git push -f.
:::

---

## Dateien

%(media) vs %(static)

::: notes
Media-Files: User-uploaded, oder von Django generiert. Auslieferung nicht via Django, sondern via den vorgeschalteten
Webserver. Redet mit den Entwickelnden, ob wirklich alle Media-Files einfach public sein sollen. Sonst: Schützenswerte
unter sub-path, den im Web-Server nicht ausliefern, und die Entwicklung muss dafür Authorisierung bauen.
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

::: notes
Statische Dateien sind JS, CSS, Bilder, usw. `collectstatic` laufen lassen, dann einfach ausliefern -- außer das Projekt
setzt `whitenoise` ein, dann werden die statischen Dateien von Django ausgeliefert. Möglicherweise wird auch noch etwas
zur Komprimierung der Dateien eingesetzt, dann muss nach `collectstatic` noch `compress` laufen.
:::

---

## Installation: Umgebung

- %(Global)
- %(User environment / `pip install --user`)
- %(Virtualenv / `python -m venv`)

::: notes
Bitte nicht im globalen Kontext (was `pip`-Default ist) Projekte installieren. Stattdessen entweder das übliche
virtualenv, oder das noch coolere `pip install --user` nehmen.
:::

---

## Installation: Nützliche Pfade

- `/usr/lib/python3.6/site-packages/`
- `~/.local/lib/python3.6/site-packages/`
- `/path/to/venv/lib/python3.6/site-packages/`

::: notes
In diesen Pfaden finden sich je nach Installationsmechanismus die installierten Pakete.
:::

===

## Installation

- %(`pip install myproject`)
- %(`pip install .`)
- %(`pip install -r requirements.txt`)

::: notes
Egal welcher dieser Befehle der richtige für euer Projekt ist, bitte guckt in die betreffende Datei (`setup.py` oder
`requirements.txt`, üblicherweise) und seht zu, ob Dependencies fix sind. Empfehlung: mindestens auf minor version
pinnen, entweder durch `dep>=5.3,<5.4` oder durch `dep==5.3.*`.
:::

---

## Konfiguration

- %(`local_settings.py`)
- %(`settings.py`)
- %(`something.cfg`)
- %(ENV)

::: notes
Es gibt einen Haufen Config-Ansätze, aber keinen prävalenten. Scheut nicht davor zurück, der Entwicklung eure
Wünsche/Bedürfnisse mitzuteilen. Falls es wirklich gar nichts gibt, legt eine `local_settings.py` neben die
`settings.py` und importiert sie **am Ende** der `settings.py`` mit `from .local_settings import *`, dann müsst ihr in
der `settings.py` bei Updates immerhin nur noch die eine Zeile pflegen.
:::

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

::: notes
DEBUG muss falsch sein, egal, was ihr sonst tut.
:::

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

::: notes
Denkt daran, diese Header in eurem Server zu setzen, und sicherzustellen, dass ihr keine von Endusern eingereichten
Werte für diese Header durchlasst. Handhabt Redirects auf HTTPS auch auf eurer Seite, nicht in Django.
:::

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
LOGGING['handlers']['mail_admins'] = {
    'level': 'ERROR',
    'class': 'django.utils.log.AdminEmailHandler',
}
```

</div>

::: notes
Diese Konfig sendet Mails an euch, wenn der Server einen unerwarteten HTTP 500 zurückgibt. Die Mail enthält einen
kompletten Traceback. Achtung vor Mailspam, aber trotzdem ist es irre nützlich, um den Entwickelnden brauchbares
Feedback bei Fehlern zukommen zu lassen.
:::

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
Gunicorn ist besser als uwsgi, aber beide funktionieren, und leiten HTTP-Requests im für Django brauchbaren WSGI-Format
weiter. Systemd-Servicefile hier nur als Beispiel.
:::


---

## Maintenance: Backups

- Datenbank
- Mediafiles
- %(`site-packages`)

::: notes
Site-packages nur ins Backup, wenn ihr einen reproducible build (as in: 100%, jede Abhängigkeit) braucht.
:::

===

## Maintenance: Updates

- `pip install (--user) -U …`
- `python ./manage.py migrate`
- `python ./manage.py collectstatic`
- %(`python ./manage.py compress`)
- %(`python ./manage.py compilemessages`)

::: notes
Backups vor Updates nicht vergessen.
:::

===

## Maintenance: Cronjobs

- Manage commands
- `python ./manage.py clearsessions`

::: notes
`clearsessions` alle Woche oder einmal im Monat ausführen, wirft abgelaufene Sessions aus der DB. Alle anderen Cronjobs
sollten auch Manage-commands (dh über manage.py ausführbar) sein. Wenn nicht, sind die Chancen groß, dass die
Entwicklenden das hätten machen sollen.
:::

===

## Ressourcen

[Django Docs!](https://docs.djangoproject.com)

- [Supported versions](https://www.djangoproject.com/download/#supported-versions)
- [Security releases](https://www.djangoproject.com/rss/weblog/)
- [Settings](https://docs.djangoproject.com/en/dev/ref/settings/)

[Recording](https://media.ccc.de/v/eh19-198-django-fr-admins)
