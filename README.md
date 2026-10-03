# OpsWatch

## Linux Infrastructure Monitoring & Incident Response Platform

OpsWatch is a hands-on Cloud/DevOps learning project built from scratch to simulate a small real-world infrastructure monitoring and deployment environment.

The primary goal is **not** to create a complicated application.

The goal is to understand, build, operate, break, troubleshoot, recover, and document a complete system across:

* Linux
* Python
* Networking
* HTTP/REST APIs
* PostgreSQL
* Docker
* Nginx
* AWS
* Terraform
* GitHub Actions
* Monitoring
* Logging
* Alerting
* Security
* Incident response
* Deployment and rollback

This project is intentionally designed so that every component can be explained by the person who built it.

---

# 1. Project Philosophy

OpsWatch follows five rules.

### Rule 1 — Understand before adding

No technology is added just to make the architecture look impressive.

Every component must solve a real problem.

### Rule 2 — Build before automating

First understand how something works manually.

Then automate it.

Examples:

```text
Manual AWS deployment
        ↓
Understand resources
        ↓
Terraform
```

and:

```text
Manual Docker deployment
        ↓
Understand containers
        ↓
GitHub Actions
```

### Rule 3 — Break everything

A successful deployment is not enough.

Every important subsystem must have deliberately simulated failures.

Examples:

* stop the monitoring agent
* stop the API
* stop the database
* break a configuration value
* block a required port
* consume disk space
* create excessive CPU usage
* deploy a broken version
* interrupt a service

The goal is to learn how to identify and recover from failures.

### Rule 4 — AI is a tutor, not the developer

AI may be used to:

* explain concepts
* explain error messages
* review code
* suggest debugging approaches
* review architecture
* provide documentation references
* quiz the developer
* identify security issues

AI should not be used to generate the entire project for copy/paste implementation.

The person building OpsWatch must understand the code.

### Rule 5 — Do not move to the next phase until the current phase passes its acceptance criteria.

Each phase has a hard Definition of Done.

---

# 2. What OpsWatch Does

OpsWatch monitors Linux servers using a lightweight Python agent.

The agent periodically collects information such as:

* CPU utilization
* memory utilization
* disk utilization
* load average
* uptime
* hostname
* operating system information
* running process information
* network information
* agent version
* heartbeat

The agent sends this information to a FastAPI backend.

The backend:

* authenticates agents
* receives metrics
* stores metrics in PostgreSQL
* tracks server health
* detects missing heartbeats
* evaluates alert thresholds
* creates alerts
* exposes data to the dashboard

The dashboard allows an operator to:

* view registered servers
* view current health
* inspect metrics
* view heartbeat status
* view active alerts
* inspect recent incidents

The application will eventually run on AWS and be deployed through an automated CI/CD pipeline.

---

# 3. Final Architecture

The final version of OpsWatch will approximately look like this:

```text
                         ┌─────────────────────┐
                         │       Browser       │
                         │   OpsWatch Web UI   │
                         └──────────┬──────────┘
                                    │
                                  HTTPS
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │       Nginx         │
                         │ Reverse Proxy/TLS   │
                         └──────────┬──────────┘
                                    │
                                    ▼
                         ┌─────────────────────┐
                         │   FastAPI Backend   │
                         │       Python        │
                         └──────┬────────┬─────┘
                                │        │
                         metrics│        │alerts
                                │        │
                                ▼        ▼
                       ┌────────────┐  ┌────────────┐
                       │ PostgreSQL │  │ Alert      │
                       │            │  │ Engine     │
                       └────────────┘  └──────┬─────┘
                                              │
                                              ▼
                                      Email Notification


 Linux Server / Instance
 ┌──────────────────────────────────────┐
 │                                      │
 │       OpsWatch Python Agent           │
 │                                      │
 │ CPU / RAM / Disk / Uptime / Network  │
 │                                      │
 └───────────────────┬──────────────────┘
                     │
                     │ HTTPS
                     ▼
               FastAPI API
```

The cloud deployment eventually becomes:

```text
                         Internet
                            │
                            ▼
                         Nginx
                            │
                            ▼
                          EC2
                      ┌─────┴─────┐
                      │           │
                 OpsWatch API   Agent
                      │
                      │ private connection
                      ▼
                  RDS PostgreSQL

GitHub
   │
   ▼
GitHub Actions
   │
   ├── Test
   ├── Build
   ├── Security Scan
   ├── Push Image
   └── Deploy
           │
           ▼
          ECR
           │
           ▼
          EC2

EC2 / Application
       │
       ▼
   CloudWatch
       │
       ├── Logs
       ├── Metrics
       └── Alarms
```

Terraform manages the cloud infrastructure.

---

# 4. Technology Stack

The stack is intentionally frozen for the core version.

## Application

| Component       | Technology                         |
| --------------- | ---------------------------------- |
| Agent           | Python                             |
| Backend         | Python + FastAPI                   |
| Validation      | Pydantic                           |
| Database access | SQLAlchemy                         |
| Database        | PostgreSQL                         |
| Dashboard       | HTML + Jinja2 + minimal JavaScript |
| Web server      | Nginx                              |

