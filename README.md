# Demo Rails app

Demonstration of:

* [Ruby][https://www.ruby-lang.org/en/) programming language

* [Ruby on Rails](https://rubyonrails.org) web framework

* [Postgres](https://www.postgresql.org) relational database

* [SQLite](https://www.sqlite.org/index.html) lightweight database


## Prepare

If you use the command `asdf` to version:

```sh
asdf global ruby latest
asdf global postgres latest
```

Have Ruby 3?

```sh
ruby --version
```

Have Rails 7?

```sh
gem list rails
```

Have Postgres 14?

```sh
postgres --version
```

Have SQLite 3?

```sh
sqlite3 --version
```


## New demo

Create a new demo:

```sh
rails new demo_rails_app --database=postgresql
cd demo_rails_app
git add -A && git commit -am "Run: rails new demo_rails_app --database=postgresql"
```

If you use `asdf` then set Ruby and Postgres

```sh
asdf local ruby latest
asdf local postgres latest
asdf install
git add -A && git commit -am "Add asdf local ruby latest and local postgres latest"
```

Verify you can use the Postgres control command `pg_ctl`:

```
pg_ctl stop
pg_ctl start
```


## Add gem dotenv-rails

Add to `Gemfile`:

```ruby
# Environment variables
gem "dotenv-rails", groups: [:development, :test]
```

```sh
bundle
git add -A && git commit -am "Add gem dotenv-rails"
```

Append to file `.gitgnore`:

```gitignore
# Ignore dotenv environment variable file.
/.env
```

```sh
git add -A && git commit -am "Add .gitignore rule for dotenv file /.env"
touch .env
```


## Set database access

For a demo app, we prefer to set database access via environment variables.

For a production app, we prefer to set database access via ephemeral controls such as via Hashicorp Vault.

For a naming convention, we prefer explicit database access environment variables:

* The database server name: POSTGRES

* The database server object: USER

* The role identifier: DEMO_RAILS_APP

* The role attribute: USERNAME or PASSWORD

* The word FOR and the runtime environment: DEVELOPMENT or TEST or PRODUCTION


For our password, we prefer to generate a strong random password, such as a strong random 32-digit number:

```sh
printf "%s\n" $(LC_ALL=C < /dev/urandom tr -dc '[:digit:]' | head -c32)
```

Edit file `.env` to set the database access enviornment variables and the generated passwords:

```sh
POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_DEVELOPMENT=demo_rails_app
POSTGRES_USER_DEMO_RAILS_APP_PASSWORD_FOR_DEVELOPMENT=11053635016261456248726591454979

POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_TEST=demo_rails_app
POSTGRES_USER_DEMO_RAILS_APP_PASSWORD_FOR_TEST=82138770260839801404701988766919

POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_PRODUCTION=demo_rails_app
POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_PRODUCTION=59637922222719767183438989864425
```

Create the database role and enter the password for development:

```sh
createuser --username=postgres --pwprompt --createdb demo_rails_app
```


## Prepare the database

Edit file `config/database.yml` to add each section's username and password:

```yaml
development:
  <<: *default
  database: demo_rails_app_development
  username: <%= ENV["POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_DEVELOPMENT"] %>
  password: <%= ENV["POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_DEVELOPMENT"] %>
  …

test:
  <<: *default
  database: demo_rails_app_test
  username: <%= ENV["POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_TEST"] %>
  password: <%= ENV["POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_TEST"] %>
  …

production:
  <<: *default
  database: demo_rails_app_production
  username: <%= ENV["POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_PRODUCTION"] %>
  password: <%= ENV["POSTGRES_USER_DEMO_RAILS_APP_USERNAME_FOR_PRODUCTION"] %>
  …
```

Run:

```sh
bin/rails db:prepare
```

Output should include:

```sh
Created database 'demo_rails_app_development'
Created database 'demo_rails_app_test'
```


## Launch

Launch the Rails Puma server:

```sh
bin/rails server
```

Output should include:

```sh
Puma starting in single mode...
* Listening on http://127.0.0.1:3000
```

Browse <http://127.0.0.1:3000> and you should see the Rails welcome page.

