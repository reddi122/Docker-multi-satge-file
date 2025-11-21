# ====== Stage 1: Build React App ======
FROM node:20 AS build
WORKDIR /app

# Install git (if you want to clone repo)
RUN apt-get update && apt-get install -y git && rm -rf /var/lib/apt/lists/*

# Clone repo (optional)
RUN git clone https://github.com/<username>/<repository>.git .

# Copy package.json and package-lock.json first for caching
COPY package*.json ./
RUN npm install

# Copy source code and build
COPY . .
RUN npm run build

# ====== Stage 2: Serve with Nginx ======
FROM nginx:alpine
WORKDIR /usr/share/nginx/html

# Remove default nginx static files
RUN rm -rf ./*

# Copy build output from Stage 1
COPY --from=build /app/build ./

# Expose port
EXPOSE 80

# Start Nginx
CMD ["nginx", "-g", "daemon off;"]
