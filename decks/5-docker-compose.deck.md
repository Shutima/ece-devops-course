---
title: "Docker Part 2: Docker Compose"
---

# DevOps course overview

- DevOps culture
- Source Control Management (SCM) - Git
- Continuous Testing
- Continuous Integration & Continuous Delivery (CI/CD) - Travis CI
- **Containers - Docker**
- Containers orchestration - Kubernetes
- Infrastructure as code (IaC) - Ansible
- Cloud native architecture

gatsby-slide

# Introduction to Docker Compose

A tool for running multi-container applications.
The easiest way to get started with services, such as having a database (MongoDB, PostgreSQl, ...)

Overview - https://docs.docker.com/compose/

gatsby-slide

# Introduction to Network

Overview - https://docs.docker.com/network/

Serve our website and gitlab behind a reverse proxy.

gatsby-slide

# Your work

## 1. Start the private servers

Start the website and GitLab containers

```
cd assets/docker/gitlab
docker-compose up
cd assets/docker/website
docker-compose up
```

Test the containers:

```
# Website
curl http://localhost:11080/
curl http://localhost:11080/ece -L
# GitLab
curl http://localhost:10080/
```

gatsby-slide

# Your work

## 2. Get the environment ready

Create an internal network shared between all containers:

```
docker network create private
docker network inspect private
```

Note the last few lines of each `docker-compose.yml`, we instruct Docker to use this network:

```
networks:
  default:
    external:
      name: private
```

**For Linux or Mac:**

```
sudo bash -c "echo '127.0.0.1    ece' >> /etc/hosts"
sudo bash -c "echo '127.0.0.1    www.ece' >> /etc/hosts"
sudo bash -c "echo '127.0.0.1    gitlab.ece' >> /etc/hosts"
```

**For Windows:**

1. Open the file `c:\\Windows\System32\drivers\etc\hosts` in any editor as administrator.

2. Append the strings:

```
127.0.0.1    ece
127.0.0.1    www.ece
127.0.0.1    gitlab.ece
```

Make sure none listen on port 80.

gatsby-slide

# Your work

## 3. Use Caddy

Start the proxy:

```
cd assets/docker/proxy_caddy
docker-compose up
```

And check the connections:

```
curl http://www.ece
curl http://gitlab.ece -L
```

gatsby-slide

# Your work

## 4. Use Nginx

Start the proxy:

```
cd assets/docker/proxy_nginx
docker-compose up
```

And check the connections:

```
curl http://www.ece
curl http://gitlab.ece -L
```

# Your work

## 5. Create the `docker-compose.yml` file for your project "userapi"

Inspiration - https://docs.docker.com/compose/

1. Use ports
2. Use volumes
3. Use the same custom network `private`
4. Define a local domain name (configure your `hosts` file), for example `userapi.ece`

Test all your micro services:

- GitLab - http://gitlab.ece
- Static website - http://www.ece
- Your app - http://userapi.ece
