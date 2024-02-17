FROM node:16-alpine as builder

USER node
RUN mkdir -p /home/node/app
WORKDIR '/app'

# Copy only the package.json first to leverage Docker cache
COPY --chown=node:node ./package.json ./
RUN npm install

# Copy the rest of the application
COPY --chown=node:node ./ ./

# Ensure the 'build' directory is owned by the 'node' user
RUN mkdir -p /app/build && chown -R node:node /app/build

# Run the build command
RUN npm run build


FROM  nginx
EXPOSE 3000
COPY --chown=node:node ./nginx/defaul.conf /etc/nginx/conf.d/default.conf
COPY --from=builder --chown=node:node ./app/build /usr/share/nginx/html