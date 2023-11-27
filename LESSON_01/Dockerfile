# >>>> instructions during image creation

# base-image defaults to latest if not specified -> node:latest
FROM node

WORKDIR /app
# now everything is relative to /app 
# so for the next command we could also use COPY . ./ or COPY . .

# copy everything what's inside the current host folder to /app
COPY . /app

# RUN is the command used during image creation
# in this case RUN calls npm install so that all the dependencies for the 
# project that are listed in 'package.json' are gathered and installed by node.js
RUN npm install


# >>>> instructions during container startup

# documentation that port 80 inside container will be exposed
# the actual mapping is done when executing the 'docker run' command.
# there you add the '-p' flag (p for publish) specifing host-to-container-port-mapping
# > docker run -p 3434:80 <image-sha1 or name>
# so the following statement is optional but best practice
EXPOSE 80

CMD ["node", "server.js"]