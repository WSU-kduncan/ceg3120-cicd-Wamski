FROM node:18-bullseye

RUN npm install -g @angular/cli

COPY /angular-site /angular-site

WORKDIR /angular-site

RUN ng serve --host 0.0.0.0