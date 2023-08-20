+++ 
date = 2023-08-20T22:21:23+01:00
title = "Adminer - See your Postgres database in a container"
+++

On the other day I was messing with my [MiniFlux](https://miniflux.app/) selfhosted instance, and I pressed the clear history option (don't do that if you want to keep all the unread articles).

I've learned 2 things with this action:
- First, I need a better backup routine. 
- And second, the entries table, keeps the entries with the status 'removed'. Fact that I learned after checking the database myself.

In my current setup, I have a server running docker. And a compose file for my Miniflux instalation:

```yaml
services:
  miniflux:
    image: miniflux/miniflux:latest
    ports:
      - "{miniflux_port}:8080"
    depends_on:
      db:
        condition: service_healthy
    environment:
      - DATABASE_URL=postgres://{postgres_user}:{postgres_password}@db/miniflux?sslmode=disable
      - BASE_URL={miniflux_base_url}
      - RUN_MIGRATIONS=1
      - CREATE_ADMIN=1
      - ADMIN_USERNAME={miniflux_admin_username}
      - ADMIN_PASSWORD={miniflux_admin_password}
  db:
    image: postgres:15
    environment:
      - POSTGRES_USER={postgres_user}
      - POSTGRES_PASSWORD={postgres_password}
    volumes:
      - {host_volume}:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "miniflux"]
      interval: 10s
      start_period: 30s
```

For me, the fastest solution I've found to see the postgres content was to add another service to the compose file. Here comes Adminer.

```yaml
services:
  ...
  adminer:
    image: adminer
    restart: always
    depends_on: 
      - db
    ports:
      - {adminer_port}:8080
  ...
```

[Adminer](https://www.adminer.org/) is a simple web ui to manage databases. 

I did my thing, changed some records and then I changed again my compose file to remove Adminer. Not that I don't like the tool, it's realy fast and easy to use. But I don't need to keep it running after fixing my issue. 
It's a container. I can instantiate it when I need it, and then, puff! There goes the bits.