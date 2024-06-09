FROM quay.io/americanfirstfinance/ubi8-minimal-nodejs:latest as builder
ARG CONT_INST
RUN echo $CONT_INST
WORKDIR /app
COPY . .
RUN npm cache clean --force && npm ci && npm run ${CONT_INST}
CMD useradd -ms /bin/bash nginx && \
 groupadd -ms /bin/bash nginx && \
 chown -R nginx:nginx .

FROM quay.io/americanfirstfinance/ubi8-nginx:latest
WORKDIR /usr/share/nginx/html
RUN rm -rf /usr/share/nginx/html/*
COPY --from=builder /app/dist/apps/c2s-signature .
COPY --from=builder /app/nginx.conf /etc/nginx/nginx.conf
RUN chmod -R 777 /etc/nginx/nginx.conf
RUN chmod -R 777 /usr/share/nginx/html/assets/ui
RUN microdnf update -y
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
