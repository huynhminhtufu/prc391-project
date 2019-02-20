## `PRC391 Project` Requirement Analysis & Technical Architect Document

![FPTU](https://i.imgur.com/Y9AIFBR.png)

## Requirements (based on User's stories) and Functions to be implemented:
- App must has a `user board` and `admin board`.
- App must be designed a easy UI for users to post their confession, with image upload available.
- App must be designed to help Confessions Facebook fan page managers easily manage the contents, automatic schedule and posting to Facebook.
- App must has a communication system for administrators to feedback about the user's confessions and postback their request.
- App must have web push notification system to notice to users that their confessions was approved.
- App must has list & history of all confession, has a clear visual or analyst board of total sent confession and which posted on Facebook.
- App must has a page to fetch all university related articles (crawl) for user to quick view, and also be able to comment on those articles.
- App must be able to run in both Desktop and Mobile web.
- App must be implement a anti-spam system.

## Entity Diagram

![Entity Diagram](./entity.jpg)

## Use Case Diagram

![Use Case Diagram](./use-case.jpg)

## Scalable Architecture: Microservices
- Project Architect is based on scaling `Microservices` principles.
- Everything in system is a Docker image (Dockerized), and be able to run on a Kubernetes Cluster (Google Kubernetes Engine with Load Balancer) as `containers` with specific `environment variables`.
- How it scale: Google Kubernetes Engine (GKE) Cluster managed, the system will auto scale up or scale down the containers (nodes) to match up with the traffic, due to our configurations.
- **Backend** implements several services to handle all actions: User API, Confession API, DMS (Database Management System which create & excute query). Communication between internal services use **high performance gRPC** (http/2) with Protobuf (binary proto buffer).
- **Frontend** implements with Single Page Application, based `Universal Web App` approach. Implement a **Javascript SDK** separated to wrap Service Caller, easy hand-on and maintainance.
- **Database** implemented with MySQL Server (RDBMS) and Google Firebase Database (NoSQL).
- **Infrastructure**:
    - **CI/CD** integrated with Jenkins: automatic build & deploy after every commit pushed, 100% up time.
    - Deployed on a **Amazon EC2** VM with Docker eco-system and Nginx.
    - Can be deployed on a Container Cluster system (with Kubernetes) if need scaling.
    - Server uptime monitor and notice via Slack when down.
    
![Architect](https://i.imgur.com/kzOkhqJ.jpg)

![Microservice Protobuf](https://i.imgur.com/Owb8Jgk.png)

![Flow](https://i.imgur.com/SZw1xuq.png)

## Technical Stack:

![Stack](https://i.imgur.com/suESnir.png)

- Backend: `Golang` (for the high performance)
    - Pure Go codebase
    - `gorilla-mux` for Routing and Dispatcher
    - `memcached` implement for server-side caching
    - `golang/protobuf` for protocol buffers
    - `golang/grpc` for gRPC implement, inter-service commucate
    - `google/recaptcha` to implement anti-spam system
- Frontend: `React` (for single page application)
    - `React 16` with `react-router`, `redux`
    - Pure Node.js `Express` for Server-side rendering
    - `webpack` and `babel` for bundle and code chunking
    - Use `firebase-sdk` to implement `Cloud Message Push Notification`
    - Use `firebase-sdk` to implement `Firebase Storage for Storing Images`
- Javascript SDK for APIs implement:
    - Pure `TypeScript`
    - `axios` for API caller
    - `webpack` for chunking and bundle
- Infrastructure: `Docker`
    - `Cloudflare` for DNS (resolve server IP) and CDN (caching)
    - `Docker` eco-system, every repos have a `Dockerfile` for container deployment
    - `Nginx` for reverse proxy, resolve services from docker containers
    - `Jenkins` for CI/CD integrate
    - `Amazon EC2` with Ubuntu 18.04 LTS (free-tier package 1GB RAM, 1CPU)
    - `Amazon RDS` for SQL serving (free-tier package)
    - Google `Firebase` SDK for Firebase Database (free package)
    - Be able to run in a Google Kubernetes Cluster when demo scale (free $300)
    - `Pingdom` to monitor uptime service and `Slack` to push notification

![React+Golang](https://media.licdn.com/dms/image/C5116AQHJEYOPh4eo5w/profile-displaybackgroundimage-shrink_350_1400/0?e=1554940800&v=beta&t=-uJ3wDvSKNW5TJU2VCBBuCnDXurhvWeaDQpa1ncdrMA)

## Development Toolkits:
- Git version control with a Github account to access repos
- Go Enviroment v1.2 with GOHOME, GOBIN and `godep`
- Node.js Environment latest with `yarn` installed
- Docker CE latest (Minikube or MiniK8s for Kubernetes Cluster development)
- Visual Studio Code latest (+ ESLint Plugin)
- MySQL Server 5.2 and MySQL Workbench (client)

## Team and Main roles
- Front-end: `Thanh`
- Back-end & Data: `Tri`
- Back-end & DevOps: `Tu`

## Details Workflow (updated 14-02-2019)
### Sprint & Backlogs (sprint lasts 1 week)
#### Sprint 1 (7-2-2019 -> 14-2-2019): Build up code base
- Build up codebase for Backend system: `Tu`
- Build up codebase for Frontend system: `Thanh`
- Documenting, visual relation entities, diagrams and documenting: `Tri`
- Create deployment, build script, infrastructure, Amazon EC2, RDS, Firebase: `Tu`
- Create domain, DNS to Cloudflare, mapping to Nginx proxy: `Tu`
- Finalize entities to Database Design: `Tri` 
#### Sprint 2 (15-2-2019 -> 21-2-2019): Develop user board
- Build and implement APIs for user board, CRUD & Search: `Tu`, `Tri`
    - Send confess APIs
    - Fetch confess APIs
    - Search confess APIs
- Build up UI and design forms, apply APIs for user board: `Thanh`
- Update documents, requirements, diagrams: `Tri`
- First deploy - monitor CI/CD, server: `Tu`
#### Sprint 3 (22-2-2019 -> 29-2-2019): Develop admin board
- Build and implement APIs for admin board, CRUD & Search: `Tu`, `Tri`
    - Approve confess APIs
    - Reject confess APIs
- Build up UI and design forms, apply APIs for admin board: `Thanh`
- Update documents, requirements, diagrams: `Tri
#### Sprint 4 (30-2-2019 -> 7-3-2019): Microservices chunking
- Restructure codebase for as Go microservice, add gRPC and protobuf: `Tu`
- Develop UserAPI, ConfessionAPI: `Thanh`
- Develop UserDMS, ConfessionDMS: `Tri`
- Develop CrawlWorker, CrawlAPI: `Tu`
- Develop API Gateway: `Tu`
#### Sprint 5 (7-3-2019 -> 15-3-2019): Microservices finalize
- Uodating and finailize JavaScript SDK: `Tu`, `Tri`
- Updating UI and animation: `Thanh`
- Overall testing: `Tri`, `Thanh`, `Tu`
### Issues
- Research microservice technologies gRPC + protobuf (deadline: 25-02-2019)

### App ecosystem:
- Web App (production): https://fptu.tech (Production for end-users)
- Web App (staging): http://staging.fptu.tech (Support debug by source-map)
- JS SDK Serving Server: https://sdk.fptu.tech/fptu-sdk.js (Static Nginx)
- JSON API Gateway (production): https://api.fptu.tech
- Jenkins CI/CD: https://cicd.fptu.tech
- Grafana analytics and monitoring: https://grafana.fptu.tech/
    - AWS EC2 Server: http://54.179.166.164/
    - AWS RDS Server: http://dms.cfszgusygva5.ap-southeast-1.rds.amazonaws.com:3306
- EC2 resource usage:
![Terminal](https://i.imgur.com/O8PxyA1.png)
