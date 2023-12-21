# Next JS Docker Template

### Summary
An easy to use template for developing or deploying your Next App inside a Docker Container! Simply clone the repo & update the dependencies or add the dockerfiles to your project and set next.config to standalone.

### An important note on secret management

**Use [Docker Secrets](https://docs.docker.com/compose/use-secrets/) or make your Docker repository private to protect secrets and environment variables.**

## Requirements

This template utilizes [Docker Buildx](https://github.com/docker/buildx) to build multi architecture docker images. Make sure you have this setup before building or use build for single architecture only.

### In your Next JS application
Inside next.config.js add the following line:
```
const nextConfig = {
    output: 'standalone',
}
```

### In Docker
Inside docker-compose.build.yml
```
# modify the build context to your project's directory

next-prod:
  image: docker-user/next:prod
  build:
    context: ${PWD}/directory
    dockerfile: Dockerfile.production
    x-bake:
      platforms:
        - linux/amd64
        - linux/arm64
```


Inside all of the docker-compose files:
```
# modify the service image

next:
  image: docker-user/image-name:tag
```

Inside .env
```
# modify TZ to your timezone & PWD to your project's directory

TZ = Asia/Jakarta
PWD = /../path/to/your/project
```
## How to build this image

### For Development

Use docker-compose-local.yml to build the development version of your next app

```
docker compose -f docker-compose-local.yml build name
```

### For Production

This template uses Docker Buildx to create AMD64 + ARM64 images of your Next project.

_I would recommend only pushing your production ready services to docker hub._

**Don't push your secrets _especially keys_ to Dockerhub**

```
# To build locally use:

docker buildx bake -f docker-compose.build.yml --load

# append the "--push" flag to build + push to docker hub

docker buildx bake -f docker-compose.build.yml --push
```

If you don't want to use buildx you can use build instead
```
next:
  build:
    context: ${PWD}/directory
    dockerfile: Dockerfile.production
```

## To use this image

```
# For Development:
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build

# For Production:
docker compose -f docker-compose.yml -d --build

```

The "--build" flag will ensure that docker builds any unbuilt images & rebuild images that need updating. Although this flag is useful during development, it is not useful having docker rebuild your next development image constantly. So I would recommend keeping your next build in _docker-compose.build.yml_ and only building when you change the dockerfile.
