FROM ruby:3.3-slim AS build

RUN apt-get update \
    && apt-get install -y --no-install-recommends build-essential git \
    && rm -rf /var/lib/apt/lists/*

# github-pages bundles the same Jekyll and plugin versions as GitHub Pages
RUN gem install github-pages webrick --no-document

WORKDIR /site
COPY html/ .
COPY rfc/ rfc/
RUN jekyll build --destination /out

FROM nginx:alpine

ARG BUILD_DATE
ARG BUILD_REVISION
ARG BUILD_VERSION
ARG BUILD_ATTESTATION

LABEL org.opencontainers.image.title="rfc2612" \
      org.opencontainers.image.created="${BUILD_DATE}" \
      org.opencontainers.image.revision="${BUILD_REVISION}" \
      org.opencontainers.image.version="${BUILD_VERSION}"

COPY --from=build /out /usr/share/nginx/html

EXPOSE 80