## Infrastructure

| Component              | Technology      |
| ---------------------- | --------------- |
| Operating system       | Ubuntu Linux    |
| Containers             | Docker          |
| Local orchestration    | Docker Compose  |
| Cloud                  | AWS             |
| Compute                | EC2             |
| Database               | RDS PostgreSQL  |
| Container registry     | ECR             |
| Networking             | VPC             |
| Access control         | IAM             |
| Firewall               | Security Groups |
| Cloud monitoring       | CloudWatch      |
| Infrastructure as Code | Terraform       |

## Automation

| Component      | Technology     |
| -------------- | -------------- |
| Source control | Git            |
| Repository     | GitHub         |
| CI/CD          | GitHub Actions |
| Scripting      | Bash + Python  |

---

# 5. Technologies Explicitly Outside Core Scope

The following are deliberately excluded from the core project:

* Kubernetes
* EKS
* Jenkins
* Ansible
* Helm
* Argo CD
* Kafka
* RabbitMQ
* Redis
* Elasticsearch
* service mesh
* microservices
* multi-region AWS
* complex event-driven architecture
* AI anomaly detection
* mobile application
* complex frontend frameworks

These technologies may be explored later, but they must not delay completion of the core project.

A finished small system is more valuable than an unfinished large one.

---

# 6. Final Project Capabilities

The completed core project must provide all of the following.

## Server Management

* register a monitoring agent
* identify a server
* record hostname
* record operating system
* record agent version
* track registration time
* track last heartbeat

## Monitoring

Collect:

* CPU utilization
* memory utilization
* disk utilization
* load average
* uptime
* process information
* network information

## Health Detection

The system must distinguish between:

```text
HEALTHY
WARNING
CRITICAL
OFFLINE
```

The exact rules will be documented and implemented during the alerting phase.

## Alerts

At minimum:

* high CPU
* high memory
* high disk usage
* missing heartbeat
* health check failure

## Dashboard

Must display:

* list of servers
* server status
* current metric values
* heartbeat
* recent alerts
* basic historical metrics

## Deployment

Must support:

* local development
* Docker Compose
* Linux server deployment
* AWS deployment
* automated deployment

## Operations

Must provide:

* structured application logs
* service logs
* health endpoints
* deployment history
* rollback procedure
* troubleshooting documentation

## Security

Must include:

* API authentication
* secret management
* non-root container
* restricted AWS security groups
* private database access
* no secrets committed to Git
* least-privilege IAM where practical

---

# 7. Repository Structure

Target repository:

```text
opswatch/
│
├── README.md
├── LICENSE
├── .gitignore
├── .env.example
│
├── agent/
│   ├── README.md
│   ├── requirements.txt
│   ├── src/
│   │   └── opswatch_agent/
│   └── tests/
│
├── api/
│   ├── README.md
│   ├── requirements.txt
│   ├── src/
│   │   └── opswatch_api/
│   └── tests/
│
├── dashboard/
│   ├── templates/
│   └── static/
│
├── database/
│   ├── migrations/
│   └── seed/
│
├── docker/
│   ├── api.Dockerfile
│   └── agent.Dockerfile
│
├── compose/
│   └── docker-compose.yml
│
├── infrastructure/
│   └── terraform/
│       ├── main.tf
│       ├── variables.tf
│       ├── outputs.tf
│       ├── providers.tf
│       ├── networking.tf
│       ├── compute.tf
│       ├── database.tf
│       ├── security.tf
│       └── README.md
│
├── scripts/
│   ├── setup.sh
│   ├── health_check.sh
│   ├── backup.sh
│   └── deployment_check.sh
│
├── docs/
│   ├── architecture.md
│   ├── api.md
│   ├── database.md
│   ├── linux.md
│   ├── networking.md
│   ├── docker.md
│   ├── aws.md
│   ├── terraform.md
│   ├── cicd.md
│   ├── security.md
│   ├── troubleshooting.md
│   └── incidents/
│       ├── 001-agent-offline.md
│       ├── 002-api-down.md
│       ├── 003-database-unavailable.md
│       ├── 004-disk-full.md
│       ├── 005-high-cpu.md
│       └── 006-failed-deployment.md
│
└── .github/
    └── workflows/
        ├── test.yml
        └── deploy.yml
```

The repository may evolve, but major structural changes should be made deliberately.

---

# 8. Development Roadmap

The project is divided into nine phases.

```text
Phase 0  → Foundation
Phase 1  → Linux Monitoring Agent
Phase 2  → API + PostgreSQL
Phase 3  → Dashboard + Alerting
Phase 4  → Docker + Local Production Environment
Phase 5  → Linux Server Deployment
Phase 6  → AWS + Terraform
Phase 7  → CI/CD
Phase 8  → Observability + Incident Response
Final   → Documentation + Interview Preparation
```

Each phase has:

1. Learning objectives
2. Implementation work
3. Testing
4. Failure simulation
5. Documentation
6. Acceptance criteria

---

# PHASE 0 — FOUNDATION

## Goal

Create a clean development environment and learn the minimum Git/Python/Linux workflow required for the project.

