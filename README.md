# cheat_sheet_rails_console

## Rails DBs
### Generate Model
must be **PascalCase**, **singular**
```
 rails g model User
```
*Create migration*
### Migrations
tables must be **snake_case**, **plural** of Model
In terminal
```
$ rails generate migration NomDeTaMigration
```
Example of migration
```
def change
  create_table :users do |t|
    t.string :name
    t.boolean :is_admin
    t.timestamps
  end
end
```
Run migration in terminal
```
$ rails db:migrate
```

Check the up & down status
```
$ rails db:migrate:status
```
## CRUD
### Create
```
> martin = User.new(first_name: "Martin", email: "martion@gmail.com", is_admin: true)
> martin.save
```
or
```
> martin = User.create(first_name: "Martin", email: "martion@gmail.com", is_admin: true)
```

### Read
```
> u = User.find(3)
> david = User.find_by(first_name: 'David')
```
Recherches plus complexes:
```
> users = User.where(first_name: 'David', is_admin: true).order(created_at: :desc)
```
### Update
```
> user_1 = User.find(1)
> user_1.first_name = "a new name"
> user_1.save
```
or
```
> user_1 = User.find(1)
> user_1.update(first_name: "a new name")
```

### Delete
```
> user_1 = User.find(1)
> user_1.destroy
```

### Quelques méthodes

+ `#all` : te ressort tous les éléments de ton model (array).
+ `#new` : crée une nouvelle instance de ton model.
+ `#save` : enregistre cette instance.
+ `#create` : crée et enregistre une instance de model.
+ `#update(attribute: value)` : met à jour l'attribute de cet objet / entrée.
+ `#last` : te retourne la dernière entrée créée dans le model.
+ `#first` : pareil mais pour la première.
+ `#find(x) : te retourne l'entrée BDD dont l'`id` vaut `x` (integer)._
+ `#find_by(attribute: value)` : te retourne le premier élément qui a value à l'attribute.
+ `#where(attribute: value)` : te retourne tous les éléments (dans un array) qui ont `value` à l'`attribute`.
+ `#destroy` : supprime de la BDD l'entrée en question.

### Seed
In `db/seeds.rb`, same commands as in rails console
```
User.create(first_name: "jean", email:"jean@jean.jean")
User.create(first_name: "paul", email:"paul@paul.paul")
puts "Deux utilisateurs ont été créés"
```
In terminal
```
$ rails db:seed
```
loops 
```
100.times do |index|
  User.create(first_name: "Nom#{index}", email: "email#{index}@example.com")
end
```
Faker
```
require 'faker'
100.times do
  user = User.create!(first_name: Faker::Name.first_name, email: Faker::Internet.email)
end
```
### To add relation 1-N 
1. In migration
To create a link in the mother table and include the child's id, user: 

```
t.belongs_to :user, index: true
```
OR
```
add_reference :articles, :user, foreign_key: true
```
*note: articles is a table = plural, user is unique = singular* 

2. In the models
a. For the N
```
class Child < ApplicationRecord
  belongs_to :parent
end
```
b. For the 1
```
class Parent < ApplicationRecord
  has_many :childs
end
```
c. Testing procedure (in console)
```
> a = Article.new #=> Un article pas encore sauvegardé
> a.user #=> il doit te renvoyer nil. S'il te renvoie une erreur, le lien est mal fait !
> u = User.first #=> un superbe user. S'il n'y en a pas encore, crées-en un.
> a.user = u #=> A nouveau, s'il te renvoie une erreur, le lien est mal fait !
> a.save #=> COMMIT. S'il te renvoie une erreur, tu as mal suivi ce tuto
> Article.last.user.articles #=> Récupère les articles du user lié au dernier article créé en base (celui que tu viens de créer) #=> Devrait te renvoyer un array qui contient l'article que tu viens de créer. Sinon c'est que le lien est mal fait.
```
### To add relation N-N 

1. In the models
a. The join table
```
class Appointment < ApplicationRecord
  belongs_to :doctor
  belongs_to :patient
end
```
b. the Doctor/Patiens
```
class Doctor < ApplicationRecord #or Patient
  has_many :appointments
  has_many :patients, through: :appointments # ordoctors
end
```

### For testing

To drop the tables and have the id starting back at 1.

```
rails db:reset
```

### Links

https://api.rubyonrails.org/
https://guides.rubyonrails.org/v7.1/
