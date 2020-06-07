FROM debian
ADD debian.list /etc/apt/sources.list.d/
RUN apt-get update && apt-get -y upgrade && apt -y install nginx && apt-get clean && \
    cd /var/www/ && rm -rf ./* && \
    mkdir -p nik1810.com/img && \
    chmod -R 754 /var/www/nik1810.com/ && \
    useradd Nikita && groupadd Sarazhinskiy && usermod -aG Sarazhinskiy Nikita && \
    chown -R Nikita:Sarazhinskiy /var/www/nik1810.com/ && \
    sed -i 's/\/var\/www\/html/\/var\/www\/nik1810.com/g' /etc/nginx/sites-enabled/default && \
    sed -i 's/user www-data/user Nikita/g' /etc/nginx/nginx.conf
ADD index.html /var/www/nik1810.com/
ADD img.png /var/www/nik1810.com/img/
CMD ["nginx", "-g", "daemon off;"]