## Learn

* Linux terminal
* directories
* files
* permissions basics
* processes
* environment variables
* Python installation
* virtual environments
* pip
* Git basics
* GitHub basics

## Build

Create the repository.

Create the initial directory structure.

Create:

```text
README.md
.gitignore
.env.example
```

Create isolated Python environments for the agent and API.

## Git knowledge required

Must understand:

```bash
git init
git status
git add
git commit
git log
git branch
git switch
git merge
git pull
git push
git diff
git restore
```

## Linux knowledge required

Must understand:

```bash
pwd
ls
cd
mkdir
touch
cp
mv
rm
cat
less
grep
find
chmod
chown
ps
top
df
du
ss
curl
```

## Acceptance Test

You are able to:

* create the repository
* create a Python virtual environment
* install a package
* create a Git branch
* make a commit
* inspect Git history
* create environment variables
* navigate Linux without a GUI

## Phase 0 Definition of Done

Do not continue until you can explain:

> What is a process?

> What is an environment variable?

> Why do we use a virtual environment?

> What is Git tracking?

> What is the difference between a working tree, staging area, and commit?

---

# PHASE 1 — LINUX MONITORING AGENT

## Goal

Build the monitoring agent yourself.

This is the first actual OpsWatch component.

## Core responsibility

The agent must:

1. start
2. identify the server
3. collect metrics
4. print metrics
5. log activity
6. continue running
7. handle temporary failures safely

## Metrics

Initial metrics:

```text
hostname
operating_system
agent_version
cpu_percent
memory_percent
disk_percent
load_average
uptime_seconds
timestamp
```

Later metrics:

```text
network statistics
top processes
```

## Python concepts to learn

Before implementation, understand:

* variables
* strings
* integers
* floats
* lists
* dictionaries
* functions
* modules
* imports
* classes
* exceptions
* context managers
* file I/O
* JSON
* logging
* datetime handling
* command-line arguments

## Agent configuration

Example:

```text
API_URL=
API_KEY=
SERVER_NAME=
INTERVAL_SECONDS=
```

Do not hard-code secrets.

## Expected behavior

```text
start agent
    ↓
load configuration
    ↓
identify host
    ↓
collect metrics
    ↓
validate metrics
    ↓
log results
    ↓
wait
    ↓
repeat
```

## Local output example

```text
2026-10-03 10:00:00 INFO Agent started
2026-10-03 10:00:01 INFO Hostname: dev-server
2026-10-03 10:00:01 INFO CPU: 24%
2026-10-03 10:00:01 INFO Memory: 51%
2026-10-03 10:00:01 INFO Disk: 43%
```

## Failure tests

### Test 1

Remove a required environment variable.

Expected behavior:

```text
agent refuses to start
clear error message
non-zero exit status
```

### Test 2

Make a monitored directory inaccessible.

Expected behavior:

```text
metric collection error is logged
agent does not silently crash
```

### Test 3

Interrupt the process.

Expected behavior:

```text
clean shutdown
useful log message
```

## Phase 1 Definition of Done

You can explain:

* what every file does
* how Python starts the program
* how metrics are collected
* how configuration is loaded
* how exceptions are handled
* how logging works
* what happens if metric collection fails

---

# PHASE 2 — FASTAPI + POSTGRESQL

## Goal

Turn the agent into a distributed system.

```text
Agent
  ↓
HTTP
  ↓
FastAPI
  ↓
PostgreSQL
```

## API endpoints

Initial API:

```text
POST /api/v1/servers/register
POST /api/v1/heartbeat
POST /api/v1/metrics

GET /api/v1/servers
GET /api/v1/servers/{server_id}
GET /api/v1/servers/{server_id}/metrics

GET /api/v1/alerts
GET /health
```

## Learn

### HTTP

Understand:

* request
* response
* headers
* body
* JSON
* methods
* status codes
* timeout
* connection refused
* DNS basics

### FastAPI

Understand:

* route
* request model
* response model
* validation
* dependency injection basics
* exception handling
* middleware basics
* application startup/shutdown

### PostgreSQL

Understand:

* database
* table
* row
* column
* primary key
* foreign key
* unique constraint
* index
* transaction
* connection
* query

## Initial database entities

### servers

```text
id
server_identifier
hostname
os_name
agent_version
registered_at
last_heartbeat
status
```

### metrics

```text
id
server_id
timestamp
cpu_percent
memory_percent
disk_percent
load_average
uptime_seconds
```

### alerts

```text
id
server_id
alert_type
severity
message
created_at
resolved_at
status
```

## API authentication

Every agent request must include an authentication mechanism.

Example:

```text
X-Agent-Key: <secret>
```

The implementation details can evolve, but unauthenticated metric submission should not be part of the final system.

## Failure tests

Test:

* API unavailable
* wrong API key
* malformed request
* missing field
* invalid metric
* database unavailable
* duplicate server
* slow request
* timeout

## Phase 2 Definition of Done

The following must work:

```text
Agent
   ↓
HTTP
   ↓
FastAPI
   ↓
PostgreSQL
```

You can inspect the database and see the metrics the agent sent.

