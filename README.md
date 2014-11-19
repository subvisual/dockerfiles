# Base Docker images

A set of Docker images to unify our development setup and deployment process.

# Development images

All images prefixed with `dev-` are intended for development purposes only. Do
not rely on these images for production environments, as they are not secure.

These are intended to be used with [fig](http://www.fig.sh/).

* [Ruby 2.1.5](dev/ruby-2.1.5)
* [Postgres 9.3](dev/postgres-9.3)

## Usage example

See [this example](test) for a real-life example of the explanation given in
this section.

In this example, we're development a Rails app with a PostgreSQL database.
For this the images `groupbuddies/dev-ruby:2.1.5` and
`groupbuddies/dev-postgres:9.3` will be used.

    web:
      image: groupbuddies/dev-ruby:2.1.5
      volumes:
        - .:/usr/src/app
      ports:
        - 3000:3000
      links:
        - db
    db:
      image: groupbuddies/dev-postgres:9.3
      volumes:
        - "~/.docker-volumes/APP_NAME/db/:/var/lib/postgresql/data/"
      expose:
        - 5432

Your app should use `foreman` to manage it's processes, so your `Procfile`
could look like the following (assuming the Rails app is served with [puma](https://github.com/puma/puma)):

    web: PORT=3000 bundle exec puma -C config/puma.rb

`PORT=3000` is required to make sure puma uses the same port that's
specified in `fig.yml`. Otherwise it would just default to the port given by
foreman.
