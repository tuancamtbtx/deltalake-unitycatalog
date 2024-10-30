# Use Debian Bullseye Slim as the base image.
FROM debian:bullseye-slim as download

# Set the working directory
WORKDIR /app

# Define UC_VERSION
ARG UC_VERSION

# Limit the memory usage of the sbt process to 2GB
ENV SBT_OPTS="-Xmx2G"

RUN apt-get update && apt-get install -y \
    curl \
    bash \
    unzip \
    wget \
    openjdk-11-jdk \
    && rm -rf /var/lib/apt/lists/*

RUN wget https://github.com/unitycatalog/unitycatalog/archive/refs/tags/v${UC_VERSION}.zip \
    && unzip v${UC_VERSION}.zip \
    && mv unitycatalog-${UC_VERSION} unitycatalog \
    && rm v${UC_VERSION}.zip


FROM node:20.0-alpine as target

WORKDIR /opt/app
COPY --from=download /app/unitycatalog/ui/ .
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the rest of the application code
COPY . .
RUN npm run build

# Listen to port 3000
EXPOSE 3000
# Run the server
CMD ["npm start"]