# Docker Tasks 02

---

#### 01 Use https://github.com/dockersamples/example-voting-app.git to build setup (Hint: needs to do troubleshooting).

- Cloning repository to local host.
```bash
git clone https://github.com/dockersamples/example-voting-app.git
```

![0101](images/01-01.png)

<br>
- Building infrastructure with docker compose (with file with images only).       

```bash
docker compose -f docker-compose.images.yml up -d
docker ps              # Checking container started
docker network ls      # Checking new networks
```

![0102](images/01-02.png)

<br>
- Checking Vote app is running or not.

![0103](images/01-03.png)

<br>
- Checking voting results.

![0104](images/01-04.png)

<br>
- Shutting down infrastructure.

```bash
docker compose -f docker-compose.images.yml down
```

![0105](images/01-05.png)

<br>

---

#### 02  Create a Multi Container Infrastructure using following instructions

a. Linux Machine for pinging Google DNS
b. A WebServer of Apache on port 8081 using [vote.html at github  engineerbaz/DevOps-Bootcamp-2024}
c: Database of your choice 
d. Webserver of your image built locally printing current time/date and your name


