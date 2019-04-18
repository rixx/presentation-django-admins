---

---

# Django für Admins

---

### rixx

::: notes

- ich: Django-Entwickler, contributor zum Django-Projekt, in der Django-Community aktiv unterwegs
- diverse kleine, mittlere und große Djangoprojekte, einige davon betreibe ich auch selber in prod
- Vortrag: Django-Grundprinzipien, Erfahrungswerte, blablabla. Slides verfügbar

:::

---

![Django](./django.png)
::: notes

- Web-Framework in Python
- alt, stabil (über 10 Jahre)
- professionell: security updates, update-schedule
- professionell: DSF, geld, maintainer
jetzt kurz: Überblick/Funktionsweise
:::

===

## Datenbank

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

```js{2}
import Lib from 'lib'
const foo = new Lib() // you can highlight lines
const foo.bar()
```
:::
