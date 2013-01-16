!SLIDE

# Plugins

!SLIDE

## Rails erweitern

-   gems vs. plugins
-   Rails 3 Kompatibilität: [railsplugins.org](http://railsplugins.org)
-   Plugin-Seiten:
    [agilewebdevelopment.com/plugins](http://agilewebdevelopment.com/plugins)
    [railslodge.com](http://railslodge.com)
-   Aber auch: [github.com](http://www.github.com)
    [rubygems.org](http://rubygems.org)
    [ruby-toolbox.com](http://www.ruby-toolbox.com)

!SLIDE

## Paperclip

-   Handling von Datei-Uploads
-   Dateiablage im Dateisystem oder bei Amazon's S3
-   Automatisches Skalieren von Bildern, Erstellen von Thumbnails
-   gem 'paperclip'

!SLIDE

## Paperclip Beispiel: Model

~~~~ {.brush: .ruby}
class User < ActiveRecord::Base
  has_attached_file :avatar,
    :styles => {
      :thumbnail => "64x64>",
      :medium => "128x128>"
    }
end
~~~~

!SLIDE

## HAML

-   Alternative HTML-Templates
-   Kompakte Syntax
-   Vollständiger Ersatz für ERb
-   Spaltet die Rails-Community
-   gem 'haml'
-   Website: [haml-lang.com](http://www.haml-lang.com)

!SLIDE

## Formtastic

-   Smarter Ersatz für eingebaute Form-Helper
-   Sinnvolle defaults
-   Generiert ausführliches, semantisches Markup
-   gem 'formtastic'
-   Alternative: simple\_form

!SLIDE

## Formtastic Beispiel

~~~~ {.brush: .ruby; .html-script: .true;}
<%= semantic_form_for @to_do do |f| %>
  <%= f.inputs %>
  <%= f.buttons %>
<% end %>
~~~~

!SLIDE

## Admin-Oberflächen

-   Fertige Adminoberflächen
-   CRUD und mehr
-   RailsAdmin
-   ActiveAdmin

!SLIDE

## will\_paginate

-   Pagination für Listenansichten o.ä.
-   Im Controller:

~~~~ {.brush: .ruby}
@posts = Post.paginate :page => params[:page]
~~~~

-   In der View

~~~~ {.brush: .ruby; .html-script: .true;}
<%= will_paginate @posts %>
~~~~

!SLIDE

## Weitere Plugins

-   authlogic und devise
-   cancan
-   userstamp
-   paper\_trail
-   cocoon und nested\_forms
-   Zusätzlich: Rack-Middleware

!SLIDE

## Plugins selber schreiben

-   rails plugin new <name\>
-   Verschiedene Varianten (kombinierbar):
    -   Reine Bibliothek
    -   Dynamisches Erweitern vorhandener Funktionalität
    -   Engine

!SLIDE

# Praxis

!SLIDE

## Plugins einsetzen

-   Zu einem Ticket soll es möglich sein, eine Datei hochzuladen
-   Übersichtsseiten mit Pagination
-   Optional: Einsatz weiterer Plugins

