# Use the do288-httpd-parent image as base
FROM docker.io/rallred/do288-httpd-parent

# DocumentRoot for Apache
ENV DOCROOT=/var/www/html 


RUN   yum install -y --nodocs --disableplugin=subscription-manager httpd && \
      yum clean all --disableplugin=subscription-manager -y && \
      echo "Hello from the httpd-parent container!" > ${DOCROOT}/index.html

# Allows child images to inject their own content into DocumentRoot

ONBUILD COPY src/  /var/www/html

RUN sed -i "s/Listen 80/Listen 8080/g" /etc/httpd/conf/httpd.conf

# This stuff is needed to ensure a clean start
RUN chgrp -R 0 /var/log/httpd /var/run/httpd
RUN chmod -R g=u /var/log/httpd /var/run/httpd

# Run as the root user
USER 1001

EXPOSE 8080
# Launch httpd
CMD /usr/sbin/httpd -DFOREGROUND