You can also deliberately stop PostgreSQL and explain why the API fails.

---

# PHASE 3 — DASHBOARD + ALERT ENGINE

## Goal

Turn the backend into a usable operator tool.

## Dashboard

Initial pages:

```text
/
  Server overview

/servers/{id}
  Server details

/alerts
  Alert list
```

## Server overview

Show:

```text
Server
Status
CPU
Memory
Disk
Uptime
Last heartbeat
```

## Server details

Show:

* current metrics
* historical metric values
* last heartbeat
* agent version
* recent alerts

A simple chart can be used.

Do not spend significant time on UI design.

## Alert rules

Initial rules:

```text
CPU >= 80%
Memory >= 85%
Disk >= 80%
No heartbeat >= configured interval
Health check failure
```

Thresholds must be configurable.

## Alert lifecycle

```text
Normal
  ↓
Threshold exceeded
  ↓
Alert created
  ↓
Condition continues
  ↓
Alert remains active
  ↓
Condition returns to normal
  ↓
Alert resolved
```

Avoid creating duplicate alerts every few seconds.

## Notification

Implement one notification mechanism first.

Email is sufficient.

Do not build SMS, WhatsApp, Slack, Telegram, etc. in the core project.

## Failure tests

* CPU threshold exceeded
* disk threshold exceeded
* agent stopped
* API stopped
* database stopped
* alert generated twice
* alert resolves

## Phase 3 Definition of Done

You can demonstrate:

```text
High CPU
    ↓
Agent detects it
    ↓
API receives metric
    ↓
Database stores metric
    ↓
Alert engine evaluates rule
    ↓
Alert is created
    ↓
Dashboard shows alert
    ↓
Notification sent
    ↓
Problem resolved
    ↓
Alert resolved
```

---

# PHASE 4 — DOCKER + LOCAL PRODUCTION ENVIRONMENT

## Goal

Package OpsWatch so it can be consistently reproduced.

## Containers

Initial services:

```text
opswatch-api
opswatch-db
opswatch-dashboard
```

The dashboard may be served by the API/Nginx depending on implementation.

## Learn

* image
* container
* layer
* Dockerfile
* build context
* port mapping
* network
* volume
* environment variables
* container logs
* container health checks
* restart policies
* Docker Compose

## Docker requirements

Application containers should:

* run as non-root where practical
* have minimal dependencies
* use environment variables
* expose only required ports
* include health checks
* produce useful logs

## Persistent database

PostgreSQL data must survive container recreation using a Docker volume.

## Docker Compose

Target command:

```bash
docker compose up -d
```

The project should start locally without manually starting each component.

## Failure tests

### Container failure

Stop API container.

Observe:

```text
dashboard/API unavailable
```

Restart it.

Confirm recovery.

### Database failure

Stop PostgreSQL.

Observe API database errors.

Restart database.

Confirm recovery.

### Configuration failure

Change a required environment variable.

Observe startup failure.

### Port failure

Create a port collision.

Identify the process/container using the port.

Resolve the conflict.

## Phase 4 Definition of Done

A new developer with the documented prerequisites can:

```bash
git clone <repository>
docker compose up -d
```

and obtain a functioning local OpsWatch system.

---

# PHASE 5 — LINUX SERVER DEPLOYMENT

## Goal

Operate OpsWatch on a real Linux server.

This phase focuses heavily on Linux administration.

## Learn

* SSH
* users
* groups
* file permissions
* systemd
* journald
* Nginx
* firewall
* process management
* service management
* logs
* environment configuration

## Service architecture

Example:

```text
Internet
   ↓
Nginx
   ↓
OpsWatch API
   ↓
PostgreSQL
```

Agent:

```text
systemd
   ↓
opswatch-agent
```

## systemd

The monitoring agent should run as a managed service.

Example:

```bash
systemctl status opswatch-agent
systemctl start opswatch-agent
systemctl stop opswatch-agent
systemctl restart opswatch-agent
journalctl -u opswatch-agent
```

## Nginx responsibilities

* receive HTTP/HTTPS traffic
* reverse proxy to backend
* expose only the required public endpoint
* provide clean access logs

## Linux security

Use:

* dedicated service account
* restricted permissions
* SSH keys
* firewall
* no unnecessary open ports

## Failure drills

### Agent stopped

```text
systemctl stop opswatch-agent
```

Expected:

```text
heartbeat disappears
server becomes offline
alert generated
```

Recover:

```text
systemctl start opswatch-agent
```

### API process killed

Identify:

```bash
ps
ss
journalctl
```

Recover the application.

### Incorrect permissions

Cause the service to fail because of a permissions problem.

Diagnose it.

Fix it.

### Port blocked

Block required traffic.

Identify the problem using networking tools.

Fix the firewall configuration.

## Phase 5 Definition of Done

You can SSH into the server and independently determine:

* whether the process is running
* whether the port is listening
* whether Nginx is functioning
* whether the application is responding
* whether logs contain errors
* whether disk/CPU/memory are healthy
* whether the firewall is blocking traffic

---

# PHASE 6 — AWS + TERRAFORM

## Goal

Move the system into AWS and make the infrastructure reproducible.

