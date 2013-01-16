!SLIDE

# Rails und JavaScript

!SLIDE

## JS in RoR

-   Früher enge Integration mit prototype
-   Seit Rails 3 lose Kopplung (Unobtrusive JavaScript) und jQuery als
    Default
-   CoffeeScript als "Empfehlung", um JS zu schreiben

!SLIDE

## AJAX

-   Asynchronous JavaScript And XML(HttpRequest)
-   Anfragen an den Server asynchron im Hintergrund
-   Gezielte Updates auf der Seite ohne diese komplett neu zu laden
-   Responsivere, dynamischere Anwendungen

!SLIDE

## AJAX-Links und Formulare

~~~~ {.brush: .ruby; .html-script: .true}
<%= form_for @comment, :remote => true do |f| %>
  ...
<% end %>

<%= link_to "Erledigt", mark_as_done_to_do_path, :remote => true %>
~~~~

!SLIDE

## AJAX-Anfrage im Controller

~~~~ {.brush: .ruby}
class ToDosController

  def mark_as_done
    if request.xhr?
      # ...
    end
    # oder
    respond_to do |format|
      format.js
    end
  end
end
~~~~

!SLIDE

## AJAX-Antwort

-   Um die Seite in Abhängigkeit von der Antwort zu aktualisieren sollte
    eine AJAX-Anfrage JavaScript zurückliefern
-   Dieses wird im Client sofort ausgeführt
-   "Vanilla" JavaScript kann mit ERb gerendert werden
-   Alternativ: CoffeeScript

!SLIDE

## CoffeeScript

-   Douglas Crockford "JavaScript: The Good Parts"
-   CoffeeScript kompiliert zu JavaScript
-   CoffeeScript arbeitet die guten Seiten von JavaScript heraus
-   CoffeeScript verbirgt JS-Schwächen und Kuriositäten
-   Anleihen auch bei Ruby
-   Gute JS-Kentnisse trotzdem empfehlenswert

!SLIDE

## Client-side MVC die Zukunft?

-   Trend geht in Richtung aufwändiger JS-Anwendungen im Browser
-   Backbone, Spine, Ember, Knockout usw.
-   Server tauscht nur noch JSON mit Browser aus
-   Vorteil: API inklusive
-   Rails gute Wahl, aber evtl. oversized

!SLIDE

# Praxis

!SLIDE

## Einsatz von AJAX

-   Ticket auf Knopfdruck erledigen
-   Tags/Labels auf Knopfdruck entfernen
-   Optional: Tags/Lables dynamisch erstellen

