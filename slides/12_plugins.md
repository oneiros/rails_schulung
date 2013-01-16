!SLIDE

# Plugins

!SLIDE

## Rails erweitern

-   gems vs. plugins
-   [ruby-toolbox.com](http://www.ruby-toolbox.com) 
-   Aber auch: [github.com](http://www.github.com)
-   [rubygems.org](http://rubygems.org)

!SLIDE

## Paperclip

-   Handling von Datei-Uploads
-   Dateiablage im Dateisystem oder bei Amazon's S3
-   Automatisches Skalieren von Bildern, Erstellen von Thumbnails mit Hilfe von imagemagick
-   gem 'paperclip'

!SLIDE

## Paperclip Beispiel: Model

~~~~ruby
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

## simple_form

-   Smarter Ersatz für eingebaute Form-Helper
-   Sinnvolle defaults
-   Generiertes Markup stark konfigurierbar
-   gem 'simple_form'

!SLIDE

## simple_form Beispiel

~~~~ruby
<%= simple_form_for @to_do do |f| %>
  <%= f.input :title %>
  <%= f.input :done %>
  <%= f.button :submit %>
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

~~~~ruby
@posts = Post.paginate :page => params[:page]
~~~~

-   In der View

~~~~ruby
<%= will_paginate @posts %>
~~~~

!SLIDE

## Weitere Plugins

-   devise und omniauth
-   cancan
-   userstamp
-   simple-navigation
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

