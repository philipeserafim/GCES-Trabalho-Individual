FROM node:lts-alpine

WORKDIR /library_front
ENV PATH /library_font/node_modules/.bin:$PATH

COPY package.json /library_front/package.json

RUN npm install
RUN npm install react-scripts@3.3.1 -g 

COPY . /library_front/

CMD [ "npm", "start" ]