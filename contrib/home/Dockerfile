# Official InvenTree image + code from the fork (Bravo13/InvenTree, branch home/1.5.x).
# Build context = the fork checkout root (see inventree-deploy docker-compose.override.yml).
# BASE must match the tag the fork branch is based on.
ARG BASE=inventree/inventree:1.5.6

FROM node:24-bookworm-slim AS frontend
WORKDIR /src
COPY src/frontend src/frontend
RUN cd src/frontend \
    && corepack enable \
    && yarn install --frozen-lockfile --network-timeout 600000 \
    && yarn run compile \
    && yarn run build

FROM ${BASE}
# Backend source (only changed files matter; the copy just overlays the image)
COPY src/backend/InvenTree /home/inventree/src/backend/InvenTree
# Replace (not overlay) the bundled frontend, so old asset files don't pile up
RUN rm -rf /home/inventree/src/backend/InvenTree/web/static/web
COPY --from=frontend /src/src/backend/InvenTree/web/static/web /home/inventree/src/backend/InvenTree/web/static/web
