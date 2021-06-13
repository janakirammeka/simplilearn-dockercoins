FROM node:4-slim
RUN apt-get update && apt-get install -y python build-essential
RUN npm install express 
RUN npm install redis
COPY files /src/files
COPY webui.js /src/
