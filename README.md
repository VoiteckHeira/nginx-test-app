<h1>Technologie chmurowe - Zadanie 1</h1>
# Wojciech Dziedzic

# Część 1 (Obowiązkowa)
## 1.1

'''

'''
## 1.2

'''
# syntax=docker/dockerfile:1.4

FROM --platform=$BUILDPLATFORM node:alpine3.16 as build-stage 

ARG VERSION
ARG TARGETPLATFORM
ARG BUILDPLATFORM

WORKDIR /app 

ENV REACT_APP_VERSION=${VERSION}

COPY package*.json /app/ 

RUN npm install axios && npm install && rm -rf $npm_config_cache && npm cache clean --force

COPY . . 

RUN npm run build


FROM nginx:1.23.4-alpine

ARG VERSION

LABEL org.opencontaine rs.image.version="$VERSION"
LABEL org.opencontainers.image.authors="Wojciech Dziedzic"

COPY nginx.conf /etc/nginx/conf.d/default.conf 

COPY --from=build-stage /app/build /usr/share/nginx/html 

EXPOSE 8080

HEALTHCHECK --interval=10s --timeout=1s \
    CMD curl -f http://localhost:8080/ || exit 1

CMD ["nginx", "-g", "daemon off;"]

'''

## 1.3



 # Część 2 (Dodatkowa)
