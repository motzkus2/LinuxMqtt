Berry and the Whales



Thomas Motzkus, Jakob Spiel og Martin Jørgensen


3 Juni 2021

![](https://github.com/motzkus2/LinuxMqtt/blob/main/3.png)



# Indledning

I løbet af ugen skal vi finde på en løsning som kan erstatte ThingSpeak. Dette skal vi gøre ved hjælp af en Raspberry PI som kan holde på vores IT-løsning. IT-løsningen skal kunne kommunikere sammen vores klienter og fremvise data fra vores embedded målinger. Vores gruppe blev valgt til at lave IT-løsningen ved hjælp af docker container.


![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/4.jpg)



# Krav


#### Krav 1:

Det skal være muligt at indsende målinger til Databasen vha. MQTT.


#### Krav 2:

Dataen skal kunne hentes fra Databasen vha. Api.


#### Krav 3: 

Databasen skal kunne modtage 1 måling pr 10. sekund.


#### Krav 4: 

Der skal gøres brug af Docker på Raspberry Pi.


#### Krav 5:

Der skal laves en Container til API.


#### Krav 6:

Der skal laves en Container til Databasen.


#### Krav 7:

Der skal laves en Container til MQTT Brokeren.


#### Krav 8: 

Der skal gøres brug af en eller flere Klienter som kan fremvise målingerne i en graf for at brugeren nemmere kan holde overblik.


# Analyse


## Operativsystem

Kandidater



*   Raspberry pi OS
*   Ubuntu Server ARM

**Raspberry Pi OS**



*   Fordele
    *   Er fra producenten
    *   Er nem at sætte op
    *   Har et desktop miljø
*   Ulemper
    *   Har problemer med at køre docker.
    *   Fås kun som armhf lige nu

**Ubuntu server ARM**



*   Fordele
    *   Kører ARM64
    *   Er lavet til at køre containere
*   Ulemper
    *   Kræver kendskab til linux CLI

**Konklution**

Vi har valgt ubuntu server da den kører docker bedre og at den kører ARM64


## Database

Kandidater



*   InfluxDB
*   MySQL

**Influx DB**



*   Fordele
    *   Time series database
    *   Hurtig og let at køre
    *   SQL queries
*   Ulemper
    *   Sikkerhed out of box
    *   Lille community
    *   Ingen cluster mulighed.

**MySQL**



*   Fordele
    *   Effektiv brug af resources
    *   Egnet til websider
    *   Hurtigt
*   Ulemper
    *   Ikke egnet til “big data”
    *   Dårligt UI

**Konklusion**

Vi valgt at gå med InfluxDB for både at prøve noget nyt men efter have læst på det sku det egne sig meget godt til sådan nogen embedded målinger. Da vi også skal tænke på at vores system skal køre på en raspberry pi har vi fundet ud af Influxdb ikke bruger særlige mange resourcer og efter noget tid laver nogen averages på den data der ligger i dens database for at kunne gøre det lettere at køre når man har haft den oppe i en styk tid.


# Acceptance Test


### Forudsætning


<table>
  <tr>
   <td>Test Nr
   </td>
   <td>Krav
   </td>
   <td>Handling
   </td>
   <td>Forventet Resultat
   </td>
   <td>Godkendt
   </td>
  </tr>
  <tr>
   <td>1
   </td>
   <td>Krav 1
   </td>
   <td>Clone Projektet ned fra github: <a href="https://github.com/motzkus2/LinuxMqtt/blob/main/src/Shiftr.cpp">https://github.com/motzkus2/LinuxMqtt/blob/main/src/Shiftr.cpp</a>
<p>
Derefter sæt jeres MKR WIFI 1010 op som vist på billedet under embedded fysisk opsætning . Tjek derefter i koden at din Node Red har samme ip-adresse som i koden på linje 137. Er det ikke tilfældet ændre ip-adressen og køre programmet op på boardet.
   </td>
   <td>Efter have kørt programmet i 5min skulle du kunne tilgå din Klient, Grafana og influxDB og se at der kommet data ind i systemet.
   </td>
   <td>✔
   </td>
  </tr>
  <tr>
   <td>2
   </td>
   <td>Krav 2
   </td>
   <td>Clone Projektet ned fra github: <a href="https://github.com/Allf3/ApiBerryandtheWhales">https://github.com/Allf3/ApiBerryandtheWhales</a>
<p>
Derefter tjek Filen Constants/APIConstants, tjek at BaseApiUrl Stemmer overens med jeres ip-addresse. Køre derefter projektet. 
   </td>
   <td>Efter have kørt programmet skal der på første side dukke 2 grafer op som viser data frem.
   </td>
   <td>✔
   </td>
  </tr>
  <tr>
   <td>3
   </td>
   <td>Krav 3
   </td>
   <td>Efter have udført Test Nr 1 sender den embedded controller data op værd 10 sekund. gå ind på Grafana.  
   </td>
   <td>Inde på Grafana kontrollere på grafen at der bliver sendt data hvert 10'ende sekund.
   </td>
   <td>✔
   </td>
  </tr>
  <tr>
   <td>4
   </td>
   <td>Krav 4
<p>
Krav 5
<p>
Krav 6
<p>
Krav 7
   </td>
   <td>Forbind til raspberry Pi’en ved hjælp af Command Promt på en Windows maskine. Skriv: ssh ubuntu@Ip-Addresse, efter have forbundet skriv følgende kommando på raspberry pi’en, sudo docker ps -a
   </td>
   <td>Efter have kørt kommandoen kontrollere at alle Images er der:
<ul>

<li>grafana/grafana

<li>influxdb:2.0.6

<li>nodered/node-red

<li>eclipse-mosquitto

<li>
</li>
</ul>
   </td>
   <td>✔
   </td>
  </tr>
  <tr>
   <td>5
   </td>
   <td>Krav 8
   </td>
   <td>Åbne en browser som har adgang til samme Acces point som raspberry Pi’en.
<p>
Tjek følgende sidder:
<ul>

<li>Ip-Adresse:8086

<li>Ip-Adresse:3000

<li>Ip-Adresse:80
</li>
</ul>
   </td>
   <td>Tjek at du kan få adgang til alle siderne kan tilgåes. 
   </td>
   <td>✔
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
</table>



# 


# Proces og notater


## Dag 1 03-06-2021


### Installation af OS


#### Raspberry pi imager

Vi har downloadet raspberry pi imager og bruger programmet til at  flash vores sd kort, for at lægge den nyeste version af 64 bit Ubuntu server over på raspberry pi enheden. Programmet gør det nemt at finde alle OS man kan flash over på.

![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/unnamed.png)

### Håndtering af Docker container


#### Visual studio Code

Ved hjælp af en extension som hedder Remote - SSH kan vi nemmere skabe et overblik af vores docker container samt have noget ease of use med en brugergrænseflade frem for at arbejde direkte i terminalen på vores raspberry pi.


![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/2.png)



#### Eclipse Mosquitto

Her har vi hentet vores docker image ned ved hjælp af informationen på denne side: [https://hub.docker.com/_/eclipse-mosquitto](https://hub.docker.com/_/eclipse-mosquitto)

Derefter har vi været inde været inde og tilføje 2 linjer i vores container under mosquitto.config ved hjælp af vores visual studio extension Remote - SSH



*   listener 1883
*   allow_anonymous true

Her har vi været inde og tillade anonym adgang for at teste vores mosquito image virker, det har vi gjort med programmet MQTTX, hvor vi har tastet ip’en ind på vores Mosquitto container.

![alt text](https://github.com/motzkus2/LinuxMqtt/blob/main/5.png)


### 


## Dag 2 04-06-2021


#### Planlægning

Efter dag 1 har vi lagt en plan for hvordan vores system skal hænge sammen og det er kommet frem til et resultat vi ønsker at se sådan her ud og virker med det forskellige komponenter i forskellige docker containers.



![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/6.png)



#### Node red

Her har vi hentet et docker image som vi køre op ved hjælp af guiden på denne side: [https://nodered.org/docs/getting-started/docker](https://nodered.org/docs/getting-started/docker)

Efter have sat containeren op kan vi se det virke på vores IP-addresse: 192.168.42.12:1880.

Node red bruger vi til at sætte et “Flow” op til vores Database så vi kan styre hvad data der kommer ind.

Til sidst kommer vores flow til at se sådan her ud inde på siden:

![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/7.png)

#### Embeded

Her har vi taget det exemplaret vi har udarbejdet i skolen hvor vi sender 2 værdier Humidity og Temperatur til vores broker. Istedet for at sende den op til thingspeak sender vi vores data på ip-addressen: 192.168.42.12:1883. Dette er vores mosquitto container som fungere som vores broker nu.

![alt text](https://github.com/motzkus2/LinuxMqtt/blob/main/8.png)

Følge følgende fysisk opsætning

![alt text](https://github.com/motzkus2/LinuxMqtt/blob/main/13.jpg)

Komponenter: DHT11, MKR WIFI 1010 og mini usb kabel


## Dag 3 07-06-2021


#### Node red flow


![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/9.png)


**Flow Template:**

[{"id":"f6f2187d.f17ca8","type":"tab","label":"tempHum","disabled":false,"info":""},{"id":"794a005f.a6ecc","type":"mqtt in","z":"f6f2187d.f17ca8","name":"Recieve data from MQTT","topic":"dht11/data","qos":"0","datatype":"utf8","broker":"1a2a39f3.c2ec26","nl":false,"rap":false,"rh":"2","x":290,"y":200,"wires":[["ba9d2851.584328","5ebbdcfd.259f04"]],"icon":"font-awesome/fa-angle-double-right"},{"id":"3519a2ed.3f004e","type":"influxdb out","z":"f6f2187d.f17ca8","influxdb":"9b388e2f.3f4f3","name":"Send data","measurement":"data","precision":"","retentionPolicy":"","database":"database","precisionV18FluxV20":"s","retentionPolicyV18Flux":"","org":"EUCSYD","bucket":"Data","x":870,"y":200,"wires":[]},{"id":"ba9d2851.584328","type":"function","z":"f6f2187d.f17ca8","name":"Convert","func":"var text = String(msg.payload)\n\nvar e = text.split(',');\n\nmsg.temp = e[0]\nmsg.hum = e[1]\n\nreturn msg","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":520,"y":200,"wires":[["a388162c.fbc418"]],"icon":"node-red/file-out.svg"},{"id":"a388162c.fbc418","type":"function","z":"f6f2187d.f17ca8","name":"Map","func":"msg.payload = {\n    temp: parseFloat(msg.temp),\n    hum: parseFloat(msg.hum)\n}\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":690,"y":200,"wires":[["3519a2ed.3f004e","f4926b86.266b28"]],"icon":"node-red/parser-json.svg"},{"id":"5ebbdcfd.259f04","type":"debug","z":"f6f2187d.f17ca8","name":"Debug","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":510,"y":260,"wires":[]},{"id":"f4926b86.266b28","type":"debug","z":"f6f2187d.f17ca8","name":"Debug","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":850,"y":260,"wires":[]},{"id":"1a2a39f3.c2ec26","type":"mqtt-broker","name":"Localhost","broker":"192.168.42.12","port":"1883","clientid":"","usetls":false,"protocolVersion":"5","keepalive":"60","cleansession":true,"birthTopic":"","birthQos":"0","birthPayload":"","birthMsg":{},"closeTopic":"","closeQos":"0","closePayload":"","closeMsg":{},"willTopic":"","willQos":"0","willPayload":"","willMsg":{},"sessionExpiry":""},{"id":"9b388e2f.3f4f3","type":"influxdb","hostname":"192.168.42.12","port":"8086","protocol":"http","database":"hum","name":"Local","usetls":false,"tls":"","influxdbVersion":"2.0","url":"http://192.168.42.12:8086","rejectUnauthorized":false}]


#### Data ind i vores influx


#### Grafana




![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/10.png)



## Dag 4 08-06-2021


#### Web Assembly Klient:

Her har vi hurtigt lavet en klient med Blazor web assembly for at bevise vi kan bruge af vores node red API sammen med en klient. 

[https://github.com/Allf3/ApiBerryandtheWhales](https://github.com/Allf3/ApiBerryandtheWhales)


#### Node red Api:


![alt_text](https://github.com/motzkus2/LinuxMqtt/blob/main/11.png)


**Flow Template:**

[{"id":"d6dd25c8.b89248","type":"tab","label":"API","disabled":false,"info":""},{"id":"aace4f9f.cf4bf","type":"http response","z":"d6dd25c8.b89248","name":"OK","statusCode":"200","headers":{},"x":850,"y":240,"wires":[]},{"id":"8762c312.7ce84","type":"http in","z":"d6dd25c8.b89248","name":"","url":"/GetHum","method":"get","upload":false,"swaggerDoc":"","x":170,"y":300,"wires":[["f575836a.ddc27"]]},{"id":"24ad1ee6.d71612","type":"influxdb in","z":"d6dd25c8.b89248","influxdb":"9b388e2f.3f4f3","name":"Query","query":"from(bucket: \"Data\")\n  |> range(start: -1h)","rawOutput":false,"precision":"","retentionPolicy":"","org":"EUCSYD","x":530,"y":240,"wires":[["1736511a.7b503f"]]},{"id":"b7b44195.44747","type":"debug","z":"d6dd25c8.b89248","name":"Debug","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","targetType":"full","statusVal":"","statusType":"auto","x":850,"y":180,"wires":[]},{"id":"a5235576.6a28f8","type":"catch","z":"d6dd25c8.b89248","name":"","scope":["24ad1ee6.d71612"],"uncaught":false,"x":670,"y":120,"wires":[["b7b44195.44747","75595e4a.edc68"]]},{"id":"f575836a.ddc27","type":"function","z":"d6dd25c8.b89248","name":"Set flag","func":"msg.field = \"hum\"\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":340,"y":300,"wires":[["24ad1ee6.d71612","c018aa1d.57d2b8"]],"icon":"font-awesome/fa-tag"},{"id":"ee02ce60.18d25","type":"http in","z":"d6dd25c8.b89248","name":"","url":"/GetTemp","method":"get","upload":false,"swaggerDoc":"","x":160,"y":180,"wires":[["2908cdc4.d12e72"]]},{"id":"2908cdc4.d12e72","type":"function","z":"d6dd25c8.b89248","name":"Set flag","func":"msg.field = \"temp\"\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":340,"y":180,"wires":[["24ad1ee6.d71612","1a6be2c0.56194d"]],"icon":"font-awesome/fa-tag"},{"id":"1736511a.7b503f","type":"function","z":"d6dd25c8.b89248","name":"Filter","func":"let Data = []\n\nmsg.payload.forEach(n => {\n    if(n._field === msg.field)\n    {\n        Data.push(n);        \n    }\n})\n\nmsg.payload = Data;\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":670,"y":240,"wires":[["aace4f9f.cf4bf","b7b44195.44747"]],"icon":"node-red/parser-json.svg"},{"id":"75595e4a.edc68","type":"http response","z":"d6dd25c8.b89248","name":"No Content Response","statusCode":"204","headers":{},"x":900,"y":120,"wires":[]},{"id":"1a6be2c0.56194d","type":"debug","z":"d6dd25c8.b89248","name":"Debug","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","targetType":"full","statusVal":"","statusType":"auto","x":530,"y":120,"wires":[]},{"id":"c018aa1d.57d2b8","type":"debug","z":"d6dd25c8.b89248","name":"Debug","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"true","targetType":"full","statusVal":"","statusType":"auto","x":530,"y":360,"wires":[]},{"id":"9b388e2f.3f4f3","type":"influxdb","hostname":"192.168.42.12","port":"8086","protocol":"http","database":"hum","name":"Local","usetls":false,"tls":"","influxdbVersion":"2.0","url":"http://192.168.42.12:8086","rejectUnauthorized":false}]


## Kilder



*   [https://MQTT.org](https://MQTT.org)
*   [http://www.mosquitto.org/](http://www.mosquitto.org/)
*   [https://hub.docker.com/](https://hub.docker.com/)
*   [https://www.hivemq.com/blog/monitoring-hivemq-influxdb-grafana/](https://www.hivemq.com/blog/monitoring-hivemq-influxdb-grafana/)
*   [https://gabrieltanner.org/blog/grafana-sensor-visualization](https://gabrieltanner.org/blog/grafana-sensor-visualization)
*   [https://nodered.org/docs/getting-started/docker](https://nodered.org/docs/getting-started/docker)
*   [https://www.youtube.com/watch?v=JdV4x925au0](https://www.youtube.com/watch?v=JdV4x925au0)
*   [https://ubuntu.com/download/raspberry-pi](https://ubuntu.com/download/raspberry-pi)
*   [https://hub.docker.com/r/grafana/grafana](https://hub.docker.com/r/grafana/grafana)
*   [https://github.com/motzkus2/LinuxMqtt/blob/main/src/Shiftr.cpp](https://github.com/motzkus2/LinuxMqtt/blob/main/src/Shiftr.cpp)
*   
