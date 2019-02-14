## `FPTU dot Tech` Requirement Analysis & Technical Architect Document

![FPTU](https://i.imgur.com/Y9AIFBR.png)

## Requirements and Functions to be implemented:
- App must has a `user board` and `admin board`.
- App must be designed to help Confessions Facebook fan page managers easily manage the contents, automatic schedule and posting to Facebook.
- App must has a communication system for administrators to feedback about the user's confessions and postback their request.
- App must has list & history of all confession, has a clear visual or analyst board of total sent confession and which posted on Facebook.
- App must has a page to fetch all university related articles (crawl) for user to quick view, and also be able to comment on those articles.
- App must be able to run in both Desktop and Mobile web.

`Need to update more about business logic here`.

## Scalable Architecture: Microservices
- System Architect is based on scaling Microservices technologies.
- Everything in system is a container (Dockerized), and be able to run on a Kubernetes Cluster (Google Kubernetes Engine with Load Balancer).
- How it scale: GKE Cluster managed, the system will auto scale up or scale down the instances to match up with the traffic.
- **Backend** implements several services to handle all actions: User API, Confession API, DMS (create & excute query). Communication between internal services use **high performance gRPC** (http/2) with Protobuf.
- **Frontend** implements with Single Page Application, based `Universal Web App` and `Progressive Web App`. Implement a **Javascript SDK** separated to wrap Service Caller, easy hand-on and maintainance.
    - Technical Paper: [How we build scalable React app in FPTU.tech from scratch](https://kipalog.com/posts/Thiet-ke-scalable-React-App-tu-dau)
- **Database** implemented with MySQL Server (RDBMS) and Google Firebase Database (NoSQL).
- **Infrastructure**:
    - **CI/CD** integrated with Jenkins: automatic build & deploy after every commit pushed, 100% up time.
    - Deployed on a **Amazon EC2** VM with Docker eco-system and Nginx.
    - Can be deployed on a Container Cluster system (with Kubernetes) if need scaling.
    - Server uptime monitor and notice via Slack when down.

![Microservice Protobuf](https://i.imgur.com/Owb8Jgk.png)

![Flow](https://i.imgur.com/SZw1xuq.png)

## Technical Stack:

![Stack](https://i.imgur.com/suESnir.png)

- Backend: `Golang` (for high performance APIs)
    - Pure Go codebase
    - `gorilla-mux` for Routing and Dispatcher
    - `gorm` for ORM
    - `memcached` implement for server-side caching
    - `golang/protobuf` for protocol buffers
    - `golang/grpc` for gRPC implement, inter-service commucate
- Frontend: `React` (for single page application)
    - `React 16` with `react-router`, `redux`
    - Pure Node.js `Express` for Server-side rendering
    - `webpack` and `babel` for bundle and code chunking
- Javascript SDK for APIs implement:
    - Pure `TypeScript`
    - `axios` for XHR maker
    - `webpack` for bundle
- Infrastructure
    - `Docker` eco-system, every repos have a `Dockerfile` for container deployment
    - `Jenkins` for CI/CD integrate
    - `Amazon EC2` with Ubuntu 18.04 LTS (free-tier package 1GB RAM, 1CPU)
    - `Amazon RC2` for SQL serving (free-tier package)
    - Google `Firebase` SDK for Firebase Database (free package)
    - Be able to run in a Google Kubernetes Cluster when demo scale (free $300)
    - `Pingdom` to monitor uptime service and `Slack` to push notification

![React+Golang](https://media.licdn.com/dms/image/C5116AQHJEYOPh4eo5w/profile-displaybackgroundimage-shrink_350_1400/0?e=1554940800&v=beta&t=-uJ3wDvSKNW5TJU2VCBBuCnDXurhvWeaDQpa1ncdrMA)

## Development Toolkit
- Git version control with Github account
- Go Enviroment v1.2 with GOHOME, GOBIN and `godep`
- Node.js Environment latest with `yarn` installed
- Docker CE latest (Minikube or MiniK8s for Kubernetes Cluster development)
- Visual Studio Code latest (+ ESLint Plugin)
- MySQL Server 5.2 and MySQL Workbench (client)

## Details Workflow (updated 14-02-2019)

### Sprint & Backlogs
- Updating

### Issues
- None
