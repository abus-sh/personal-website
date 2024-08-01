FROM node:20.14

RUN ["npm", "install", "-g", "typescript"]
RUN ["npm", "install", "-g", "@angular/cli"]

WORKDIR "/app"

# Copy files for building
COPY "./tsconfig.json" "."
COPY "./package.json" "."
COPY "./package-lock.json" "."
COPY "./tsconfig.app.json" "."
COPY "./tsconfig.spec.json" "."
COPY "./angular.json" "."

RUN ["npm", "install", "--save-prod"]

RUN --mount=type=bind,source="./src",target="/app/src" ["ng", "build"]

FROM nginx:1.27

COPY --from=0 --chown=nginx:nginx "/app/dist/abus.sh/browser" "/app"

RUN ["apt", "update"]
RUN ["apt", "install", "-y", "cron"]
RUN ["apt", "install", "-y", "vim"]
RUN ["bash", "-c", "curl https://get.acme.sh | sh -s"]