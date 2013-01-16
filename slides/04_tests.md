!SLIDE

# Softwaretests in Rails

!SLIDE

## Softwaretests

-   Codeverifikation praktisch unmöglich
-   Ziel daher: Möglichst gutes automatisiertes Testen mit
    reproduzierbaren Ergebnissen
-   Test first: Test als Spezifikation
-   Verträgt sich besonders gut mit OO
-   Test Suite mit einzelnen Test Cases
-   Assertions formulieren Annahmen
-   Fest eingebaut in Rails

!SLIDE

## Test first

-   Funktionlität zunächst genau planen
-   Test formulieren
-   Test legt Schnittstellen und gewünschtes Verhalten fest
-   Dann erst implementieren
-   Ausführen des Tests als Bestätigung, dass alles wie gewünscht
    funktioniert

!SLIDE

## Test Driven Development (TDD)

![image](images/tdd.png)

!SLIDE

## 3 Arten von Tests in Rails

-   Testen von Models = Unit Tests
-   Testen einzelner Controller = Functional Tests
-   Controller-übergreifendes Testen = Integration Tests

!SLIDE

## Rubys Testframework

-   Assertions formulieren Annahmen, die überprüft werden sollen
-   Einfachste Annahme - Überprüfen auf Wahrheitswert: assert
    *expression*
-   Spezifischere Hilfsmethoden, wie z.B.
    -   assert\_equal
    -   assert\_match
    -   assert\_not\_nil
    -   assert\_raise

!SLIDE

## Testen in Rails

-   Alle Tests ausführen mit rake test
-   Nur bestimmte Tests ausführen:
    -   rake test:units
    -   rake test:functionals
    -   rake test:integration
-   Zuletzt geänderte Tests ausführen mit rake test:recent

!SLIDE

## Fixtures

-   Definierter Bestand an Testdaten
-   Definition in YAML-Dateien in test/fixtures/
-   Einfache Definition von Verknüpfungen
-   Dynamisch durch ERb
-   Werden vor jedem Test neu geladen
-   Einfache Zugriffsmethoden
-   Nicht unumstritten

!SLIDE

## Mocks und Stubs

* Ungewünschte Seiteneffekte bei der Ausführung von Tests verhindern
* Z.B. keine Anfragen an externe Dienste
* Geschwindigkeitsvorteil
* Entkopplung von Komponenten beim Testen (wichtig für TDD)
* Mocking-Bibliotheken wie z.B. mocha

!SLIDE

## Unit Tests

~~~~ruby
class PostTest < ActiveSupport::TestCase
  setup do
    @post = Post.new
  end

  test "should return correct string" do
    @post.subject = "test"
    assert_equal "Post: test", @post.to_s
  end
end
~~~~

!SLIDE

## Functional Tests

~~~~ruby
class PostsControllerTest < ActionController::TestCase

  test "should get index" do
    get :index
    assert_response :success
    assert_not_nil assigns(:posts)
  end

end
~~~~

!SLIDE

## Rails Assertions

~~~~ruby
assert_valid post

assert_response 200

assert_redirected_to posts_path

assert_template "posts/index.html.erb"

assert_difference "Post.count" do
  Post.create(:subject => "test")
end
# ...
~~~~

!SLIDE

## Integration Tests

~~~~ruby
class CreationTest < ActionDispatch::IntegrationTest
  test "can create a project and a post" do
    assert_difference "Project.count" do
      post "/projects", :project => {:title => "test"}
    end
    assert_difference "Post.count" do
      post "/posts", :post => {
        :subject => "test", 
        :project_id => assigns(:project).id
      }
    end
  end
end
~~~~

!SLIDE

## Code Coverage

-   Kann man Qualität von Tests messen?
-   (C0) Coverage beschreibt, wieviel Anwendungscode beim Testen
    durchlaufen wurde
-   Lücken bei der Coverage sind offensichtliche Schwächen der Test
    Suite
-   Perfekte Coverage bedeutet leider nicht, dass die Tests gut sind
-   gem 'simplecov', :require =\> false, :group =\> :test
-   In tests/test\_helper.rb

~~~~ruby
require 'simplecov'
SimpleCov.start 'rails'
~~~~

!SLIDE

## Continuous Integration

-   Konzept von Martin Fowler
-   Zentraler Testserver, um Integrationsprobleme zeitnah aufzudecken
-   Eine ganze Reihe Ruby-CI-Server
-   Platzhirsch Hudson/Jenkins kann natürlich auch Ruby/Rails
-   Travis CI für Open Source Projekte

!SLIDE

## Tipps

-   Test first / TDD ausprobieren
-   Testen lohnt bei großen / langlebigen Projekten fast immer
-   Minimum: Jede "Seite" einmal aufrufen
-   Wenn ein Bug entdeckt wird, immer erst Test schreiben, dann fixen
-   Factories statt Fixtures

!SLIDE

## Ruby Debugger

-   gem install debugger
-   Gemfile anpassen
-   Schlüsselwort debugger im Code
-   Server startem mit rails server --debugger
-   Alle gängigen Debugger-Features

!SLIDE

# Praxis

!SLIDE

## Ticketsystem testen

-   Personen erweitern: Anzahl offener/geschlossener Tickets berechnen
-   Personen und Tickets: Methode to\_s überschreiben
-   Fixtures für Personen und Tickets anlegen
-   Tests schreiben
-   Optional: SimpleCov integrieren

