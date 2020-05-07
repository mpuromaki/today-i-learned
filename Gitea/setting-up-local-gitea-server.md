# Setting up local gitea server

Everyone already should know [gitea], beautiful github-like
server which is really simple to self-host. 

Here is the instructions on how to set up local gitea 
server on docker. I have personally tested this on Windows 10
version of the [Docker Desktop]. Installation of Docker was 
really straight forward and it did not need any configuration
after install. Docker-compose is built-in to the Docker Desktop.

:warning: Think about port-forwarding/firewall. You really might
not want to show this server to whole internet.

### 1. Create folder Gitea
### 2. Create file docker-compose.yml

Change the settings to your liking. These are hard to change after
the instance is created..

```
version: "2"

networks:
  gitea:
    external: false
    
volumes:
  gitea:
    driver: local

services:
  server:
    image: gitea/gitea:latest
    environment:
      - APP_NAME="Local Version Control"
      - USER_UID=1000
      - USER_GID=1000
      - DOMAIN=localhost
      - ROOT_URL="http://localhost:37764"
      - SSH_DOMAIN=localhost
      - SSH_PORT=45186
      - SSH_LISTEN_PORT=2222
      - USE_COMPAT_SSH_URI=true
      - DISABLE_REGISTRATION=true
      - REQUIRE_SIGNIN_VIEW=true
    restart: always
    networks:
      - gitea
    volumes:
      - gitea:/data
    ports:
      - "45186:2222"
      - "37764:3000"
```

### 3. In Gitea folder run the following command

```
docker-compose up -d
```
-d option will make the docker-compose run in detached mode, in the background.
Doing this allows you to close the cmd window after running the command.

This will show up as "Gitea" on your Docker Desktop dashboard.

### 4. Connect to http://localhost:37764/

This is your Gitea server instance. Bookmark this page to allow easier
navigation in future.

### 5. Click any link on the page, you will be automatically forwarded to setup
page.

Most of the settings should be fine at this point. I suggest you keep the
database as Sqlite3 for easy setup.

### 6. Under server and third-party settings select following:

- Enable local mode
- Disable Gravatar
- Disable self-registration
- Require sign-in to view pages
- Allow creation of organisations by default
- Enable time-tracking by default

### 7. Under Administration Account Settings

Set-up your admin account details.

### 8. Install Gitea. Do not rush, it will take a moment.
### 9. Log into your admin account
### 10. Open Settings

Under SSH/GPG keys add your SHH key (public part of it) to your
admin account.

### 11. Enjoy your local version control!

---

## Deleting everything to change docker-compose settings

You need to remove the old gitea instance for these changes to work.

### 1. back-up your version controlled files
### 2. Run following commands.

:warning: These will really delete everything on that gitea instance :warning:

```
docker-compose down
docker-compose rm -v
docker volume rm gitea
```
:warning: These will really delete everything on that gitea instance :warning:

### 3. Follow the start of this file to reinstall everything.


[gitea]:  https://gitea.io/en-us/
[Docker Desktop]: https://hub.docker.com/editions/community/docker-ce-desktop-windows
