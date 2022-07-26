# microservices
specific repo for microservices to exchange with partner

This microservice provides tidal info based on Station ID, which can be found here https://tidesandcurrents.noaa.gov/stations.html?type=Harmonic+Constituents, and date/time. 

The user/client can either: 
  1) provide a Station ID and a specific date/time or 
  2) provide a Station ID and -1 for either day, month, year, hour, or minute in which case the current date/time of the server would be used

Station ID, day, month, year, hour, and minute must be integers  

This microservice is using ZeroMQ as a library for communication

## how to REQUEST data
***Ex 1 - Station ID and a specific date/time***


import zmq


context = zmq.Context()

socket = context.socket(zmq.REQ)

socket.connect("tcp://localhost:5555")


station_id_var = 8537121

month = 10                      #must be from 1 to 12 or -1

day = 1                         #must be from 1 to 31 or -1

year = 22                       #must be in a 2 digit format or -1

hour = 23                       #must be from 0 to 23 or -1

minute = 30                     #must be from 0 to 59 or -1

request = str(str(station_id_var) + "," + str(month) + "," + str(day) + "," + str(year) + "," + str(hour) + "," + str(minute))

socket.send(request.encode())


***Ex 2 - Station ID and current date/time***


import zmq


context = zmq.Context()

socket = context.socket(zmq.REQ)

socket.connect("tcp://localhost:5555")


station_id_var = 8537121

month = 10                      #must be from 1 to 12 or -1

day = -1                         #must be from 1 to 31 or -1

year = 22                       #must be in a 2 digit format or -1

hour = 23                       #must be from 0 to 23 or -1

minute = 30                     #must be from 0 to 59 or -1


request = str(str(station_id_var) + "," + str(month) + "," + str(day) + "," + str(year) + "," + str(hour) + "," + str(minute))

socket.send(request.encode())

## how to RECEIVE data

message = socket.recv().decode()

message may look like the following 

>start_time_tide,2022-07-25 00:18:00,1.437,receding

the client then can use message.split(",") to break the string into a list and use the values of interest

## UML Sequence Diagram (simplified)
![UML 361](https://user-images.githubusercontent.com/108242862/180926010-9093fa7b-e51f-4584-8868-e07b8b5b381d.png)

