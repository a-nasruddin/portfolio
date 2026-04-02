# Portfolio — Hugo · Docker · Kubernetes

> A personal portfolio website built with Hugo SSG, containerized with Docker + Nginx, and deployable to a local Kubernetes cluster.

---

## Tech Stack

| Layer | Technology |
|---|---|
| Static Site Generator | [Hugo Extended](https://gohugo.io/) |
| Theme | Custom `devops-minimal` (dark DevOps aesthetic) |
| Web Server | Nginx Alpine |
| Containerization | Docker (multi-stage build) |
| Container Orchestration | Kubernetes (K3d) |

---

## Project Structure

```
portfolio/
├── config.toml                   # Hugo configuration
├── content/
│   ├── _index.md                 # Homepage content (Hero, About, Skills, Contact)
│   └── blog/                     # Blog posts
├── themes/
│   └── devops-minimal/           # Custom Hugo theme
│       ├── layouts/              # HTML templates
│       └── static/
│           ├── css/main.css      # Stylesheet
│           └── js/main.js        # Animations & interactions
├── Dockerfile                    # Multi-stage build: Hugo → Nginx Alpine
├── nginx.conf                    # Nginx configuration
├── k8s/
│   ├── deployment.yaml           # Kubernetes Deployment (2 replicas)
│   └── service.yaml              # Kubernetes NodePort Service (:30080)
└── README.md
```

---

## Prerequisites

| Tool | Minimum Version |
|---|---|
| [Hugo Extended](https://github.com/gohugoio/hugo/releases) | 0.110+ |
| [Docker](https://docs.docker.com/get-docker/) | 24.x+ |
| [kubectl](https://kubernetes.io/docs/tasks/tools/) | 1.28+ |
| [K3d](https://k3d.io/) | 5.x+ |

---

## Running Locally

```bash
# Start the Hugo development server
hugo server -D
```

Then open [http://localhost:1313](http://localhost:1313) in your browser.

---

## Docker

```bash
# Build the image
docker build -t portfolio:latest .

# Run locally
docker run --rm -p 8080:80 portfolio:latest
# Open: http://localhost:8080
```

---

## Kubernetes (K3d)

```bash
# Create a local cluster with NodePort forwarding
k3d cluster create portfolio-cluster \
  --port "30080:30080@server:0" \
  --agents 1

# Import the Docker image into the cluster
k3d image import portfolio:latest -c portfolio-cluster

# Deploy
kubectl apply -f k8s/

# Check pod status
kubectl get pods -w
```

Once pods are `Running`, open [http://localhost:30080](http://localhost:30080).

---

## Customizing Content

All homepage content is driven by the front matter in `content/_index.md`:

| Section | Key | Description |
|---|---|---|
| Hero | `hero.firstName`, `hero.lastName` | Your name |
| Hero | `hero.roles` | Roles shown in typing animation |
| About | `about.paragraphs` | Bio paragraphs |
| Skills | `skills` | Skill categories and tools |
| Contact | `contact.email`, `contact.github`, `contact.linkedin` | Contact links |

---
