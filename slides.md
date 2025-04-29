---
theme: seriph
background: https://source.unsplash.com/collection/94734566/1920x1080
class: 'text-center'
highlighter: shiki
lineNumbers: true
info: |
  ## Docker Best Practices & Cloud Native Buildpacks
  Learn about Docker optimization and modern build tools.
drawings:
  persist: false
css: unocss
---

# How to Make Container Images Smaller

Building efficient and secure containers

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space for next page <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: image
---

# What is a Container?

A container is a lightweight, standalone, and executable package that includes everything needed to run a piece of software: code, runtime, system tools, libraries, and settings.

- Isolated from the host system
- Consistent across environments
- Fast startup and efficient resource usage

<img src="/assets/container-arch.png" alt="Containers" width="400" height="400">

---
layout: center
---

# Why to reduce container image size?

<v-clicks>

- Affects storage costs
- Impacts deployment times
- Influences scalability
- Can affect security

</v-clicks>




---
layout: two-cols
---

# Dockerfile examples

Node app 
Size: node:22 --> 1.12GB

```dockerfile {all|1|all}
FROM node:22
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
EXPOSE 4173

CMD ["npm", "run", "preview"]
```

::right::

<v-clicks>

- Base image
- Copying source files and node modules
- Takes more time to build

</v-clicks>

---
layout: two-cols
---

# Possible options

<v-clicks>

- Use minimal base images (e.g., `alpine`, `slim`)
- Multi-stage builds
- Copy only necessary files
- Leverage build cache
- Remove build dependencies
- Use `.dockerignore`
- Pin image versions

</v-clicks>

::right::

````md magic-move {lines: true}

```dockerfile
FROM node:22
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
EXPOSE 4173

CMD ["npm", "run", "preview"]
```


```dockerfile {all|1|9|all}
FROM node:22 AS builder

WORKDIR /app
COPY . .

RUN npm install
RUN npm run build

FROM node:22-alpine

COPY --from=builder /app/dist ./dist
COPY package.json .

CMD ["npm", "start"]
```

```dockerfile {all|1|9|all}
FROM node:22 AS builder

WORKDIR /app
COPY . .

RUN npm ci --production
RUN npm run build

FROM node:22-alpine@sha256:13b7e62e8df80264dbb74799570

COPY --from=builder /app/dist ./dist
COPY package.json .

CMD ["npm", "start"]
```

````


---
layout: two-cols
---

# Dockerfile Linting with Hadolint

Hadolint is a linter for Dockerfiles that helps you follow best practices and avoid common mistakes.

<v-clicks>

- Detects bad patterns and security issues
- Enforces consistency
- Easy to integrate in CI/CD

</v-clicks>

```bash
# Install Hadolint
brew install hadolint

# Lint a Dockerfile
hadolint Dockerfile
```

---
layout: two-cols
---

# Container Vulnerability Scanning

Scan your images for known vulnerabilities before shipping to production.

<v-clicks>

- Tools: Trivy, Snyk, Grype, Docker Scout
- Integrate scanning in CI/CD
- Regularly update base images

</v-clicks>

```bash
# Example: Scan with Trivy
trivy image myapp:latest
```
---
layout: two-cols
---

# Reduce Image Size with DockerSlim

DockerSlim automatically analyzes and optimizes your images, removing unnecessary files and reducing attack surface.

<v-clicks>

- Shrinks image size dramatically
- Removes unused binaries and files
- Easy to use

</v-clicks>

```bash
# Install DockerSlim
brew install docker-slim

# Minify an image
docker-slim build --tag myapp.slim myapp:latest
```


---

# Cloud Native Buildpacks

Cloud Native Buildpacks (CNBs) transform your application source code into container images that can run on any cloud. With buildpacks, organizations can concentrate the knowledge of container build best practices within a specialized team, instead of having application developers across the organization individually maintain their own Dockerfiles. 

<v-clicks>

- Automated container builds
- Language detection
- Built-in best practices
- Standardized process
- Regular security updates
- Reproducible builds

</v-clicks>

```bash
# Example usage
pack build myapp --builder paketobuildpacks/builder:base
```

---
layout: two-cols
---

# Buildpacks vs Dockerfile

#### Buildpacks:

<v-clicks>

- Automated detection
- Built-in optimizations
- Standard compliance
- Reduced maintenance
- Auto-updates

</v-clicks>

::right::
<br>

#### Dockerfile:

<v-clicks>

- Full control
- Custom configurations
- Transparent process
- Learning curve
- Manual updates

</v-clicks>

---
layout: center
---

# Tools and Links

- Ghostty Terminal - https://ghostty.org/
- Slidev (Slides for devlopers) - https://sli.dev/
- pnpm(Efficient package manager for nodejs) - https://pnpm.io/
- Hadolint(Dockerfile lint) - https://github.com/hadolint/hadolint
- Grype(Vulnerability scanner for container) - https://github.com/anchore/grype
- Slim(Optimise containers) - https://github.com/slimtoolkit/slim
- Buildpacks and Packeto - https://buildpacks.io/


---
layout: end
---

# Thank You!

[GitHub](https://github.com/arunkumar461/containers)
