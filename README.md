# LAN-of-Things-standard
This is an attempt to create an open standard for LAN of things devices. LAN of things devices are devices that are connected to a network, but not necessarily the Internet.  

# Get involved
This project is still a work in progress. If you want to get involved, have an idea, suggestion or question, feel free to open an issue. 

# Terminology

| Term                   | Description  |
| ---------------------- | ------------ |
| LANOT                  | abbreviation for LAN of things |
| LANOT device           | A device such as a TV, a thermostat, a coffee machine, a computer, a computer program, <br /> a light, a remote controlled outlet, etc. |
| LANOT device interface | An HTTP based interface to control one specific LANOT device (e.g. turn on, turn off, <br /> increase brightness etc.). |
| LANOT node             | A chip/device/server that is connected to a network and exposes one or more device <br /> interfaces to the network. It also listens to specific broadcast messages and informs <br />the sender about its existence. |
| LANOT interfacer       | A device/program/app that can access/control one or more LANOT devices by interfacing <br /> with one or more LANOT nodes (e.g. Sending a GET request to <br /> `http://{IP_OF_A_LANOT_NODE}/thermostat/bedroom-thermostat/set-temperature/22` <br /> to tell a thermostat to change the temperature to 22 degrees Celsius. |

# HTTP API
https://t-vk.github.io/LAN-of-Things-standard/LAN-of-Things-standard.html

# Abstract Example Set Up

```
            ------------------------
            |      interfacer      |
            ------------------------
                ^           |
                |           |
               HTTP   UDP broadcast
                |           |
                V           V
            -------------------------
            |      device node      |
            -------------------------
              /        |          \
             /         |           \
      ----------   ----------   ----------
      | device |   | device |   | device |
      ----------   ----------   ----------
```

# Specific Example Set Up
```
            ------------------------
            |    Smartphone App    |
            ------------------------
                       |    
                       | 
                       | 
            -------------------------
            |        ESP8266        |
            -------------------------
           /        |                \
          /         |                 \
      ------    ---------   ----------------------
      | TV |    | Light |   | Temperature sensor |
      ------    ---------   ----------------------
```

# Advanced Example Set Up
```
                  ------------------------
                  |    Smartphone App    |
                  ------------------------
                   /                     \
                  /                       \
                 /                         \
                /                           \
               /                             \
  --------------------------------   ------------------------------------------
  |        Raspberri Pi          |   |                 ESP8266                |
  | (wired to 433mhz transmitter |   | (wired to IR transmitter to control TV |
  | to control remote controlled |   |      and wired to temperature sensor)  |
  | outlets)                     |   |                                        |
  --------------------------------   ------------------------------------------
        |                   \             /                    |
     on/off                on/off   volume up/down      read temperature
        |                      \    channel up/down            |
        |                       \    /                         |
    ----------                  ------               ----------------------
    | Light1 |                  | TV |               | Temperature sensor |
    ----------                  ------               ----------------------
    
```

# Reasons for our decisions and FAQ

In this section we try to explain why we decided to go with certain protocols etc.

**Why are we currently dividing things into LANOT devices, LANOT device interfaces and LANOT device nodes?**   
While often times a device node will only control one device, there are a lot of cases in which this is not the case. For instance to control a TV, a sound system etc you would need a device that can send IR signals just like the original remotes. Now you could of course have one IR sender for each device and conenct each of these senders to your WiFi network. But if the devices are in the same room, you can save costs and resources by simply using one IR sender.  
At first sight this might seem to raise problems, because now you have one IP address that corresponds to multiple devices. But that is not a problem because the TV could be accessable under `192.168.1.3/device-interfaces/generic-tv/livingroom-tv` and the sound system could be accessable under `192.168.1.3/device-interfaces/generic-speakers/livingroom-soundsystem` and at the same time you could also make the IR sender accessable directly under `192.168.1.3/device-interfaces/ir-remote/livingroom-ir-sender`.  

**Why are we currently using HTTP**  
HTTP is a widely use protocol that many devices understand. It is easy to implement the server (device interfaces) on Raspberry PIs, on Espressif chips like the ESP8266/ESP8265/ESP32, on Windows/Linux/MacOS machines and while more complicated it is also possible on Android devices. And the clients (device interfacers) can easily be implemented on almost any platform: Raspberri Pi, Android, Windows, Linux, MacOS, iOS, Espressif chips like the ESP8266/ESP8265/ESP32, FreeBSD, OpenBSD, ... 

**Why are we currently using UDP broadcasts**  
Since HTTP doesn't support broadcasting, we have to use UDP for that instead. The broadcast is used to send a message to all LANOT device nodes on the same network, the device nodes then have to send back an answer so that the LANOT interfacer knows the IP addresses of all device nodes. Since UDP broadcasting and receiving isn't always easy to implement, this is an optional feature. A LANOT interfacer should have the option to let the user enter the LANOT device nodes IP addresses manually. 

**Why are we currently not using the HTTP CRUD principle with POST/GET/PUT/DELTE?**  
It is known that some applications and libraries only support the GET and POST methods. Some actually only support GET.  
So we try to define an interface that can be used with GET only. Since an HTTP GET request limited in size, we also allow using POST. In that case you would simply send the query parameters in the body instead of the URL.

**Why does every LANOT device node need an HTTP route that returns information on all connected devices**  
While it would be possible to make the device nodes provide the information in the response to a UDP broadcast scan, the current specification makes the implementation of UDP optional. 
