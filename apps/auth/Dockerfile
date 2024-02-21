# Use base node image for development stage
FROM node:alpine as development

# set the working directory
WORKDIR /usr/src/app

# copy the package.json and pnpm-lock.yaml files to WORKDIR
COPY package.json ./
COPY pnpm-lock.yaml ./

# install pnpm globally
RUN npm install -g pnpm

# install development and production dependencies
RUN pnpm install

# copy the source code to WORKDIR
COPY . .

# build the stage app  with all dependencies
RUN pnpm run build

# Use base node image for production stage
FROM node:alpine as production

# define a variable to be used on docker build
# variable with an optional default value that users can override at build-time when using docker build.
ARG NODE_ENV=production

# set the NODE_ENV to the value of the variable 
ENV NODE_ENV=${NODE_ENV}

# set the working directory 
WORKDIR /usr/src/app

# copy the package.json and pnpm-lock.yaml files to WORKDIR
COPY package.json ./
COPY pnpm-lock.yaml ./

# install pnpm globally
RUN npm install -g pnpm

# install only the production dependencies
# which will make the production image much lighter
RUN pnpm install --prod

# copy the dist from development to production
COPY --from=development /usr/src/app/dist ./dist

CMD ["node", "dist/apps/auth/main"]