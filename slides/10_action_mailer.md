!SLIDE

# ActionMailer

!SLIDE

## Konfiguration

-   Üblicherweise in config/environemnts/
-   Delivery-Methods :smtp, :sendmail, :test, :file

~~~~ruby
config.action_mailer.delivery_method = :smtp
config.action_mailer.smtp_settings = {
  :address => "mail.example.com",
  :port => 25,
  :domain => "helo.mydomain.com",
  :authentication => :plain,
  :user_name => "test",
  :password => "secret"
}
~~~~

!SLIDE

## Mailer analog zu Controller

~~~~ruby
class Notifier < ActionMailer::Base
  default :from => "noreply@mydomain.com"

  def new_project(project)
    @project = project

    mail :to => "admin@mydomain.com"
  end
end
~~~~

!SLIDE

## Mails senden

~~~~ruby
class Project < ActiveRecord::Base
  after_create :notify_admin

  private

  def notify_admin
    Notifier.new_project(self).deliver
  end
end
~~~~

!SLIDE

## Mails empfangen

-   receive-Methode überschreiben
-   Kein Mailclient - Mail muss Rails-Applikation zugeführt werden
-   Mögliche Wege: MTA, procmail, fetchmail, eigenes Skript
-   mail gem
-   mailman gem