## AWS components

Core:

```text
VPC
Subnet
Internet Gateway
Route Table
Security Groups
EC2
RDS PostgreSQL
ECR
IAM
CloudWatch
```

## Networking design

The database must not be publicly accessible.

Conceptually:

```text
Internet
   │
   ▼
EC2 / Application
   │
   │ private connection
   ▼
RDS PostgreSQL
```

Only required traffic should be allowed.

## EC2

Run the OpsWatch application.

## RDS

Run PostgreSQL separately from the application server.

This teaches the difference between:

```text
application layer
database layer
network boundary
```

## ECR

Store application container images.

## IAM

Learn:

* user
* role
* policy
* permission
* least privilege
* temporary credentials

Avoid long-lived credentials inside the repository.

## Terraform

Terraform must manage the infrastructure.

At minimum:

```text
providers.tf
variables.tf
main.tf
networking.tf
security.tf
compute.tf
database.tf
outputs.tf
```

## Terraform workflow

Understand:

```bash
terraform init
terraform fmt
terraform validate
terraform plan
terraform apply
terraform destroy
```

## Failure tests

### Security group failure

Remove required access.

Diagnose why communication fails.

### Database connectivity failure

Test:

```text
DNS
route
security group
database status
credentials
port
```

### Terraform state problem

Understand what state represents and why it exists.

### Resource dependency problem

Understand why some AWS resources must exist before others.

## Phase 6 Definition of Done

The environment can be created from Terraform.

You know what every important AWS resource does.

You can explain:

> Why is the database private?

> Why does EC2 need this security group rule?

> Why does the route table exist?

> What happens when Terraform destroys the environment?

---

# PHASE 7 — CI/CD

## Goal

Automate testing, image building, and deployment.

## Pipeline

Target:

```text
Developer
    ↓
git push
    ↓
GitHub
    ↓
GitHub Actions
    │
    ├── lint
    ├── tests
    ├── build Docker image
    ├── security scan
    └── push image
             ↓
            ECR
             ↓
           Deploy
             ↓
        Health Check
             ↓
       Success / Rollback
```

## CI requirements

Every pull request should run:

```text
lint
tests
build validation
```

## CD requirements

Main branch deployment should:

1. build image
2. tag image
3. scan image
4. push image to ECR
5. deploy
6. run health check
7. report success/failure

## Deployment versions

Every deployment should have a meaningful version.

Example:

```text
opswatch-api:1.0.0
opswatch-api:1.0.1
opswatch-api:1.1.0
```

## Rollback

A failed deployment must not require manually rebuilding the previous application.

The operator should be able to restore the previous known-good version.

## CI/CD security

Do not store long-lived AWS access keys in GitHub when a safer short-lived identity approach is available.

The exact identity setup must be documented.

## Failure drills

### Broken application

Commit deliberately broken code in a controlled branch.

Pipeline should fail before deployment.

### Broken image

Cause the container to fail health checks.

Deployment should detect failure.

### Failed deployment

Deploy a known-bad version.

Verify:

```text
deployment starts
health check fails
old version becomes active
service recovers
```

## Phase 7 Definition of Done

A normal code change can go from:

```text
git push
```

to:

```text
tested
built
scanned
deployed
verified
```

without manually rebuilding the entire environment.

---

# PHASE 8 — OBSERVABILITY + INCIDENT RESPONSE

## Goal

Treat OpsWatch as an operational system rather than simply an application.

## CloudWatch

Collect useful infrastructure/application information.

At minimum investigate:

* CPU
* disk
* logs
* application failures
* availability

## Application logging

Logs must contain enough context to diagnose failures.

Example:

```text
timestamp
level
component
server
request
message
error
```

Avoid logging secrets.

## Health endpoints

At minimum:

```text
GET /health
```

Possible additional distinction:

```text
/health/live
/health/ready
```

The distinction must be understood before implementation.

## Incident documentation

Every significant failure gets a document.

Template:

```text
# Incident: Agent Offline

## Date

## Symptoms

## Impact

## Detection

## Investigation

## Commands Used

## Root Cause

## Resolution

## Verification

## Prevention

## Lessons Learned
```

## Required incidents

At minimum:

### Incident 001 — Agent Offline

```text
stop agent
→ heartbeat stops
→ alert
→ inspect systemd
→ inspect logs
→ restart
→ verify recovery
```

### Incident 002 — API Down

```text
stop API
→ requests fail
→ inspect process
→ inspect listening ports
→ inspect logs
→ restore service
```

### Incident 003 — Database Unavailable

```text
stop PostgreSQL/RDS
→ API cannot connect
→ investigate
→ restore DB
→ verify recovery
```

### Incident 004 — Disk Full

Simulate a high disk condition safely.

Investigate using:

```text
df
du
find
```

Clean up and verify recovery.

### Incident 005 — High CPU

Generate controlled CPU usage.

Investigate using:

```text
top
ps
```

Identify the responsible process.

Resolve the problem.

### Incident 006 — Failed Deployment

Deploy a known-bad application version.

Detect failed health check.

Rollback.

Verify service recovery.

