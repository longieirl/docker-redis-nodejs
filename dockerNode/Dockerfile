# DOCKER-VERSION 0.10.0

# Pull base image
FROM dockerfile/ubuntu

# Make sure apt is up to date
# Install nodejs and npm
RUN \
	env && \
	apt-get update && \
	apt-get install -y nodejs npm git git-core && \ 
	PATH=/usr/bin/node:$PATH

# Bundle app source
ADD src /opt/app/

# Use changes to package.json to force Docker not to use the cache
# when we change our application's nodejs dependencies:
ADD src/package.json /tmp/package.json
RUN cd /tmp && npm install && \
	mkdir -p /opt/app && cp -a /tmp/node_modules /opt/app/ && \
	cp -a /tmp/node_modules /opt/app/redis

EXPOSE  9191
CMD ["nodejs", "/opt/app/redis/app.js"]