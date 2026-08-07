1. How to run the app locally?
2. .gitignore in real time
3. github.event
4. how authentication works
5. check artifacts. example uploading docker image during build
6. 6 export path in linux
7 for docker check what pks need to be installed for node.js,springboot ..other
github actions context- https://docs.github.com/en/actions/reference/workflows-and-actions/contexts

8 reading the logs in work flows
9. github conext https://docs.github.com/en/actions/reference/workflows-and-actions/contexts#github-context


for Node.js application
---
```
my-app/
│
├── package.json
├── package-lock.json
├── src/
└── Dockerfile
```


Step 2: What's inside this image?

After Docker builds this image:
```
FROM node:22
WORKDIR /app
COPY . .
RUN npm install
RUN npm run build
CMD ["npm", "start"]
```

the image typically contains:

Operating System (Debian/Bookworm)
Node.js
npm
Your application source code
package.json
package-lock.json (if present)
node_modules
Compiled build output (e.g., dist/ or build/)
npm cache (unless cleaned)
Application configuration files

If your project uses build tools, they are also included because npm install installs all dependencies by default (unless you explicitly install only production dependencies).
For example:
TypeScript
Webpack
Vite
Babel
ESLint
Jest
React
Angular CLI
Nest CLI
Development dependencies

These tools are required to build the application, but they are usually not needed to run it in production.
So your final image contains:

✓ Operating System
✓ Node.js
✓ npm
✓ Source code
✓ node_modules
✓ Build output (dist/, build/, etc.)
✓ Build tools (if installed)
✓ Development dependencies (unless excluded)
✓ Temporary files and npm cache (unless removed)

**Why is this a problem?**
Imagine your application only needs this to run:

Node.js
dist/
Production dependencies

But your image also contains:
TypeScript compiler
Webpack
ESLint
Jest
Source code
npm cache
Development dependencies



But they stay forever.
**Multistage builds Doc:** https://docs.docker.com/get-started/docker-concepts/building-images/multi-stage-builds/?utm_source=chatgpt.com

| Files you see      | Project type | Build image  |
| ------------------ | ------------ | ------------ |
| `package.json`     | Node.js      | `node:22`    |
| `pom.xml`          | Java Maven   | `maven:...`  |
| `build.gradle`     | Java Gradle  | `gradle:...` |
| `go.mod`           | Go           | `golang:...` |
| `Cargo.toml`       | Rust         | `rust:...`   |
| `requirements.txt` | Python       | `python:...` |
| `composer.json`    | PHP          | `php:...`    |

**For Node.js
Step 2: Find the build command**

Look in package.json.
```
Example:
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  }
}
```

OR
```
{
  "scripts": {
    "build": "vite build",
    "start": "node server.js"
  }
}
```

Then
npm run build

then you know:

npm must exist
Node.js must exist

So the builder image should be:

FROM node:22 AS builder

--
1. Docker Official Image page for the Node image https://hub.docker.com/_/node?utm_source=chatgpt.com

This page explains the image variants (node, node:slim, node:alpine, etc.) and links to the Dockerfiles used to build them

2. Official GitHub repository (best place to inspect contents) https://github.com/nodejs/docker-node?utm_source=chatgpt.com

On this page, under Image Variants, it explicitly states:

"All of the images contain pre-installed versions of node which includes also npm."

FROM node:22
is based on the buildpack-deps image. That means it includes Node.js plus a broader set of common development tools and libraries intended for building applications.
https://github.com/nodejs/docker-node?utm_source=chatgpt.com

FROM node:22-slim
uses a much smaller base image. Docker's documentation says the slim variant "only contains the minimal packages needed to run node."


4. How do professionals see exactly what's installed?
The most reliable approach is to start a shell in the image.
docker run -it --rm node:22 bash

Then inspect it:
node --version
npm --version
which node
which npm
check the operating system:cat /etc/os-release
List installed Debian packages::dpkg -l
inspect the available commands:ls /usr/local/bin
To install any commands in container:
#RUN apt-get update && \
    apt-get install -y curl


