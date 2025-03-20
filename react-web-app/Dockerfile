# Base Image - Build Stage
FROM node:20-alpine AS builder

# Working Directory
WORKDIR /myapp

# Copy package.json file
COPY package*.json .

# Install only the dependencies present in package.josn file 
RUN npm install

# Copy the Code from Machine to Container
COPY . .

# Create a build folder of final static project
RUN npm run build

# Base Image - Production Build
FROM nginx:1.23-alpine as production

# Working Directory of nginx (From where html file is hosted)
WORKDIR /usr/share/nginx/html

# Remove all current files from the folder
RUN rm -rf ./*

# Copy build folder files to /usr/share/nginx/html
COPY --from=builder /myapp/build .

# Run nginx
ENTRYPOINT [ "nginx", "-g", "daemon off;" ]


# To create an image from this file run the below command in shell
# docker build -t react-app:multi-stage .
# DockerFile will fetch node:20-alpine and nginx:123_alpine image form docker.io

# [builder 1/6] FROM docker.io/library/node:20-alpine
# => [production 1/4] FROM docker.io/library/nginx:1.23-alpine
# => [production 2/4] WORKDIR /usr/share/nginx/html                                                                                                    0.4s
# => [production 3/4] RUN rm -rf ./*                                                                                                                   0.9s
# => [builder 2/6] WORKDIR /myapp                                                                                                                   0.2s
# => [builder 3/6] COPY package*.json .                                                                                                             0.1s
# => [builder 4/6] RUN npm install                                                                                                                 40.0s
# => [builder 5/6] COPY . .                                                                                                                         0.1s
# => [builder 6/6] RUN npm run build                                                                                                               25.8s
# => [production 4/4] COPY --from=builder /myapp/build .                                                                                               0.1s
# => exporting to image                                                                                                                             0.2s
# => => exporting layers                                                                                                                            0.1s
# => => writing image xxxxx
# => => naming to docker.io/library/react-app:multi-stage 