# container-nextcloud-aio

A Docker Compose container setup for [Nextcloud AIO](https://github.com/nextcloud/all-in-one).

## Table of contents

- [container-nextcloud-aio](#container-nextcloud-aio)
  - [Table of contents](#table-of-contents)
  - [Setup](#setup)
  - [First startup](#first-startup)

## Setup

0. Requirements

   - Docker
   - Docker Compose
   - A running [Traefik instance](https://github.com/jonas-merkle/container-traefik)

1. Link nextcloud-aio.yml Traefik config to Traefik instance

    ```bash
    ln ./config/traefik/nextcloud-aio.yml <traefik-container-root-dir>/config/dynamic-config/nextcloud-aio.yml
    ```

2. Add environment variables

    Add the missing information for the environment variables:

    ```bash
    nano .env
    ```
    
    Mark the `.env` file so it's not tracked by git:

    ```bash
    git update-index --assume-unchanged .env
    ```

3. Start container

    ```bash
    docker-compose up -d
    ````

4. Stop container

    ```bash
    docker-compose down
    ```

## First startup

1. Start domain verification:
   Open `https://admin.<nextcloud-url>` in browser and start the process.

2. Add the nextcloud-aio-domaincheck container to the `trafik-net`:

    ```bash
    docker network connect traefik-net nextcloud-aio-domaincheck
    ```

3. Modify the `nextcloud-aio.yml` file:
   - Add a `#` at the beginning of the following line: `- url: "http://nextcloud-aio-apache:11000"`
   - Remove the '#' at the beginning of the next line.
   - Save the file.

4. Run domain verification
   - Enter the nextcloud-url in the form.
   - Click the check button.

5. Modify the `nextcloud-aio.yml` file again:
   - undo the changes from step 3.

6. Proceed with the setup.

7. Add the nextcloud-aio-apache container to the `trafik-net`.

    ```bash
    docker network connect traefik-net nextcloud-aio-apache
    ```
