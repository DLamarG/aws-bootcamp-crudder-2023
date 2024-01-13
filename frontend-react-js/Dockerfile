# Use an official Node runtime as a parent image
FROM node:14

# Set the working directory in the container
WORKDIR /frontend-react

# Copy package.json and package-lock.json to the container
COPY package*.json ./

# Install dependencies
RUN npm install

# Copy the local code to the container
COPY . .

# Expose port 3000
EXPOSE 3000

# Start the application
CMD ["npm", "start"]