# Deploy static web service
## Method 1 - Use `docker run` command
confirm current directory: `01.static-web/`
`docker run -p 80:80 --name my-first-web -v ./static::usr/share/nginx/html -d nginx`

## Method 2 - Use `Dockerfile`
create `Dockerfile` in  `01.static-web/` if not exist.
```
FROM nginx:latest

COPY ./static /usr/share/nginx/html

EXPOSE 80
```

## Method 3 - Use `docker-compose`
### 3.1 create with official nginx image.

### 3.2 create with image by `Dockerfile`