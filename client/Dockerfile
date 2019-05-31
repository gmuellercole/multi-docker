##########  SECTION 1 #######
FROM node:alpine as builder
WORKDIR '/app'
COPY ./package.json ./
RUN npm install
COPY . .

#emits runtime content to the build folder
RUN npm run build

##########  SECTION 2 #######

# inline FROM delineates the start of a second phase
FROM nginx

# inform Elastic Bean Stalk that we want port 3000 open (and mapped to port 3000)
EXPOSE 3000

# overwrite the default.conf file with the one containing our configuration
COPY ./nginx/default.conf /etc/nginx/conf.d/default.conf

# the first section emits a bunch of artifacts (to the /app/build folder)
# copy them to nginx's location (we know the location bc we read the nginx
# image's documentation at docker hub).
COPY --from=builder /app/build /usr/share/nginx/html

# we don't need to specify an explicit startup command bc the nginx
# image starts nginx by default