# Rocket.Chat_ES_Bert_Moons

The server can be found at http://rocket.best-it.be/
You should have your personal login codes, otherwise you can contact bertmoons@gmail.com

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
