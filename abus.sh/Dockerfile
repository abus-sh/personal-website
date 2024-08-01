FROM node:20.14

# Build the Angular site, then copy the build to the nginx section

RUN ["npm", "install", "-g", "typescript"]
RUN ["npm", "install", "-g", "@angular/cli"]

WORKDIR "/data"

# Copy files for building
COPY "./tsconfig.json" "."
COPY "./package.json" "."
COPY "./package-lock.json" "."
COPY "./tsconfig.app.json" "."
COPY "./tsconfig.spec.json" "."
COPY "./angular.json" "."
#COPY "./public" "./public"

RUN ["npm", "install", "--save-prod"]

RUN --mount=type=bind,source="./src",target="/data/src" \
    --mount=type=bind,source="./public",target="/data/public" \
    ["ng", "build"]

#CMD ["tail", "-f", "/dev/null"]

FROM nginx:1.27

COPY --from=0 --chown=nginx:nginx "/data/dist/abus.sh/browser" "/data/abus.sh"

RUN ["apt", "update"]
RUN ["apt", "install", "-y", "cron"]
RUN ["bash", "-c", "curl https://get.acme.sh | sh -s"]