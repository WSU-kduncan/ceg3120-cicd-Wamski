FROM node:18-bullseye

COPY /angular-site /app

WORKDIR /app

RUN npm install
RUN npm install -g @angular/cli

CMD ng serve --host 0.0.0.0

