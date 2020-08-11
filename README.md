# angulardocker


docker build -t ng-client .

docker run -p 4200:4200 ng-client


docker-compose up


My Dockerfile looks like this:
# based on https://mherman.org/blog/dockerizing-an-angular-app/
# base image
FROM node:8.9.3

# install chrome for protractor tests
# RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -
# RUN sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'
# RUN apt-get update && apt-get install -yq google-chrome-stable

# set working directory
RUN mkdir /usr/src/app
WORKDIR /usr/src/app

# add `/usr/src/app/node_modules/.bin` to $PATH
ENV PATH /usr/src/app/node_modules/.bin:$PATH

# install and cache app dependencies
COPY package.json /usr/src/app/package.json
RUN npm install

# add app
COPY . /usr/src/app

EXPOSE 4200

# start app
CMD ng serve --host 0.0.0.0




And docker-compose.yml looks like this:


version: '3'
services:
  server:
    build: ./server
    ports:
    - '3000:3000'
  # db:
  #   build: mongo:4.1
  #   ports:
  #   - '27017:27017'
  client:
    build: ./client
    ports:
    - '4200:4200'
