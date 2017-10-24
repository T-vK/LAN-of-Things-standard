# LAN-of-Things-standard
This is an attempt to create an open standard for LAN of things devices. LAN of things devices are devices that are connected to a network, but not necessarily the Internet. 

# Terminology
- LANOT: short form for LAN of things
- LANOT device: A device such as a TV, a thermostat, a coffee machine, a computer, a computer program, a light, a remote controlled outlet, etc.
- LANOT device interface: An HTTP based interface to control one specific LANOT device (e.g. turn on, turn off, increase brightness etc.).
- LANOT node: A chip/device/server that is connected to a network and exposes one or more device interfaces to the network. It also listens to specific broadcast messages and informs 
- LANOT interfacer: A device/program/app that can access/control one or more LANOT devices by interfacing with one or more LANOT nodes (e.g. Sending a GET request to `http://{IP_OF_A_LANOT_NODE}/thermostat/bedroom-thermostat/set-temperature/22` to tell a thermostat to change the temperature to 22 degrees Celsius.

# HTTP API
https://t-vk.github.io/LAN-of-Things-standard/LAN-of-Things-standard.html

# Abstract Example 

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

# Specific example
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

# Advanced example
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
  
