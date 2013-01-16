!SLIDE

# Einführung in Ruby

!SLIDE

## Installation

1.  RVM - The Ruby Version Manager ([rvm.io](http://rvm.io)) oder alternativ rbenv/ruby-build
2.  Ruby 1.9.3
3.  Rails 3.2.11

!SLIDE

## Einführung in Ruby

-   Enwickelt von Yukihiro "Matz" Matsumoto
-   1995 erstmals vorgestellt
-   Interpretierte, objektorientierte Sprache
-   Anleihen bei Perl, LISP und Smalltalk

!SLIDE

## Hello World

~~~~ruby
#!/usr/bin/env ruby

puts "Hello World"
~~~~

!SLIDE

## Skripte schreiben

-   Textdateien
-   Shebang line
-   Dateirechte

!SLIDE

## IDEs und Editoren

-   Textmate
-   Netbeans
-   Aptana
-   Rubymine
-   Vim
-   ... oder jeder andere Editor

!SLIDE

## irb

-   interaktive Ruby-Shell (REPL)
-   (fast) beliebiger Ruby-Code kann interaktiv ausprobiert werden
-   Rückgabewert wird sofort angezeigt

!SLIDE

## Anweisungen

-   Eine pro Zeile

~~~~ruby
puts "eins"
puts "zwei"
puts "drei"
~~~~

-   Oder getrennt durch Semikolon

~~~~ruby
puts "eins" ; puts "zwei" ; puts "drei"
~~~~

-   Kommentare mit \# einleiten

!SLIDE

## Variablen und Ausdrücke

~~~~ruby
a = 3 # 3
a = b = 4 # 4
a, b = 5, 6 # [5, 6]

local_variable = 1
CONSTANT = "hallo"
@instance_variable = 5
@@class_variable = "test"
$global_variable = "meh"
~~~~

!SLIDE

## Methoden

-   Objekte haben Methoden

~~~~ruby
5.to_s # "5"
"test".length # 4
~~~~

-   Methoden definieren

~~~~ruby
def my_method(param1)
  puts param1.to_s
end
~~~~

-   ? und !
-   Klammern, oder nicht klammern?

!SLIDE

## Datentypen

* Zahlen
 * Fixnum: 5
 * Float: 5.0
* String: "hello"
* Range: 1..20
* Boolean: true, false
* Symbol: :test

!SLIDE

## Arrays

-   Sortierte Liste von Werten

~~~~ruby
a = [1, 2, 5, 6]
a[0] # 1
a = ["hello", 1, 3.8, :name]
a.push(2) ; a.pop ; a.first ; a.last
a.select {|e| e > 2}
a.map {|e| e * 2 }
~~~~

!SLIDE

## Hashes

*   Schlüssel/Wert-Paare

~~~~ruby
h = {
  :name => "Otto", 
  :strasse => "Musterstr. 22", 
  :ort => "Musterstadt"
}
h[:name] # "Otto"
~~~~

* Alternative Syntax seit Ruby 1.9

!SLIDE

## Operatoren

~~~~ruby
5 + 1 # 6
5 / 2 # 2
5.0 / 2 # 2.5
"Hello" + " World" # "Hello World"
true and false # false
true || false # true
[1, 2] << 3 # [1, 2, 3]
~~~~

!SLIDE

## Fallentscheidungen 1

~~~~ruby
if a > 0
  puts "positiv"
elsif a < 0
  puts "negativ"
else
  puts "genau null"
end
puts "positiv" unless a <= 0
(a > 0) ? "positiv" : "nicht positiv"
~~~~

!SLIDE

## Fallentscheidungen 2

~~~~ruby
case a
when 0
  puts "null"
when 1, 3, 5, 7, 9
  puts "ungerade"
when 2, 4, 6, 8
  puts "gerade"
else
  puts "zu groß!"
end
~~~~

!SLIDE

## Schleifen

-   Im Wesentlichen nur while:

~~~~ruby
while (a > 0)
  a -= 1
  puts "Countdown: #{a}"
end
~~~~

-   Steuerung mit next, break und redo

!SLIDE

## Code Blocks

~~~~ruby
5.times do
  puts "hello"
  puts "world"
end
5.times { puts "hello world" }
5.times { |i| puts i }
~~~~

!SLIDE

## Iteratoren

~~~~ruby
a = [1, 2, 3]
a.each {|e| puts e}
for element in a do
  puts element
end
h = {:eins => 1, :zwei => 2, :drei => 3}
h.each {|key, value| puts "#{key}: #{value}"
~~~~

!SLIDE

## Reguläre Ausdrücke

~~~~ruby
puts "found" if a =~ /[Hh]ello [Ww]orld/
r = /^\s*Hello World$/
puts "found" if a =~ r
~~~~

!SLIDE

## Klassen und Objekte

~~~~ruby
class Test
end

t = Test.new
~~~~

!SLIDE

## Methoden

~~~~ruby
class Test
  def self.hello
    puts "hello"
  end

  def initialize(name)
    @name = name
  end

  def hello(name)
    puts "hello #{name}"
  end
end
~~~~

!SLIDE

## Defaultparameter

~~~~ruby
def greet(name, greeting = "hello")
  "#{greeting} #{name}"
end

greet("Otto") # "hello Otto"
greet("Otto", "hallo") # "hallo Otto"
~~~~

!SLIDE

## Beliebige Anzahl Parameter

~~~~ruby
def greet_all(*args)
  args.map do |arg|
    greet(arg)
  end
end

greet_all("Otto", "Peter", "Karl") 

# ["hello Otto", "hello Peter", "hello Karl"]
~~~~

!SLIDE

## Variablen die zweite

-   @test ist eine Instanzenvariable
-   @@test ist eine Klassenvariable
-   Nicht initialisierte Instanzenvariablen == nil

!SLIDE

## Zugriff auf Instanzenvariablen

-   Instanzenvariablen repräsentieren den internen Zustand eines Objekts
-   Direkter Zugriff von außen nicht möglich
-   Getter/Setter

~~~~ruby
class Test
  def test
    @test
  end
  def test=(new_test)
    @test = new_test
  end
end
~~~~

!SLIDE

## Kurzform

~~~~ruby
class Test
  attr_accessor :test
  attr_reader :read_only
  attr_writer :write_only
end
~~~~

!SLIDE

## Vererbung

~~~~ruby
class Rechteck
  def initialize(width, height)
    @width = width ; @height = height
  end
end

class Quadrat < Rechteck
  def initialize(width)
    super(width, width)
  end
end
~~~~

!SLIDE

## Zugriffsschutz

~~~~ruby
class Test
  def public_method ; end
  
  protected

  def somewhat_internal_method ; end

  private

  def internal_method ; end
end
~~~~

!SLIDE

## Exceptions

~~~~ruby
def erroneous
  raise "Oops"
end

begin
  erroneous
rescue RuntimeError
  puts "caught!"
ensure
  puts "safe!"
end
~~~~

!SLIDE

## Module 1: Namespaces

~~~~ruby
module NameSpace
  CONST = "hello"
  class Test
  end
end

NameSpace::CONST # "hello"
t = NameSpace::Test.new
~~~~

!SLIDE

## Module 2: Mixins

~~~~ruby
module Mix
  def greeting ; "hello world" ; end
end
class Test
  include Mix
end

t = Test.new
t.greeting # "hello world"
~~~~

!SLIDE

## Rubygems

-   Paketverwaltung für Ruby-Programme und -Bibliotheken
-   CLI ähnlich apt-get, yum etc.
-   gem install <paketname\>
-   [www.rubygems.org](http://www.rubygems.org)
-   require

!SLIDE

## Bundler

-   bundler managed gem-Abhängigkeiten
-   gem install bundler
-   Gemfile - Abhängigkeiten definieren
-   bundle - Abhängigkeiten installieren
-   Gemfile.lock - Exakte Abhängigkeiten
-   require "bundler/setup"

!SLIDE

## Rake

-   Ruby-Variante des bekannten Make-Tools
-   Task-Automatisierung inkl. Abhängigkeiten
-   gem install rake
-   Rakefile
-   Ruby-Code
-   rake -T zum Auflisten aller Tasks

!SLIDE

## Ruby-Ressourcen

-   why's poignant guide
-   "Pickaxe book"
-   The Ruby Programming Language - David Flanagan und Yukihiro Matsumoto
-   [www.ruby-lang.org](http://www.ruby-lang.org)
-   [www.ruby-doc.org](http://www.ruby-lang.org)
-   und natürlich: üben, üben, üben

