!SLIDE

# Deployment und Skalierung

!SLIDE

## Deploymentszenarien

-   Webservermodul: Passenger (für Apache und nginx)
-   Ruby-Applicationserver, wie z.B.
    -   mongrel
    -   thin
    -   unicorn
-   Java-Applicationserver und JRuby
-   Parallelität durch Threads oder Prozesse

!SLIDE

## Passenger

-   Modul für Apache2 und nginx
-   Ziel: PHP-ähnliches Deployment
-   Optimal für Einsteiger und Shared Hosting
-   gem install passenger
-   Teilweise auch OS-Pakete oder Repositories verfügbar

!SLIDE

## Ruby-Applicationserver 1

![image](images/appserver.png)

!SLIDE

## Ruby-Applicationserver 2

-   Frontendwebserver (z.B. Apache, nginx, LB)
-   Mongrel: Früher Platzhirsch, heute kaum maintained
-   Thin: Schlank und schnell
-   Unicorn: Prozessmanagement und Unix-Sockets
-   Separation OS- und Rubywelt möglich

!SLIDE

## Capistrano

-   Paralleles Ausführen von Kommandos auf entfernten Systemen via SSH
-   Anleihen bei Rake
-   Standard-Tasks zum Deployment von Rails-Anwendungen
-   Einfaches Rollback
-   gem install capistrano
-   [capify.org](http://capify.org)

!SLIDE

## Voraussetzungen

-   Versionskontrollsystem (svn, git o.ä.)
-   Ein oder mehrere Produktionsserver
-   Zugriff auf diese Server per SSH (public-key auth)

!SLIDE

## Server-Rollen

-   Capistrano unterscheidet standardmäßig drei Rollen: web, app, und db
-   Ein Server kann alle drei Rollen einnehmen
-   Pro Rolle kann mehr als ein Server definiert werden
-   :primary-Parameter, um primären Server einer Rolle anzugeben

!SLIDE

## Ablauf des Deployments

-   Letzte Version in ein neues Verzeichnis auschecken
-   Symbolischen Link des aktuellen Verzeichnisses auf das neue
    Verzeichnis setzen
-   (Web-/App-)Server neu starten

!SLIDE

## Erste Schritte

-   Capfile anlegen mit capify .
-   config/deploy.rb anpassen
-   Verzeichnisstrukur anlegen: cap deploy:setup
-   Erstmaliges Deployment: cap deploy:cold
-   Jedes weitere Deployment: cap deploy

!SLIDE

## Skalieren von Rails-Anwendungen

-   Entgegen anderslautender Gerüchte skalieren Rails-Anwendungen sehr
    gut
-   Mindestens 1 Prozess pro CPU(-Kern)
-   Mit "shared nothing"-Architektur über mehrere Server skalieren
-   Infrastruktur im Auge behalten
-   Frontend-Performance

!SLIDE

## Optimieren von Rails-Anwendungen

-   Algorithmisch (Profiler)
-   Datenbank
-   Caching

!SLIDE

## Datenbank optimieren

-   DBMS tunen
-   Indizes setzen
-   Eager Loading verwenden, um "versteckte" Abfragen einzusparen
-   oder: Eager Loading verhindern, um große Abfragen zu vermeiden
-   SQL für komplexe Abfragen verwenden
-   NoSQL?

!SLIDE

## Caching

-   Selbst in hochdynamischen Systemen muss nicht immer alles neu
    berechnet werden
-   Rails bietet Caching von Views
-   Caching als allgemeines Prinzip auch darüberhinaus anwendbar (z.B.
    mit memcached)
-   Achtung: Erhöhte Komplexität und erschwerte Fehlersuche

!SLIDE

## Page Caching

-   caches\_page im Controller cached ganze Seiten
-   Passend benannte, statische HTML-Datei wird in public/ angelegt
-   Diese Datei wird vom Webserver ausgeliefert, ohne dass die Anwendung
    bemüht wird
-   expire\_page invalidiert eine gecachte Seite

!SLIDE

## Action Caching

-   caches\_action analog zu caches\_page
-   Unterschied: Filter werden ausgeführt
-   Dadurch ist z.B. eine Zugriffskontrolle möglich
-   Dennoch Geschwindigkeitsvorteile
-   expire\_action zum Invalidieren

!SLIDE

## Fragment Caching

-   Cachen einzelner HTML-Fragmente einer View
-   cache { ... }
-   fragment\_exist? um zu testen, ob ein Fragment gecached wurde
-   expire\_fragment zum Invalidieren
-   Fragmente werden nach Controller, Action und ggf. Action Suffix
    benannt

!SLIDE

## Vorgehensweise

-   "Optimiert wird später"
-   Architektur sauber aufbauen ("shared nothing")
-   Alle Möglichkeiten offen halten
-   Echte Performanceprobleme anhand der laufenden Anwendung
    identifizieren

