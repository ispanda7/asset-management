#ระบบ Asset-Management

วิธีการติดตั้งด้วย Docker

0.Config file my_env_file

1.Pull Image
docker pull snipe/snipe-it

2.Pull & Run Image Mysql
docker run \
 --name snipe-mysql \
 --env-file=my_env_file \
 --mount source=snipesql-vol,target=/var/lib/mysql -d -P mysql:5.6

3.Gen APP_KEY
docker run --rm snipe/snipe-it

จะได้ข้อความตัวอย่างประมาณนี้
Please re-run this container with an environment variable $APP_KEY
An example APP_KEY you could use is: 
base64:D5oGA+zhFSVA3VwuoZoQ21RAcwBtJv/RGiqOcZ7BUvI=

 นำ APP_KEY ที่ได้ไปใส่ใน file  my_env_file

4.Run Container snipeit ด้วย port 80
docker run \
 -d \
 -p 80:80 \
 --name="snipeit" \
 --link snipe-mysql:mysql \
 --env-file=my_env_file \
 --mount source=snipe-vol,dst=/var/lib/snipeit \
 --mount source=snipe-code,dst=/var/www/html \
 snipe/snipe-it
