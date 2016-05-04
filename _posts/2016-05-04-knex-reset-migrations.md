---
layout: post
title: "Reset Migrations With Knex"
date: 2016-05-04
---

[Knex](http://knexjs.org/) is a Node.js SQL query builder that supports multiple
database implementations. It also supports [migrations](http://knexjs.org/#Migrations)
which are essential to any serious project using a relational db.

During local development, when running tests, and particuarly when testing
migrations, I often wanted to reset the db and rerun all migrations. I was
shocked to find that Knex migrations doesn't have a reset method.

I first tried to follow some advice and write a bash script. After a coule of
hours trying different methods of running postgres commands from the CLI and
battling database user permissions (both for authentication and db access rights)
I ended up with this:

{% highlight bash %}
#!/bin/bash

# Delete and recreate the database
psql -U postgres -h 127.0.0.1 -c 'DROP DATABASE IF EXISTS my_database;'
psql -U postgres -h 127.0.0.1 -c 'CREATE DATABASE my_database;'
psql -U postgres -h 127.0.0.1 -c 'GRANT CONNECT ON DATABASE my_database TO my_db_user;'

# Run migrations to generate the schema
knex migrate:latest
{% endhighlight %}

It did finally work, but I wasn't happy with it. It was very specific to my
environment. Plus I don't like including bash scripts in Node projects unless
there's absolutely no other sane method.

After reading more about Knex migrations and realizing that the long promised
[arctic ](https://github.com/knex/arctic) migrations API wasn't coming, I looked
at the current [Migrate](https://github.com/tgriesser/knex/blob/8a303f1a964acc98205575b1fcb594f5d6b0ea90/test/integration/migrate/index.js)
source to find that the API was very neat (perhaps it's improved in the past
year). I quickly found a very simple solution.

{% highlight js %}
var migrate = knex.migrate;

// Force unlock in case of a bad state
migrate.forceFreeMigrationsLock()

// Get completed migrations
.then(function(){
  return migrate._listCompleted();
})

// Rollback migrations
.then(function(completedMigrations){
  return migrate._waterfallBatch(0, completedMigrations, 'down');
})

// Migrate to the latest
.then(function(){
  return migrate.latest();
});
{% endhighlight %}