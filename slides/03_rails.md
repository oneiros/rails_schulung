!SLIDE

# Einführung in Rails

!SLIDE

## Was ist Rails?

-   MVC-Framework zur Erstellung von datenbankgestützten Webapplikationen
-   Rails basiert auf Ruby
-   Freie Software (MIT)
-   Ideal für CRUD-Anwendungen
-   Opinionated Software
-   "Optimized for developer happiness"
-   [www.rubyonrails.org](http://www.rubyonrails.org)

!SLIDE

## Webanwendungen historisch betrachtet

-   **ca. 1995:** CGI mit Perl oder C
-   **ca. 1998:** PHP
-   **ca. 2000:** JSP und/oder Servlets
-   **ca. 2002:** Java-Frameworks
-   **2004:** Ruby on Rails

!SLIDE

## DHH

![image](images/wired.jpg)

!SLIDE

## Basecamp

![image](images/basecamp.png)

!SLIDE

## Grundsatz 1

* **D** o not
* **R** epeat
* **Y** ourself

!SLIDE

## Grundsatz 2

### Convention Over Configuration

!SLIDE

## Model-View-Controller

![image](images/mvc.png)

!SLIDE

## REST

-   Representational State Transfer
-   Dissertation von Roy Fielding
-   Ressourcen eindeutig identifiziert durch URI (z.B.
    http://www.linuxhotel.de/kurs/ruby\_on\_rails)
-   HTTP-Methoden zur Manipulation von Ressourcen
-   Leichtgewichtige Web Services
-   Designansatz für webbasierte Software

!SLIDE

## HTTP-Methoden

| HTTP    | CRUD   |
|---------|--------|
| POST    | Create |
| GET     | Read   |
| PUT     | Update |
| DELETE  | Delete |

!SLIDE

## Architektur

![image](images/architecture.png)

!SLIDE

## Projekt erstellen

~~~~bash
rails new <projekt>
~~~~

!SLIDE

## Verzeichnisstruktur 1

-   app
-   config
-   config.ru
-   db
-   doc
-   Gemfile
-   lib

!SLIDE

## Verzeichnisstruktur 2

-   log
-   public
-   Rakefile
-   README
-   script
-   test
-   tmp
-   vendor

!SLIDE

## Das app/-Verzeichnis

-   assets
-   controllers
-   helpers
-   models
-   views

!SLIDE

## Drei Umgebungen

-   development: Zum Entwickeln
-   test: Für automatisierte Tests
-   production: Für die Produktion
-   beliebige eigene Umgebungen

!SLIDE

## Datenbankkonfiguration

-   Datei config/database.yml
-   YAML-Format: YAML Ain't a Markup Language
-   Eine Datenbank pro Umgebung
-   Default: sqlite3
-   Support für MySQL, PostgreSQL und viele weitere

!SLIDE

## Freundliche Helfer: rails generate

-   Generiert Codegerüste
-   Kurz: rails g
-   Beispiele
    -   rails generate model &lt;name&gt;
    -   rails generate controller &lt;name&gt;
-   rails destroy zum Rückhängigmachen

!SLIDE

## Models

-   Erben jede Menge Verhalten von ActiveRecord::Base
-   Keine Konfiguration, wenn man Konventionen folgt
-   Datenbanktabelle "projects" &lt;=&gt; Model-Klasse "Project"
-   Deklarative Definition von Beziehungen und Validierungen
-   rails g model &lt;name&gt; &lt;attribut&gt;:&lt;datentyp&gt; ...

!SLIDE

## Migrations

-   Schemaänderungen in Ruby-Code
-   Migration von Daten
-   Versionierte (Timestamp) Migration-Dateien in db/migrations/
-   Gleicher Stand der DB-Struktur auf allen Systemen und in allen
    Umgebungen
-   Möglichkeit eines Rollbacks
-   rake db:migrate

!SLIDE

## CRUD

~~~~ruby
params = {:title => "MyProject"}

# Create
Project.create(params)
p = Project.new(params) ; p.save # true

# Read
p = Project.find(1)
p = Project.find_by_title("MyProject")
p = Project.all

# Update
p.update_attributes(:title => "NewTitle") # true

# Delete
p.destroy
~~~~

!SLIDE

## Views

-   ERb-HTML-Templates
-   Hierarchie aus Layout, Template und Partial(s)
-   Spezielle Tags &lt;% %&gt; erlauben Ausführen von Ruby-Code
-   &lt;%= "hallo" %&gt; erlaubt das Einbetten von Werten in das Dokument
-   Volle Ruby-Möglichkeiten - erfordert Disziplin!
-   Helper für Formulare, Text und vieles mehr
-   Werden nach den zugehörigen Controllern strukturiert: rails g controller &lt;name&gt;

!SLIDE

## Routing

-   Vorschriften, wie HTTP-Methode + URL-Pfad auf Controller und Methoden (Actions) darin gemappt wird
-   Beispiel: GET /projects ruft Methode index der Klasse ProjectsController auf
-   Definition in config/routes.rb
-   Einfachster Fall: resources :projects
    -   Kurzform, um 7 "restful" Routen anzulegen für Standard-CRUD
-   rake routes listet alle Routen
-   Hilfsmethoden zur Pfad-/URL-Generierung
-   root für die Startseite (index.html löschen!)

!SLIDE

## Controller

-   Workflow
    -   Empfängt Anfrage
    -   Interagiert mit Models (z.B. CRUD)
    -   Erzeugt eine Ausgabe
-   public-Methoden sind aufrufbare "Actions"
-   Eine Action kann unterschiedliche Ausgabeformate erzeugen
-   Zugriff auf Parameter, Header, Session usw.

!SLIDE

## Scaffolding

* rails g scaffold &lt;name&gt; &lt;attribute&gt; ...
* Generiert Model, Views und Controller
* HTML-Views + JSON-API
* Guter Startpunkt, aber passt es auch wirklich?
* Code verstehen und Zeitersparnis hinterfragen

!SLIDE

## Einfache Beziehungen zwischen Models

-   Ein Projekt hat viele Posts
-   Ein Post gehört eindeutig zu einem Projekt
-   In Project: has\_many :posts
-   In Post: belongs\_to :project
-   Impliziert Spalte project\_id in Tabelle posts
-   OO-Abstraktion durch Zugriffsmethoden: post.project, project.posts

!SLIDE

## Validierung

~~~~ruby
class Project
  validates_presence_of :title
  validates_uniqueness_of :title
end
class Post
  validates_presence_of :subject, :body
  validates_length_of :subject, :minimum => 10
end
~~~~

!SLIDE

## Logging

-   logger-Objekt mit Methoden für verschiedene Loglevel
-   Loglevel: debug, info, warn, error, fatal
-   Pro Umgebung kann konfiguriert werden, ab welchem Level tatsächlich geloggt wird
-   Logdateien in log/ benannt nach Umgebung

!SLIDE

## API-Dokumentation

### [api.rubyonrails.org](http://api.rubyonrails.org)

!SLIDE

# Praxis

!SLIDE

## Ticketsystem

-   Ziel: Ticketsystem zum Erfassen und Bearbeiten von Vorfällen
-   Erster Schritt: MVC für Tickets und Personen
-   Ticket hat mindestens Betreff, Status und Beschreibungstext
-   Personen haben mindestens Vor- und Nachnamen, sowie E-Mailadresse
-   Alle vorgenannten Daten sind Pflichtangaben
-   Ein Ticket gehört eindeutig einer Person - eine Person kann viele Tickets haben

