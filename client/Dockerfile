FROM node:16-alpine as builder

USER node
WORKDIR '/app'

# Set permissions for the /app directory
RUN mkdir -p /app && chown -R node:node /app

COPY --chown=node:node ./package.json ./

# Switch to root user temporarily for npm install
USER root
RUN npm install

# Switch back to the node user
USER node

COPY --chown=node:node ./ ./

# Ensure the 'build' directory is owned by the 'node' user
RUN mkdir -p /app/build && chown -R node:node /app/build

# Run the build command
RUN npm run build


FROM  nginx
EXPOSE 3000
COPY --chown=node:node ./nginx/defaul.conf /etc/nginx/conf.d/default.conf
COPY --from=builder --chown=node:node ./app/build /usr/share/nginx/html