## Phase 8 Definition of Done

You can demonstrate at least six incidents from beginning to recovery and explain:

```text
What happened?
How did you detect it?
What evidence did you collect?
What commands did you use?
What was the root cause?
How did you fix it?
How could you prevent it?
```

---

# 9. Testing Strategy

OpsWatch uses multiple levels of testing.

## Unit Tests

Test:

* metric collection
* validation
* threshold logic
* alert creation
* status calculation

## Integration Tests

Test:

```text
Agent → API
API → PostgreSQL
Alert engine → Database
```

## Manual Infrastructure Testing

Infrastructure failures are deliberately reproduced manually.

Examples:

```text
service stop
network block
bad configuration
database unavailable
resource exhaustion
failed deployment
```

The purpose is learning operational behavior, not simply getting automated test coverage.

## Health Checks

Every deployment must have a simple verification step.

Example:

```text
GET /health
```

Expected:

```text
HTTP 200
```

---

# 10. Security Requirements

Security is part of the project from the beginning.

## Secrets

Never commit:

```text
AWS credentials
database passwords
API keys
email passwords
private keys
```

Use:

```text
.env
.env.example
AWS secret storage
GitHub secrets / short-lived credentials
```

## API Authentication

Agent-to-API requests must be authenticated.

## Database

Do not expose PostgreSQL directly to the public internet.

## Containers

Where practical:

* run as non-root
* minimize packages
* pin important dependencies
* expose only needed ports

## AWS

Use:

* restrictive Security Groups
* least-privilege IAM
* SSH key authentication
* minimal public exposure

## Logging

Never log:

* passwords
* API keys
* private keys
* sensitive connection strings

---

# 11. Networking Knowledge Required

Before finishing the project, understand:

```text
IP address
MAC address
port
socket
TCP
UDP
DNS
HTTP
HTTPS
TLS
subnet
CIDR
routing
NAT
firewall
security group
reverse proxy
```

You must be able to reason through:

```text
Browser
   ↓
DNS
   ↓
IP
   ↓
Port
   ↓
Nginx
   ↓
Application
   ↓
Database
```

When something fails, investigate from the network outward rather than guessing.

Useful Linux tools:

```bash
ping
curl
ss
ip
dig
nslookup
traceroute
nc
```

---

# 12. Troubleshooting Method

OpsWatch troubleshooting follows a repeatable process.

```text
1. Observe the symptom
2. Define the expected behavior
3. Identify the failing layer
4. Gather evidence
5. Check logs
6. Check network connectivity
7. Check processes/services
8. Check configuration
9. Check resource usage
10. Reproduce
11. Fix
12. Verify
13. Document
```

Example:

```text
Website returns 502
        ↓
Is Nginx running?
        ↓
Is application process running?
        ↓
Is application listening?
        ↓
Is correct port configured?
        ↓
Can Nginx reach application?
        ↓
What do application logs say?
        ↓
Fix
        ↓
Verify
```

Never jump directly to a random restart without understanding the failure.

---

# 13. Production-Minded Design Principles

OpsWatch does not claim to be a large production platform.

Instead, it demonstrates production-minded engineering.

The project should demonstrate:

* reproducible deployments
* configuration management
* health checks
* monitoring
* logging
* alerting
* authentication
* least privilege
* rollback
* backups
* failure recovery
* documented incidents
* infrastructure as code
* CI/CD
* separation of application and database
* controlled network exposure

---

# 14. Database Backup Requirement

Before declaring the cloud version complete, implement a simple backup strategy for the application data.

The strategy must answer:

```text
What is backed up?
Where is it stored?
How often?
How do we verify the backup?
How do we restore it?
```

A backup that has never been restored should not be treated as proven.

Perform at least one test restoration.

---

# 15. Configuration Management

Configuration should be separated from code.

Examples:

```text
API_URL
DATABASE_URL
AGENT_INTERVAL
CPU_THRESHOLD
MEMORY_THRESHOLD
DISK_THRESHOLD
API_KEY
```

Use different configuration for:

```text
local
staging/test
production
```

Do not duplicate application code merely to change configuration.

---

# 16. Project Milestones

Each milestone is a usable state.

## Milestone 1

```text
Linux monitoring script works
```

## Milestone 2

```text
Agent → FastAPI → PostgreSQL
```

## Milestone 3

```text
Dashboard + alerts
```

## Milestone 4

```text
Dockerized local system
```

## Milestone 5

```text
Linux server deployment
```

## Milestone 6

```text
AWS deployment
```

## Milestone 7

```text
Terraform-managed infrastructure
```

## Milestone 8

```text
Automated CI/CD
```

## Milestone 9

```text
Monitoring + incident response
```

## Final Milestone

```text
Documented, reproducible, demonstrable project
```

---

# 17. Final Acceptance Test

OpsWatch is considered complete only when the following scenario can be demonstrated.

## Clean Setup

```text
Clone repository
Install prerequisites
Configure environment
Start local environment
```

## Agent

```text
Register server
Send heartbeat
Send metrics
```

## Dashboard

```text
See server
See metrics
See status
```

## Alert

