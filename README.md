# container-nextcloud-aio

docker compose container setup for [nextcloud-aio](https://github.com/nextcloud/all-in-one).

## setup

0. requirements

   - docker
   - docker-compose
   - a running [traefik instance](https://github.com/jonas-merkle/container-traefik)

1. link nextcloud-aio.yml traefik config to traefik instance

    ```bash
    ln ./config/traefik/nextcloud-aio.yml <traefik-container-root-dir>/config/dynamic-config/nextcloud-aio.yml
    ```

2. add environment variables

    ```bash
    nano .env
    ```

    add the missing information for the environment variables

3. start container

    ```bash
    docker-compose up -d
    ````

4. first setup

    a) start domain verification
        open `https://admin.<nextcloud-url>` in browser and start

    b) add the nextcloud-aio-domaincheck container to the trafik-net

    ```bash
    docker network connect traefik-net nextcloud-aio-domaincheck
    ```

    c) modify `nextcloud-aio.yml`
        - add a `#` at the beginning of the following line: `- url: "http://nextcloud-aio-apache:11000"`
        - remove the '#' at the beginning of the next line.
        - save the file.

    d) run domain verification
        - enter the nextcloud-url in the form
        - click the check button

    e) modify `nextcloud-aio.yml`
        - undo the changes from step b).

    f) proceed with the setup

    g) add the nextcloud-aio-apache container to the trafik-net

    ```bash
    docker network connect traefik-net nextcloud-aio-apache
    ```

5. stop container

    ```bash
    docker-compose down
    ```
