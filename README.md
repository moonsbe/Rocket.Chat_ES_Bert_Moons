# Rocket.Chat_ES_Bert_Moons

The server can be found at http://rocket.best-it.be/
You should have your personal login codes, otherwise you can contact bertmoons@gmail.com

There is one installation done with Snapd and one with Docker

Docker: 

 

 

sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-Linux-x86_64" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose

 

sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose

sudo apt-get install nginx
 

sudo snap install core; sudo snap refresh core
sudo apt-get remove certbot
sudo snap install --classic certbot
sudo certbot --nginx
Last command not yet working
sudo nano /etc/nginx/certificate.key

curl -L https://raw.githubusercontent.com/RocketChat/Rocket.Chat/develop/docker-compose.yml -o docker-compose.yml

sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
 

sudo mkdir -p /var/www/rocket.chat/data/runtime/db
sudo mkdir -p /var/www/rocket.chat/data/dump

Update the docker-compose.yml file according to the documentation
Sudo docker-compose up -d

Test the solution at http://18.221.179.40:3000/


Sudo vi /etc/init/rocketchat_mongo.conf

description "MongoDB service manager for rocketchat"
# Start MongoDB after docker is running
start on (started docker)
stop on runlevel [!2345]
# Automatically Respawn with finite limits
respawn
respawn limit 99 5
# Path to our app
chdir /var/www/rocket.chat
script
# Showtime
exec /usr/local/bin/docker-compose up mongo
end script

sudo vi /etc/init/rocketchat_app.conf

description "Rocketchat service manager"
# Start Rocketchat only after mongo job is running
start on (started rocketchat_mongo)
stop on runlevel [!2345]
# Automatically Respawn with finite limits
respawn
respawn limit 99 5
# Path to our app
chdir /var/www/rocket.chat
script
# Bring up rocketchat app and hubot
exec /usr/local/bin/docker-compose up rocketchat hubot
end script

After rebooting the server

 ![image](https://user-images.githubusercontent.com/3437364/125816060-a57aa8f2-d828-4fa7-b243-c57424a8b082.png)


sudo certbot --nginx

![image](https://user-images.githubusercontent.com/3437364/125815926-7233cad7-6418-41c1-b150-3965c2b075ce.png)



Snapd: 
For the installation, I chose for a a freetier AWS instance
![image](https://user-images.githubusercontent.com/3437364/125083041-2f810e00-e0c8-11eb-8531-11a7ff4ac91a.png)

On the AWS instance, I choose for the Snaps installation as this was the one I wasn't familiar with yet. 
I followed the commands https://docs.rocket.chat/installing-and-updating/snaps

The total time for the AWS setup and Rocket server installation took 30 minutes, see the timings here 
![image](https://user-images.githubusercontent.com/3437364/125083452-b504be00-e0c8-11eb-8494-2cf51eaf9e25.png)
![image](https://user-images.githubusercontent.com/3437364/125083486-c2ba4380-e0c8-11eb-828e-f240b644a2df.png)


API calls

Create user

curl -H "X-Auth-Token: 08eLCuzNKDNTSAbCZ9laoQuDLrXtD_ZrVuG6zmlcc0t" -H "X-User-Id: weK4NgDHnM3QBvGa4" -H "Content-type: application/json" http://18.118.31.47:8080/api/v1/users.create -d '{"name": "Bond", "email": "007@mi6.com", "password": "YouRock", "username": "MrBond"}'

Result

{"user":{"_id":"vyRQF8KyETQKzwcWy","createdAt":"2021-07-09T13:04:19.321Z","username":"MrBond","emails":[{"address":"007@mi6.com","verified":false}],"type":"user","status":"offline","active":true,"_updatedAt":"2021-07-09T13:04:19.481Z","__rooms":["GENERAL"],"roles":["user"],"name":"Bond","settings":{}},"success":true}

Get Rooms
curl -H "X-Auth-Token: 08eLCuzNKDNTSAbCZ9laoQuDLrXtD_ZrVuG6zmlcc0t" -H "X-User-Id: weK4NgDHnM3QBvGa4" -H "Content-type: application/json" http://18.118.31.47:8080/api/v1/rooms.get

 

Result

{"update":[{"_id":"GENERAL","ts":"2021-07-03T07:20:33.949Z","t":"c","name":"general","usernames":[],"usersCount":2,"default":true,"_updatedAt":"2021-07-09T07:23:58.935Z"}],"remove":[],"success":true}

 

Get Rooms Info General Room
curl -H "X-Auth-Token: 08eLCuzNKDNTSAbCZ9laoQuDLrXtD_ZrVuG6zmlcc0t" -H "X-User-Id: weK4NgDHnM3QBvGa4" -H "Content-type: application/json" http://18.118.31.47:8080/api/v1/rooms.info?roomId=GENERAL

 

Result

{"room":{"_id":"GENERAL","ts":"2021-07-03T07:20:33.949Z","t":"c","name":"general","usernames":[],"msgs":2,"usersCount":2,"default":true,"_updatedAt":"2021-07-09T07:23:58.935Z"},"success":true}

Get users in Role
curl -H "X-Auth-Token: 08eLCuzNKDNTSAbCZ9laoQuDLrXtD_ZrVuG6zmlcc0t" -H "X-User-Id: weK4NgDHnM3QBvGa4" -H "Content-type: application/json" http://18.118.31.47:8080/api/v1/roles.getUsersInRole



Result

{"success":false,"error":"Query param \"role\" is required [error-param-not-provided]","errorType":"error-param-not-provided"}



Documentation from this api call is not up to date https://developer.rocket.chat/api/rest-api/endpoints/roles/getusersinrole as I have to give the role names and in the documetnation it's mentioned that we also get an overall list without parameters 
