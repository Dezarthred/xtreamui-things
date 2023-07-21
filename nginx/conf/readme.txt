this nginx.conf file is prepared to use with certbot ssl certificates

challange method is standalone, it will use 80 port.

sudo apt-get update
sudo apt-get install software-properties-common
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot 

sudo certbot certonly --standalone --preferred-challenges http-01 -d  yourdomain.com 


also use wget command to get dhparam from mozilla

wget --no-check-certificate "https://ssl-config.mozilla.org/ffdhe4096.txt" -O /home/xtreamcodes/iptv_xtream_codes/nginx/conf/dhparam.pem



for main server,

wget "https://github.com/Dezarthred/xtreamui-things/raw/master/nginx/conf/nginx.conf" -O /home/xtreamcodes/iptv_xtream_codes/nginx/conf/nginx.conf

since r22d, developer added isp api, it is an experimentle api. uncomment the "include nginx_isp_api.conf;" in nginx.conf file.
but with this you need to use certbot webroot challenge method.

wget "https://github.com/Dezarthred/xtreamui-things/raw/master/nginx/conf/nginx_isp_api.conf" -O /home/xtreamcodes/iptv_xtream_codes/nginx/conf/nginx_isp_api.conf


mkdir -p /var/www/_letsencrypt
chown xtreamcodes /var/www/_letsencrypt

sudo certbot certonly --preferred-challenges http-01 --webroot -w /var/www/_letsencrypt -d yourdomain.com --email info@yourdomain.com -n --agree-tos --force-renewal




also enable https streaming in panel with adding server id to settings > use_https data like this with a mysql querry.

UPDATE `xtream_iptvpro`.`settings` set `use_https` = '["1","2","3",....,"xyz"]' where `id` = 1;


echo -n "deploy-hook = /home/xtreamcodes/iptv_xtream_codes/nginx/sbin/nginx -s reload" >> /etc/letsencrypt/cli.ini


clear nginx logs every 3 hours

0 */3 * * * echo "" > /home/xtreamcodes/iptv_xtream_codes/logs/main.error.log && echo "" > /home/xtreamcodes/iptv_xtream_codes/logs/main.access.log

