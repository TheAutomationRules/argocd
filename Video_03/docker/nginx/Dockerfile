ARG FROM_IMG
ARG FROM_VER
FROM $FROM_IMG:$FROM_VER
ARG VERSION

LABEL org.opencontainers.image.authors="The Automation Rules <theautomationrules@gmail.com>" \
      org.opencontainers.image.url="http://www.theautomationrules.com/" \
      org.opencontainers.image.title="custom-nginx" \
      org.opencontainers.image.description="Docker Image to deploy a Custom NGiNX Container"

# CREATE CUSTOM NGINX INDEX
RUN echo '<body style="font-family: helvetica"><div style="width: 100%; margin-top:20px; text-align: center"><img style="width: 200px; border-radius:50%; margin-bottom: 20px" src="https://avatars.githubusercontent.com/u/95985608?v=4"></div><h2 style="text-align: center">Docker Container</br><strong style="color: green">NGiNX</strong></br>version: '$VERSION'</h2><body>' > /usr/share/nginx/html/index.html