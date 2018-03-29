# Agorex-io Backend
[Chat on Discord](https://discord.gg/GVuj6UY)

[Join us on Reddit](https://www.reddit.com/r/Agorex/)

This repository contains the old backend forked from [ForkDelta](https://github.com/forkdelta), and is an artifact from the migration of developers from ForkDelta to Agorex-io.

Until the cooperative makes a formal decision regarding a development roadmap, this repository will remain in Agorex-io's GitHub organization.

## About
The backend provides off-chain orderbook functionality and an API to get a filtered view of Ethereum blockchain events on an [EtherDelta-like contract](https://www.reddit.com/r/EtherDelta/comments/6kdiyl/smart_contract_overview/).

Best way to learn about a system is to read the source code. Start with a look at [docker-compose.yml](docker-compose.yml).

## API

For information and documentation on the API, look here: https://github.com/Agorex-io/BackEnd/tree/master/docs/api

## Developing

### Setting up a development environment
Requirements:
* Some sort of shell environment
* A reasonably recent version of Docker (>= 17.06, ideally 17.12)
* docker-compose (= 1.18)
* Basic familiarity with Docker keywords: image, container (https://docs.docker.com/get-started/#docker-concepts)

Setup:
1. Clone the repo (git clone https://github.com/Agorex-io/BackEnd.git)
2. Navigate to the root of the working copy, where the README file is.
3. Copy `default.env` file to `.env` in root.
4. Uncomment the `COMPOSE_FILE=` line in `.env` to enable mounting of working copy code into the containers.
4. Build a Docker image containing our backend code: `docker-compose build contract_observer`
5. Create the database and migrate it to the latest schema: `docker-compose run contract_observer alembic upgrade head`
6. Run the backend systems: `docker-compose up`. You can shut everything down with Ctrl+C at any time.

Tips:
* There are multiple containers running our backend code: `contract_observer`, `etherdelta_observer`, `websocket_server`.
* Running `docker-compose build <service-name>` for any of the above builds the same image.
* `docker-compose build contract_observer` builds an image, copying the code and installing Python libraries in our dependencies.
  You have to rebuild any time the dependencies change; however, in development, code in the working copy is mounted into the container,
  so it's enough to restart the container (with `docker-compose restart <service-name>`) to apply changes for a given service.
* You can inspect the list of currently running containers with `docker-compose ps`.
