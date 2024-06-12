# ngmix
sudo apt update sudo apt upgrade 
Commands Used
STEP 1
sudo apt update
sudo apt install libnginx-mod-rtmp
sudo nano /etc/nginx/nginx.conf
WRITE IN
rtmp {
        server {
                listen 1935;
                chunk_size 4096;
                allow publish 127.0.0.1;
                deny publish all;

                application live {
                        live on;
                        record off;
                }
        }
}

sudo ufw allow 1935/tcp
sudo systemctl reload nginx.service

STEP 2
sudo apt install python3-pip
sudo pip install youtube-dl
youtube-dl address -f mp4
sudo apt install ffmpeg
ffmpeg -re -i "VIDEO NAME" -c:v copy -c:a aac -ar 44100 -ac 1 -f flv rtmp://localhost/live/stream
--------------------------------------
Nginx RTMP is a TCP-based convention intended to keep up low-dormancy associations for sound and video spilling. To expand the measure of information that can be easily transmitted, streams are part into littler sections called parcels. RTMP additionally characterizes a few virtual channels that work autonomously of one another for bundles to be conveyed on. This implies video and sound are conveyed on discrete channels all the while, To get more information click here to visit the official website.

Install Dependencies
To install Nginx RTMP server on ubuntu use the following commands with using root privileges.

sudo -i

apt-get update

apt-get install ffmpeg libpcre3 unzip libssl-dev build-essential libpcre3-dev -y

Download Nginx and RTMP Modules
To download the nginx and rtmp module use the following commands.

cd /tmp

wget https://github.com/arut/nginx-rtmp-module/archive/master.zip

wget http://nginx.org/download/nginx-1.14.0.tar.gz

Extract and Compile the Nginx with RTMP module
After downloaded nginx with RTMP module, You need to extract and unzip the master.zip packages and compile the nginx rtmp module by following the commands.

tar -zxvf nginx-1.14.0.tar.gz

unzip master.zip

cd nginx-1.14.0

./configure --with-http_ssl_module --add-module=../nginx-rtmp-module-master

make

make install

Configuration of Nginx Daemon
To control nginx daemon, We need to download the pre-define the service of nginx and make executable it by following the commands.

cd /tmp

wget https://raw.github.com/JasonGiedymin/nginx-init-ubuntu/master/nginx -O /etc/init.d/nginx

chmod +x /etc/init.d/nginx

On boot enable nginx service
If you want start the nginx service on boot the server use the following commands.

update-rc.d nginx defaults

Configuration of RTMP protocal in nginx config file by using the following the commands, Create a backup file of nginc.conf and than edit the original nginx.conf

cp -p /usr/local/nginx/conf/nginx.conf nginx.conf_backup

Open the nginx.conf with nano editor .

nano /usr/local/nginx/conf/nginx.conf

Add the following configuration.
#user nobody;

worker_processes 1;

error_log logs/rtmp_error.log debug;

pid logs/nginx.pid;

events {

worker_connections 1024;

}

http {

server {

listen 80;

server_name localhost;

location /hls {

# Serve HLS fragments

# CORS setup

add_header 'Access-Control-Allow-Origin' '*' always;

add_header 'Access-Control-Expose-Headers' 'Content-Length';

# allow CORS preflight requests

if ($request_method = 'OPTIONS') {

add_header 'Access-Control-Allow-Origin' '*';

add_header 'Access-Control-Max-Age' 1728000;

add_header 'Content-Type' 'text/plain charset=UTF-8';

add_header 'Content-Length' 0;

return 204;

}



types {

application/vnd.apple.mpegurl m3u8;

video/mp2t ts;

}

root /tmp;

add_header Cache-Control no-cache;

}

}

}

rtmp {

server {

listen 1935;

chunk_size 8192;

application hls {

live on;

meta copy;

hls on;

hls_path /tmp/hls;

}

}

}

Save and Exit from nano editor.

Create required directory for Stream and recording data by following the commands.

mkdir /HLS

mkdir /HLS/live

mkdir /HLS/mobile

mkdir /video_recordings

chmod -R 777 /video_recordings

Restart the nginx service

systemctl restart the nginx.service

Update the UFW firewall

If you have enable ufw firewall so then you need to allow the port 80 and port 1935 for rtmp protocol to access from the network, Use the following commands to open port 80 and port 1935.

ufw allow 80

ufw allow 1935

ufw status

Check the nginx's RTMP service using netstate commands.

netstate -plntu | grep 1935

Now you can stream with any key using OBS and Webcam, Use the given details.

 rtmp://localhost/hls

To view the live HLS stream open you online player and enter the given url.
------------------------------------------------------
https://www.infinitivehost.com/knowledge-base/how-to-install-nginx-rtmp-media-server/
https://gist.github.com/xiCO2k/e378be37e933b3f5c6c106ba149f34af
https://obsproject.com/forum/resources/how-to-set-up-your-own-private-rtmp-server-using-nginx.50/
https://adamtheautomator.com/nginx-rtmp/
https://www.digitalocean.com/community/tutorials/how-to-set-up-a-video-streaming-server-using-nginx-rtmp-on-ubuntu-20-04
https://www.servermania.com/kb/articles/nginx-rtmp
https://hlsbook.net/hls-nginx-rtmp-module/
