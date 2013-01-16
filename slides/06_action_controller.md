!SLIDE

# ActionController

!SLIDE

## Resource Routing

~~~~ruby
resource :session
resources :projects do
  resources :posts
end

resources :to_dos do
  member { put :mark_done }
  collection { put :mark_all_done }
end
~~~~

!SLIDE

## Named Routes

~~~~ruby
match "/show_post/:id" => "posts#show",
  :as => "show_post"

# show_post_path(:id => 5) => /show_post/5

~~~~

!SLIDE

## Ausgaben: Render Und Redirect

-   Standardmäßig wird das Template mit dem Namen der Action gerendert
-   Durch expliziten Aufruf von render kann dieses Verhalten geändet
    werden
-   Alterniv: redirect\_to löst einen Browser-Redirect aus
-   Achtung: render und redirect\_to brechen die Bearbeitung nicht ab - explizites return bei komplexen Verzweigungen!

!SLIDE

## Was kann man rendern?

~~~~ruby
render :action => "new"
render :template => "projects/new"
render :partial => "form"
render :file => "/home/dave/test.erb"
render :text => "hello world"
render :nothing => true
render :json => project
render :xml => project
~~~~

!SLIDE

## Request und Response

-   request enthält alle wichtigen Informationen zur Anfrage, z.B. URL und Header
-   headers-Hash ermöglicht setzen von zusätzlichen Headern in der Antwort

!SLIDE

## Verschiedene Ausgabeformate

-   Unterscheidung anhand Endung oder Header mit Hilfe von respond\_to:

~~~~ruby
respond_to do |format|
  format.html
  format.xml { render :xml => @post }
end
~~~~

-   Bekannte Formate: html, xml, text, js, yaml, rss, atom, ics
-   Weitere Formate können registriert werden

!SLIDE

## Session Handling

-   session-Hash ermöglicht Setzen und Auslesen von Werten
-   fast alle Objekte können in der Session abgelegt werden
-   best practice: möglichst wenig in der Session ablegen
-   Verschiedene Session Storages - Default: CookieStorage
-   reset\_session zum Zurücksetzen

!SLIDE

## Filter

-   Zentraler Ort für mehrfach benötigte Funktionalität (DRY)
-   3 Filterarten:
    -   before\_filter
    -   after\_filter
    -   around\_filter
-   Lokal im Controller definieren, oder global für alle in
    ApplicationController

!SLIDE

## Beispiel-Filter

~~~~ruby
class PostsController < ApplicationController

  before_filter :load_project

  # ...

  def load_project
    @project = Project.find(params[:project_id])
  end
end
~~~~

!SLIDE

## Flash

-   Hash für Rückmeldungen an den Benutzer
-   Gespeichert in der Session, so dass sie bei redirect\_to erhalten bleiben
-   Bsp: redirect\_to @project, :notice =\> "Successfully created."
-   Alternativ: Direkter Zugriff auf den Hash
-   Beliebige Schlüssel um verschiedene Arten von Nachrichten zu unterscheiden

!SLIDE

## The Bare Metal

-   ActionController ist sehr modular aufgebaut
-   Mit allen Features ist er recht groß und (vergleichsweise) behäbig
-   Manchmal benötigt man nicht alle Features, aber mehr Performance
-   Controller von ActionController::Metal ableiten, Funktionalität gezielt includen

!SLIDE

# Praxis

!SLIDE

## Dashboard

-   Neuen Controller erstellen
-   Dashboard soll analog zur Liste aller Tickets nur die offenen Tickets anzeigen.
-   Dashboard soll Startseite werden
-   Testen!

