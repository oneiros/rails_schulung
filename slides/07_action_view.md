!SLIDE

# ActionView

!SLIDE

## Layouts

~~~~ruby
class PostsController < ApplicationController
  layout "my_layout"

  def index
    render :layout => "other_layout"
  end
end
~~~~

!SLIDE

## Partials

~~~~ruby
# Lange Form
render :partial => "form",
  :locals => {:f => form_builder}

# Kurze Form
render "form", :f => form_builder
~~~~

!SLIDE

## Helpers

-   Nützliche Helfer, die HTML generieren, werden mitgeliefert
-   Eigene können in Modulen in app/helpers/ implementiert werden
-   Ein Helper-Modul pro Controller
-   Zentraler Helper ApplicationHelper
-   Controller-Methoden können zu Helpern werden mit Hilfe von helper_method

!SLIDE

## Nützliche Helfer

~~~~ruby
link_to "Edit", edit_project_path(@project)
link_to "Delete", @post,
  :confirm => "Are you sure?"

javascript_include_tag "myjs"
stylesheet_link_tag "mycss"

cycle("even", "odd")
~~~~

!SLIDE

## Formulare

~~~~ruby
<%= form_for @post do |f| %>
  <%= f.text_field :subject %>
  <%= f.submit %>
<% end %>

<%= form_tag posts_path, :method => :post do %>
  <%= f.text_field_tag "post[subject]" %>
  <%= f.submit_tag "Submit" %>
<% end %>
~~~~

!SLIDE

## Beziehungen mit select

~~~~ruby
<%= form_for @to_do do |f| %>
  <%= f.collection_select :user_id, 
        User.all, :id, :name 
  %>
  ...
<% end %>
~~~~

!SLIDE

## Nested Attributes 2

~~~~ruby
<%= form_for @user do |f| %>
  ...
  <%= f.fields_for :phone_numbers do |cf| %>
    <%= cf.text_field :number %>
  <% end %>
<% end %>
~~~~

!SLIDE

## XML Templates mit Builder

~~~~ruby
xml.instruct!
xml.project do
  xml.name(@project.name)
end
~~~~

!SLIDE

## Weitere Templates

-   ERb kann alle textbasierten Formate rendern
-   Alternative Templatesysteme als Plugins:
    -   HAML (später)
    -   Liquid
    -   Mustache
    -   ...

!SLIDE

## Abschließend: Was gehört wohin?

-   Model: Business-Logik (Abfrage-Logik, Berechnungen, Umwandlungen
    usw.)
-   View: Präsentationsschicht mit wenig bis gar keiner Logik
-   Controller: Vermittelnder Code, so schlank wie möglich
-   Best practice: Skinny controller, fat model

!SLIDE

## Concerns: Abspecken (nicht nur für Models)

-   "Fette" Klassen mit Hilfe von Modules entzerren
-   Eine Aufgabe (Concern) pro Module
-   ActiveSupport::Concern als Starthilfe
-   Alternativ: Klassische OO-Methoden

!SLIDE

# Praxis

!SLIDE

## Bessere Templates

-   Übergreifende Navigation
-   Tickets bearbeiten: Verantwortliche Person auswählbar machen
-   Ticketansicht: Kommentar hinzufügen
-   Testen!

!SLIDE

## Benutzer-/Sessionmanagement

-   Benutzerverwaltung mit Login und Zugriffsschutz