```text
Trigger high CPU or disk
Receive alert
See alert on dashboard
Resolve condition
Confirm alert resolution
```

## Docker

```text
Build images
Start Compose
Stop container
Recover container
Inspect logs
```

## Linux

```text
Start service
Stop service
Inspect systemd
Inspect logs
Restart service
```

## AWS

```text
Provision infrastructure
Deploy application
Connect application to RDS
Verify health
```

## CI/CD

```text
Push code
Run tests
Build image
Scan image
Push image
Deploy
Health check
```

## Failure

```text
Deploy known-bad version
Detect failure
Rollback
Verify recovery
```

## Incident Response

Explain at least six incidents from:

```text
symptom
→ investigation
→ root cause
→ fix
→ verification
→ prevention
```

---

# 18. Interview Readiness Requirement

The project is not finished when it works.

It is finished when the developer can defend it.

The developer must be able to explain:

## Architecture

* Why Python?
* Why FastAPI?
* Why PostgreSQL?
* Why Docker?
* Why Nginx?
* Why EC2?
* Why RDS?
* Why ECR?
* Why Terraform?
* Why GitHub Actions?
* Why CloudWatch?

## Linux

* What is a process?
* What is a service?
* What does systemd do?
* How do you inspect logs?
* How do you find which process owns a port?
* What happens when disk is full?

## Networking

* What happens when a browser requests the dashboard?
* What is DNS?
* What is TCP?
* What is a port?
* Why would a connection be refused?
* Why might a connection time out?
* What does Nginx do?

## Docker

* What is an image?
* What is a container?
* Why use a volume?
* Why use a network?
* What happens if the container exits?
* Why shouldn't application containers run as root?

## AWS

* What is a VPC?
* What is a subnet?
* What is a route table?
* What is a security group?
* Why is RDS private?
* What is IAM?
* Why shouldn't AWS credentials be hard-coded?

## Terraform

* Why Terraform?
* What is Terraform state?
* What does `plan` do?
* What does `apply` do?
* What happens during `destroy`?
* What happens if infrastructure changes outside Terraform?

## CI/CD

* What triggers the pipeline?
* What happens when tests fail?
* How is a Docker image versioned?
* How is the image deployed?
* How is a bad deployment detected?
* How does rollback work?

## Monitoring

* What is a heartbeat?
* How do you decide whether a server is offline?
* How are thresholds evaluated?
* What is the difference between a metric, log, and alert?

## Incident response

* What happened?
* How did you know?
* What evidence did you collect?
* What command did you run?
* What was the root cause?
* How did you recover?
* How would you prevent recurrence?

If the answer to an interview question is:

> "I don't know because AI generated that part."

that component was not sufficiently understood and should not be considered complete.

---

# 19. Resume Requirement

Final resume description should focus on engineering outcomes rather than listing tools.

Example:

**OpsWatch — Linux Infrastructure Monitoring & Incident Response Platform**

> Built a Python/FastAPI monitoring platform for Linux servers with PostgreSQL-based metric storage, heartbeat-based availability detection, threshold-based alerting, Dockerized local deployment, AWS infrastructure provisioned through Terraform, and GitHub Actions CI/CD with automated health verification and rollback. Designed and documented failure scenarios covering service outages, database connectivity, resource exhaustion, networking issues, and failed deployments.

Only include technologies that were actually implemented.

---

# 20. Git Commit Strategy

Use meaningful commits.

Examples:

```text
chore: initialize project structure
feat(agent): collect cpu memory and disk metrics
feat(agent): add heartbeat generation
feat(api): add server registration endpoint
feat(api): persist metrics to postgres
feat(alerts): add disk threshold alerts
feat(dashboard): add server overview
feat(docker): containerize api
feat(linux): add systemd service
feat(terraform): provision vpc and ec2
feat(cicd): add automated test pipeline
feat(cicd): add image deployment
feat(ops): add health verification
docs(incident): document agent offline failure
```

Avoid commits like:

```text
update
changes
final
final2
latest
working
```

---

# 21. Documentation Requirements

Every major phase must update documentation.

Required documents:

### `docs/architecture.md`

Explain:

```text
components
connections
data flow
deployment flow
trust boundaries
```

### `docs/linux.md`

Document:

```text
services
systemd
logs
permissions
process troubleshooting
```

### `docs/networking.md`

Document:

```text
ports
DNS
HTTP
network flow
AWS networking
```

### `docs/aws.md`

Document:

```text
VPC
subnets
security groups
EC2
RDS
ECR
IAM
CloudWatch
```

### `docs/terraform.md`

Document:

```text
resources
state
variables
outputs
workflow
destroy/recreate
```

### `docs/cicd.md`

Document:

```text
pipeline
stages
deployment
health checks
rollback
```

### `docs/troubleshooting.md`

Document recurring troubleshooting procedures.

### `docs/incidents/*`

Document real failure exercises performed during development.

---

# 22. Evidence for Portfolio

The final repository should include evidence such as:

* architecture diagram
* local Docker deployment
* dashboard screenshot
* Linux service screenshot/output
* AWS architecture diagram
* Terraform output
* GitHub Actions successful run
* failed deployment
* rollback
* CloudWatch logs/metrics
* alert example
* incident report examples

