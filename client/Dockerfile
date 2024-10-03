# from node image angular app as "web-build" img with tag 14.
# the working dir will be /usr/src/app in remote machine
# copy the entire source code from local machine to /usr/src/app/client in remote machine
# in the run instruction: go to client/ and run npm install 
# the artifact will be built and store in this client/ => we need to copy to nginx 
FROM node:14 AS web-build  
WORKDIR /usr/src/app
COPY ./ ./client
RUN cd client && npm install && npm run build --prod

# Use official nginx image as the base image - web app
FROM nginx:latest

# Copy the build output to replace the default nginx contents.
# /usr/src/app is the path from the build image step, in there we have /dist/client/ - where we will have the artifact. The content in that will be copied to /usr/share/ngin/html in nginx
# Also copy 'nginx.conf' customized file from the local machine into /etc/nginx/conf.d/default.conf
COPY --from=web-build /usr/src/app/client/dist/client/ /usr/share/nginx/html
COPY nginx.conf /etc/nginx/conf.d/default.conf

# Expose port 4200
EXPOSE 4200
