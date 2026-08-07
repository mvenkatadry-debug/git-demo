1. How to run the app locally?
2. .gitignore in real time
3. github.event
4. how authentication works
5. check artifacts. example uploading docker image during build
6. export path in linux
7. for docker check what pks need to be installed for node.js, springboot, and other
github actions context- https://docs.github.com/en/actions/reference/workflows-and-actions/contexts

8. reading the logs in workflows
9. github context https://docs.github.com/en/actions/reference/workflows-and-actions/contexts#github-context


for Node.js application
---
my-app/
│
├── package.json
├── package-lock.json
├── src/
└── Dockerfile


FROM node:22

WORKDIR /app

COPY . .

RUN npm install

RUN npm run build

CMD ["npm", "start"]

**Step 2:** What's inside this image?

After the build finishes, your image contains

Operating System
Node.js
npm
package.json
node_modules
Source Code
Build Tools
Compiled Files
Temporary Files

Everything.

Even things you no longer need.

For example:

TypeScript compiler
Webpack
ESLint
Testing libraries

These were only needed during the build.

But they stay forever.
https://docs.docker.com/get-started/docker-concepts/building-images/multi-stage-builds/?utm_source=chatgpt.com

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

Example:
{
  "scripts": {
    "build": "tsc",
    "start": "node dist/index.js"
  }
}

OR

{
  "scripts": {
    "build": "vite build",
    "start": "node server.js"
  }
}

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