Do not fabricate screenshots.

All evidence should come from the actual system.

---

# 23. What Success Looks Like

At completion, OpsWatch should demonstrate the following chain:

```text
Linux
  ↓
Python
  ↓
Metrics
  ↓
HTTP
  ↓
FastAPI
  ↓
PostgreSQL
  ↓
Dashboard
  ↓
Alerts
  ↓
Docker
  ↓
Linux Services
  ↓
AWS
  ↓
Terraform
  ↓
GitHub Actions
  ↓
Deployment
  ↓
Monitoring
  ↓
Incident Response
  ↓
Recovery
```

The project is intentionally designed so that learning one layer naturally creates the reason to learn the next.

---

# 24. Project Completion Checklist

## Foundation

* [ ] Repository initialized
* [ ] README created
* [ ] `.gitignore` created
* [ ] environment configuration documented
* [ ] Git workflow understood

## Agent

* [ ] CPU metrics
* [ ] memory metrics
* [ ] disk metrics
* [ ] uptime
* [ ] hostname
* [ ] OS information
* [ ] logging
* [ ] configuration
* [ ] error handling
* [ ] heartbeat

## API

* [ ] server registration
* [ ] authentication
* [ ] heartbeat endpoint
* [ ] metrics endpoint
* [ ] server endpoint
* [ ] alert endpoint
* [ ] health endpoint
* [ ] validation
* [ ] exception handling

## Database

* [ ] schema designed
* [ ] migrations
* [ ] constraints
* [ ] indexes
* [ ] metrics stored
* [ ] alerts stored
* [ ] backup plan
* [ ] restoration tested

## Dashboard

* [ ] server overview
* [ ] server details
* [ ] current metrics
* [ ] historical metrics
* [ ] alerts
* [ ] status

## Alerting

* [ ] CPU threshold
* [ ] memory threshold
* [ ] disk threshold
* [ ] heartbeat timeout
* [ ] alert creation
* [ ] alert resolution
* [ ] email notification

## Docker

* [ ] API image
* [ ] agent image
* [ ] Compose
* [ ] persistent database volume
* [ ] health checks
* [ ] non-root containers where practical
* [ ] container logs

## Linux

* [ ] SSH
* [ ] service account
* [ ] systemd service
* [ ] journald
* [ ] Nginx
* [ ] firewall
* [ ] process troubleshooting
* [ ] disk troubleshooting

## AWS

* [ ] VPC
* [ ] subnet
* [ ] route table
* [ ] security group
* [ ] EC2
* [ ] RDS
* [ ] ECR
* [ ] IAM
* [ ] CloudWatch

## Terraform

* [ ] provider
* [ ] variables
* [ ] outputs
* [ ] networking
* [ ] compute
* [ ] database
* [ ] security
* [ ] plan
* [ ] apply
* [ ] destroy

## CI/CD

* [ ] automated tests
* [ ] Docker build
* [ ] image scanning
* [ ] ECR push
* [ ] deployment
* [ ] health check
* [ ] versioning
* [ ] rollback

## Operations

* [ ] centralized logs
* [ ] metrics
* [ ] health checks
* [ ] alerts
* [ ] deployment history
* [ ] backup verification
* [ ] incident documentation

## Failure Lab

* [ ] agent offline
* [ ] API down
* [ ] database unavailable
* [ ] disk exhaustion
* [ ] high CPU
* [ ] network failure
* [ ] bad configuration
* [ ] failed deployment
* [ ] rollback

## Portfolio

* [ ] architecture diagram
* [ ] README complete
* [ ] screenshots
* [ ] incident reports
* [ ] deployment documentation
* [ ] troubleshooting guide
* [ ] interview questions prepared

---

# 25. Final Definition of Done

OpsWatch is officially complete only when:

1. The complete system works locally.
2. The system runs in Docker.
3. The system runs on Linux.
4. The application is deployed to AWS.
5. Infrastructure is reproducible through Terraform.
6. Code changes pass automated CI.
7. Deployments are automated.
8. Deployment health is automatically checked.
9. A failed deployment can be rolled back.
10. Monitoring and logging work.
11. Alerts work.
12. Backups have been restored successfully at least once.
13. At least six realistic failure scenarios have been reproduced.
14. Each failure has been investigated and documented.
15. The developer can explain every major component.
16. The developer can explain why each major technology was selected.
17. The developer can troubleshoot the system without relying on AI.
18. The repository contains enough documentation for another developer to reproduce the project.

**Until these conditions are met, the project is still in development.**

---

# 26. Core Principle

OpsWatch is not a project to finish as quickly as possible.

It is a project designed to make the developer capable of saying:

> "I built it."

and then proving it when the interviewer asks:

> "Show me."

The final objective is not the number of technologies in the repository.

The objective is **technical ownership**.

```text
Understand
    ↓
Build
    ↓
Test
    ↓
Break
    ↓
Troubleshoot
    ↓
Recover
    ↓
Automate
    ↓
Document
    ↓
Defend in interview
```

That is the complete OpsWatch learning loop.
