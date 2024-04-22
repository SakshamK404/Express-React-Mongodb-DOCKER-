
In the project's root directory, you'll come across the DockerFile snippet tailored for the backend (Node.js) setup.
#### Snippet of backend(Node.js)`DockerFile`

FROM node:13.13.0-stretch-slim
# We define an argument, NODE_PORT, which will be supplied via docker-compose.
ARG NODE_PORT
# Verifying if the supplied argument is correctly loaded.
RUN echo "The provided port argument is: $NODE_PORT"
# Establishing the working directory for the application.
WORKDIR /usr/src/app
# Copying the project contents into the container's working directory.
COPY . .
# Installing the necessary dependencies for the application.
RUN npm install
# The exposed port corresponds to the one specified in the .env file, which is sourced via docker-compose.
EXPOSE ${NODE_PORT}
# Command to initiate the application within the container using npm run dev, which is defined in package.json's scripts.
CMD npm run dev

##### Explanation of backend(Node.js) `DockerFile`
Backend(Node.js) DockerFile Breakdown:

The initial directive instructs Docker to utilize a specific Node.js image fetched from DockerHub. Here, we opt for the official Node.js version 10 image.
Following that, we declare an argument named NODE_PORT, intending to receive it from docker-compose.
A logging statement on the subsequent line is inserted to validate the successful loading of the provided argument.
The fourth line simplifies the process of creating the working directory within the container, ensuring proper housing for the application code. 
The contents of the project are then transferred to the container's designated working directory. 
The application's dependencies are installed within the container using npm install. 
The port exposed by Docker during container runtime is configured based on the value defined in the.env file, which is dynamically sourced via docker-compose. 
Finally, Docker is told to run the application within the container using the command npm run dev, which is specified in package.json. 


