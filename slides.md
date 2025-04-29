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

# What is container image

![container architecture](/assets/container-arch.png)

---
layout: image
---


# Why to reduce container image size?

<v-clicks>

- Affetcts storage costs
- Impacts deplyment times
- Influces scalablity
- Can affct security

</v-clicks>

---
layout: two-cols
---

# Dockerfile examples

Node app 

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
- Includes only necessary artifacts
- Better security posture

</v-clicks>

---
layout: two-cols
---

# Multi-stage Builds
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


```dockerfile {all|1||all}
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


````
::right::

<v-clicks>

- Separates build and runtime
- Reduces final image size
- Includes only necessary artifacts
- Better security posture

</v-clicks>

---

# Layer Optimization

<div grid="~ cols-2 gap-4">

```dockerfile
# ❌ Bad Practice
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
```

```dockerfile
# ✅ Good Practice
FROM node:18
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
```

</div>

<v-clicks>

- Leverage build cache
- Order instructions by change frequency
- Combine RUN commands
- Use .dockerignore

</v-clicks>

---
layout: image-right
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Security Best Practices

<v-clicks>

- Scan for vulnerabilities
- Use specific version tags
- Minimize attack surface
- Keep base images updated
- Use non-root users
- Sign your images
- Implement least privilege

</v-clicks>

---

# Cloud Native Buildpacks

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

Buildpacks:
<v-clicks>

- Automated detection
- Built-in optimizations
- Standard compliance
- Reduced maintenance
- Auto-updates

</v-clicks>

::right::

Dockerfile:
<v-clicks>

- Full control
- Custom configurations
- Transparent process
- Learning curve
- Manual updates

</v-clicks>

---
layout: center
class: text-center
---

# Best Practices Summary

<div class="grid grid-cols-2 gap-4">
<div>

## Docker
<v-clicks>

- Multi-stage builds
- Layer optimization
- Security first
- Version control
- Documentation

</v-clicks>
</div>
<div>

## Buildpacks
<v-clicks>

- Automated builds
- Standardization
- Regular updates
- Easy adoption
- Cloud native ready

</v-clicks>
</div>
</div>

---
layout: end
---

# Thank You!

[Docker Docs](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) · [Buildpacks.io](https://buildpacks.io/) · [GitHub](https://github.com/buildpacks)
