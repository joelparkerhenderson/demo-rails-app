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

Run:

```sh
bundle
git add -A && git commit -am "Add gem dotenv-rails"
```
