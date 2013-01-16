!SLIDE

# Asset Pipeline

!SLIDE

## In a Nutshell

-   Definierter Ort und Präprozessor für Assets
-   Assets = Bilder, JavaScript und Stylesheets
-   Preprocessing mit Sprockets
-   SCSS und ERb für CSS
-   CoffeeScript und ERb für JavaScript
-   Zusammenfassen von vielen Dateien in eine

!SLIDE

## 3rd Party Assets

-   In vendor/assets/
-   Plugins können Assets enthalten

!SLIDE

## Abhängigkeiten definieren

~~~~javascript
//= require jquery
//= require jquery_ujs
//= require_tree .
~~~~

!SLIDE

## Precompilation für die Produktion

-   Rake Task: rake assets:precompile
-   Dateien landen in public/
-   Achtung: Dateien konfigurieren in config/environments/production.rb

!SLIDE

## SCSS

~~~~css
$mycolor = #ff34a2;

.flash {
  border: 1px solid #000;
  p { background: #f00; }
}

.error {
  @extend .flash;
}
~~~~
