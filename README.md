# Progressive Web App (React): Task List

![task-list-v1-update](https://user-images.githubusercontent.com/11150895/157128754-5ec44361-eb5b-4ef8-9c9f-4745bd6dc9f3.png)

Task List is a **Progressive Web App** built with React as part of my book [Implementing PWA with React](https://leanpub.com/pwawithreact). This is **version 1** of the application and **support** an update mechanism. It is an installable app, with the app shell cached (using cache first strategy), but requires the network to create tasks.

It requires to have it running two services I have wrote: [UserAuth](https://github.com/enriquemolinari/userauth) and [TaskList](https://github.com/enriquemolinari/tasklist). Both services are pretty easy to start, with minimal dependencies.

## Install and Start

- git clone https://github.com/enriquemolinari/react-pwa-tasklist-v1 react-pwa-tasklist-v1
- cd react-pwa-tasklist-v1
- npm install
- npm run build
- serve -s build (service worker is only enabled in production environment)

## Users

- guser/guser123
- juser/juser123

## Using localhost (for your PC)

The pwa app, and the two back-end services must be accessed through a **reverse proxy** to have the same-origin policy and the SameSite=strict cookie working. Below I have provided configuration files for [Kong](https://konghq.com/install/#kong-community) and [Nginx](https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source/) if you want to use any of those. Make sure the `.env` file is pointing to the correct reserse proxy port. Below is a sample using port 8000.

```
# for simple localhost
REACT_APP_URI_AUTH=http://localhost:8000/auth
REACT_APP_URI_TASK=http://localhost:8000/app

# for using with localtunnel
#REACT_APP_URI_AUTH=https://auth1.loca.lt
#REACT_APP_URI_TASK=https://task1.loca.lt
```

### Kong config

```
services:
- name: backend-auth
  url: http://localhost:1234
  routes:
  - name: backend-auth-route
    paths:
    - /auth
- name: backend-tasks
  url: http://localhost:1235
  routes:
  - name: backend-tasks-route
    paths:
    - /app
- name: frontend
  url: http://localhost:3000
  routes:
  - name: frontend-route
    paths:
    - /
```

### Nginx config

```
server {
  listen 8000;
  listen [::]:8000;

  location / {
         proxy_pass http://localhost:3000;
  }

  location /auth/ {
         proxy_pass http://localhost:1234/;
  }

  location /app/ {
         proxy_pass http://localhost:1235/;
  }
}
```

## Using LocalTunnel (for your mobile or PC)

By using [localtunnel](https://github.com/localtunnel/localtunnel) we are able to browse the PWA using httpS allowing us to test the application using a **mobile** device. We are also able to share the URL with anyone, as it is a public URL poiting to our local development PC. Check each LocalTunnel section on each service ([UserAuth](https://github.com/enriquemolinari/userauth) and [TaskList](https://github.com/enriquemolinari/tasklist)) to start them correctly.

### Install Localtunnel

`npm install -g localtunnel`

### Start the tunnels

You need to start a tunnel for each service. For the PWA app (subdomain must be web-emp):

`lt --port 3000 --subdomain web-epm`

For the [UserAuth](https://github.com/enriquemolinari/userauth) service:

`lt --port 1234 --subdomain auth1`

And for the [TaskList](https://github.com/enriquemolinari/tasklist) service:

`lt --port 1235 --subdomain task1`

After setting this up, to enable it, open a browser and navigate to each URL: https://web-epm.loca.lt, https://auth1.loca.lt and https://task1.loca.lt. You will have to read a friendly reminder and click a button to proceed.

Finally, make sure the `.env` file is pointing to the correct tunnel URLs for each service. Below is how they should be:

```
# for simple localhost
#REACT_APP_URI_AUTH=http://localhost:8000/auth
#REACT_APP_URI_TASK=http://localhost:8000/app

# for using with localtunnel
REACT_APP_URI_AUTH=https://auth1.loca.lt
REACT_APP_URI_TASK=https://task1.loca.lt
```

### Enjoy

Navigate to https://web-epm.loca.lt/

The book is completely **free** for my students (if you want to read it, just write to me).
