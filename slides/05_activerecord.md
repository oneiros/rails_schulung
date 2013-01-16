!SLIDE

# ActiveRecord

!SLIDE

## ActiveRecord

-   Objektrelationer Mapper (ORM)
-   Treiber für diverse DBMS (sqlite, mysql, postgres eingebaut, weitere
    als Plugin)
-   Starke Automatisierung und Abstraktion, Einsatz von SQL aber
    jederzeit möglich
-   Referentielle Integrität und sämtliche Anwendungslogik in der
    Anwendung, nicht in der Datenbank

!SLIDE

## Migrations: Datentypen

  ----------- ------------
  :string     :text
  :integer    :float
  :decimal    :boolean
  :datetime   :timestamp
  :time       :date
  :binary     
  ----------- ------------

!SLIDE

## Migrations: Operationen

~~~~ {.brush: .ruby}
create_table :projects {|t| ; }
drop_table :projects
rename_table :projects, :projekte

add_column :projects, :comment, :text
rename_column :projects, :comment, :kommentar
change_column :projects, :kommentar, :string
remove_column :projects, :kommentar

add_index :posts, :project_id
remove_index :posts, :project_id
~~~~

!SLIDE

## Migrations: Operationen contd.

~~~~ {.brush: .ruby}
create_table :posts do |t|
  t.references :project
  t.index :project_id
  t.timestamps
end

execute("UPDATE projects SET name = 'test'")
~~~~

!SLIDE

## Migrations: Rollback ermöglichen

~~~~ {.brush: .ruby}
class TestMigration < ActiveRecord::Migration
  def change
    add_column :posts, :test, :string
  end
end

class TestMigration < ActiveRecord::Migration
  def up ; execute() ; end

  def down ; execute() ; end
end
~~~~

!SLIDE

## Migrations ausführen

-   Auf die aktuellste Version migrieren: rake db:migrate
-   Zu konkreter Version migrieren: rake db:migrate VERSION=xxx
-   Migration rückhängig machen: rake db:migrate:down VERSION=xxx
-   Migration einzeln ausführen: rake db:migrate:up VERSION=xxx
-   Runter und wieder hoch: rake db:migrate:redo

!SLIDE

## Beziehungen: has\_many/belongs\_to

![image](images/has_many.png)

!SLIDE

## has\_and\_belongs\_to\_many

![image](images/habtm.png)

!SLIDE

## has\_many :through

![image](images/has_many_through.png)

!SLIDE

## Polymorphic Associations

-   Manchmal steht der Typ einer Verknüpfung nicht a priori fest
-   Beispiel: ToDo hat Verantworlichen und verantwortlich sein kann
    sowohl ein Benutzer als auch eine Gruppe
-   belongs\_to :responsible, :polymorphic =\> true
-   has\_many :to\_dos, :as =\> :responsible
-   In der DB: responsible\_id und responsible\_type

!SLIDE

## Single Table Inheritance

-   Wenn ActiveRecord-Klassen vererbt werden, geht Rails davon aus, dass
    alle Objekte in der selben Tabelle gespeichert werden
-   Zusätzliche Spalte type für den Namen der Klasse
-   Macht Abfragen komplexer
-   Selten wirklich notwendig

!SLIDE

## Nested Attributes

~~~~ {.brush: .ruby}
class Post < ActiveRecord::Base
  has_many :comments

  accepts_nested_attributes_for :comments
end
~~~~

!SLIDE

## Eigene Validations

~~~~ {.brush: .ruby}
class Post < ActiveRecord::Base
  validate :subject_not_unknown

  protected

  def subject_not_unknown
    if self.subject =~ /unknown/i
      self.errors.add(:subject, "cannot be unknown.")
    end
  end
end
~~~~

!SLIDE

## Wann wird validiert?

-   Auf explizite Nachfrage: project.valid?
-   Vor dem Speichern: project.save
-   save und update\_attributes liefern true oder false zurück
-   Varianten mit ! werfen Exceptions

!SLIDE

## Callbacks

-   Eingreifen in den "Lebenszyklus" eines AR-Objekts
-   before\_validation(\_on\_create)
-   after\_validation(\_on\_create)
-   before\_save
-   after\_save
-   before\_create
-   after\_create

!SLIDE

## Schutz vor mass assignments

-   new, create und update\_attributes setzen/ändern beliebige Anzahl
    von Attributen auf einmal
-   Eingabe häufig aus übertragenen Parametern
-   Das macht **alle** Attribute änderbar, auch wenn sie nicht im
    Formular verwendet werden
-   Positivliste mit attr\_accessible
-   Negativliste mit attr\_protected

!SLIDE

# Praxis

!SLIDE

## Ticket-System verbessern

-   Tickets können mit Hilfe von Tags/Labels kategorisiert werden
-   Ein Ticket kann beliebig viele Tags zugeordnet bekommen
-   Beim Ändern des Ticketstatus soll automatisch ein Kommentar erstellt
    werden (Tipp: object.changes)
-   Validations, Fixtures, Tests

!SLIDE

## Komplexe Abfragen

~~~~ {.brush: .ruby}
Post.where(:subject => "test")
project.posts.where(:subject => "test")

Post.order("created_at ASC")

Project.joins(:posts).where(
  :"posts.subject" => "test"
)
~~~~

!SLIDE

## Komplexe Abfragen (Rails 2)

~~~~ {.brush: .ruby}
Post.find(:all, :conditions => {:title => "test"})

Post.find(:all, :order => "created_at ASC")
~~~~

!SLIDE

## Komplexe Abfragen 2

~~~~ {.brush: .ruby}
!SLIDE

# Sicheres Einbetten von Argumenten
Post.where(["subject = ?", "test"])

!SLIDE

# Eager Loading
Project.includes(:posts).all

!SLIDE

# SQL verwenden
Project.find_by_sql("SELECT * FROM projects")
~~~~

!SLIDE

## Scopes

~~~~ {.brush: .ruby}
class ToDo < ActiveRecord::Base
  belongs_to :user

  default_scope order(:created_at)
  scope :open, where(:done => false)
  scope :done, where(:done => false)
  scope :newer_than, lambda { |time|
    where(arel_table[:created_at].gt(time))
  }

end
~~~~

!SLIDE

## Seed Data

-   Initialer Datenbestand
-   Stammdaten aller Art, z.B. Benutzer, Rollen, Kategorien usw.
-   Datei db/seeds.rb
-   rake db:seeds

!SLIDE

## Transaktionen

~~~~ {.brush: .ruby}
ActiveRecord::Base.transaction do
  project = Project.create!(:title => "test")
  post = Post.create!(
    :subject => "Welcome", 
    :project => project
  )
  post.do_something
  post.save!
end

Post.transaction { do_something }
~~~~

!SLIDE

# Praxis

!SLIDE

## Scopes und Finder

-   Scope für offene/geschlossene Tickets
-   Ticketübersicht und Personenseite mit Statistiken anreichern

