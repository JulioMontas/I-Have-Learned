# Rails cheatsheet

### Creating a new rails app from scratch
- Create a new rails app (with a pg db)
```
rails new contacter_app --database=postgresql
```

- `cd` into the new app
- Create the database; this command searches the `database.yml` file for a database name & if it doesn't find one, it names your DB after your app name.
  - `rake db:create`
- Create tables with columns:
  - Formula: `rails generate model NameOfTable first_column_name second_column_name:data_type`
  - `rails generate model Product title post_at`
  - Table name is Capital, singular, ProperCase
  - The default data type is `string`, you can specify this way (email is string, age is integer):
    - `rails generate model Product email age:integer`
    - This command auto-creates migration & model files for you
  - To undo this:
    - `rails destroy model Product`
- Run migrations
  - `rake db:migrate`
- If you need to make changes to schema later:
  - Option 1: run `rake db:drop db:create db:migrate`
  - Option 2: run `rake db:reset` and `rake db:migrate`
  - `rake db:reset` = `rake db:drop db:create db:schema:load db:seed`
- Require any gems in gemfile
  - `pry-rails`, `awesome_print`
  - Then `bundle install`
  - Anytime you make changes to gemfile, `bundle install`
- Fill out Model files
  - Model files specify relationships and which attributes to validate
  - Model file names are always `snake_case_singular.rb` (`workshop_location.rb`)
  - Model names, however, are proper CamelCase (`WorkshopLocation`)
- Fill out seed file
  - In hash format
  - Run `rake db:seed`
  - Run `rails console` or `psql` to see DB in terminal
- Fill out `routes.rb`
  - Define methods here
  - Run `rake routes`
- Generate Controller files
  - Controller file names are always snake case & plural (`actors_controller.rb`) except for `application_controller.rb`
  - `rails generate controller products`
  - If you mess up: `rails destroy controller products`
- Fill out controller files with 5 actions:
- Create json View files - four main views for each entity - index, show, new, and edit
- Run your app from root of project `rails server` & go to `localhost:3000`

### When you pull someone else's rails app
- If this is someone else's project, make sure you run `bundle install`
  - This reads from the `gemfile` & installs the necessary gems.
  - This is like when you pull a `node` project from GH; you run `npm install`, which reads from their `package.json` to see which npm packages are necessary for this project.
- Create the DB, migrate your tables & seed the db. This reads from their `database.yml` file for a DB name (if none, it'll name the DB after the name of the app).
  - `rake db:create`
  - `rake db:migrate`
  - `rake db:seed`
- From root dir of project, run `rails server`
