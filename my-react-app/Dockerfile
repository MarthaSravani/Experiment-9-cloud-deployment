# --- STAGE 1: The "Build" Stage ---
# We use a full Node.js image to build our app
FROM node:18-alpine AS build

# Set the working directory inside the container
WORKDIR /app

# Copy package.json and package-lock.json
# We copy these first to take advantage of Docker's layer caching
COPY package.json package-lock.json ./

# Install all dependencies
RUN npm install

# Copy the rest of our app's source code
COPY . .

# Run the React build script
RUN npm run build

# --- STAGE 2: The "Production" Stage ---
# We use a tiny Nginx image to serve the built files
FROM nginx:1.25-alpine

# Copy the Nginx configuration file we created
# This will tell Nginx how to handle React routing
COPY nginx/default.conf /etc/nginx/conf.d/default.conf

# Copy the built, static files from the "build" stage
# This is the magic of multi-stage builds!
COPY --from=build /app/build /usr/share/nginx/html

# Tell Docker that our container listens on port 80
EXPOSE 80

# The default command to start Nginx
CMD ["nginx", "-g", "daemon off;"]

