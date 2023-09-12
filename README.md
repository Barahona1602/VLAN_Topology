# VLAN_Topology
## Integrantes Grupo 7
| Ip | Nombre | Carnet |
|----------|----------|----------|
| 10.8.0.2 | Pablo Josué Barahona Luncey | 202109715 |
| 10.8.0.3 | Joshua David Osorio Tally |202110773 |
| 10.8.0.4 | Ryan José Rodrigo Sigüenza Huertas | 202100101 |
## Configurar OpenVPN
Con el archivo generado anteriormente ingresaremos a OpenVPN y cargaremos el archivo, este archivo iniciará la VPN y debemos de tener una pantalla así:

![image](https://github.com/Barahona1602/Redes1/assets/98893615/1b00d5bd-d360-4327-9389-39cb93ab1701)

Con esto ya tendremos configurada nuestra VPN y nuestra ip cambiará, para ello pasaremos al siguiente método y comprobación
## Comprobación de ip y ping ente estudiantes
- Primero verificaremos que nuestra ip se haya configurado correctamente, para ello ingresaremos el siguiente comando en nuestro cmd: `ipconfig` deberíamos de tener algo así. Para saber si está bien, debemos de ver en Dirección IPv4 y tiene que estar `10.8.0.2` (el último número puede variar):

![image](https://github.com/Barahona1602/Redes1/assets/98893615/76432076-fff5-40e7-9fa4-57c87db9f72e)

- Nuestro grupo es de 3 integrantes, por lo tanto estas son los ping de cada uno:
  - Pablo Barahona (ip: 10.8.0.2)
    
  ![image](https://github.com/Barahona1602/Redes1/assets/98893615/3cf4db1e-2020-4893-9204-dcbe2d347916)

 - Ryan Sigüenza (ip: 10.8.0.3)
   
   ![image](https://github.com/Barahona1602/Redes1/assets/98893615/e5d15339-b5c7-493b-8295-480ed5095462)

  - Joshua Osorio (ip: 10.8.0.4)
    
    ![image](https://github.com/Barahona1602/Redes1/assets/98893615/1a7015d2-c8db-4531-995a-eb08a5310165)

## Topologías de VLAN
Se requiere una conexión a través de la nube en tre tres topologías, una de área de trabajo, otra de backbone y otra de Servers. Estas deben de permitir la conexión entre sus propias VLAN pero no entre otras, por ejemplo RRHH no puede conectarse con los de Informática y viceversa.
### Topología 1: Área de trabajo
Esta zona, corresponde a la sección de cableado horizontal y área de trabajo, donde los usuarios finales tendrán acceso a los puntos de red y conectar un dispositivo final.  La cual tiene el diseño siguiente:

![WhatsApp Image 2023-09-12 at 1 16 55 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/df2d27c9-cc22-44d3-a9a9-420706fbbbe2)
### Topología 2: Backbone
La zona de cableado vertical será la encargada de conectar el área de trabajo con la zona de servidores, para esto se tiene nodos altamente redundantes cuya finalidad es brindar una conectividad el 100% del tiempo. La cual tiene el diseño siguiente:

![WhatsApp Image 2023-09-12 at 1 38 25 PM (1)](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/db6d2748-e81a-45bc-9e72-dd30b73dac48)

### Topología 3: Servidores
Lugar donde se almacenan los servidores web de la empresa. Se requiere que los mismos estén siempre activos, debido a esto la topología se vuelve extremadamente pesada. Por lo que se usará un nodo maestro-esclavo para equilibrar la carga del mismo. La cual tiene el diseño siguiente:

![WhatsApp Image 2023-09-12 at 1 38 25 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/a34600ff-a164-48a8-9707-4712c7947c2a)

## Configuraciones de topologías
Se hicieron la siguientes configuraciones en donde se le asigna modo truncal a los puertos correspondientes, se asigna la VTP, se crean las VLAN y se configura el STP, como también se configuran las ip correspondientes en los siguientes dispositivos:
### ESW1
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW2
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW3
```sh
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW4
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode server
end
conf t
vlan 10
name RRHH
exit
vlan 20
name Informatica
exit
vlan 30
name Contabilidad
exit
vlan 40
name Ventas 
end
conf t
spanning-tree vlan 10 root primary
end
conf t
spanning-tree vlan 20 root primary
end
conf t
spanning-tree vlan 30 root primary
end
conf t
spanning-tree vlan 40 root primary
end
copy running-config startup-config
```
### ESW5
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/3
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW6
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```

### ESW7
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/3
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW8
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/3
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW9
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/3
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/4
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```

### ESW10
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/1
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/3
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```
### ESW11
```sh
conf t
int f1/0
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/2
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
int f1/4
Switchport mode trunk
Switchport trunk allowed vlan 1,10,20,30,40,1002-1005
end
conf t
vtp domain GRUPO7
vtp password grupo7
vtp version 2
vtp mode client
end
copy running-config startup-config
```

### RRHH_1
```sh
ip 192.168.71.2/24 192.168.71.1
save
```

### RRHH_2
```sh
ip 192.168.71.3/24 192.168.71.1
save
```

### RRHH_3
```sh
ip 192.168.71.4/24 192.168.71.1
save
```

### RRHH_4
```sh
ip 192.168.71.5/24 192.168.71.1
save
```

### Informatica_1
```sh
ip 192.168.72.2/24 192.168.72.1
save
```

### Informatica_2
```sh
ip 192.168.72.3/24 192.168.72.1
save
```

### Informatica_3
```sh
ip 192.168.72.4/24 192.168.72.1
save
```

### Conta_1
```sh
ip 192.168.73.2/24 192.168.73.1
save
```

### Conta_2
```sh
ip 192.168.73.3/24 192.168.73.1
save
```

### Conta_3
```sh
ip 192.168.73.4/24 192.168.73.1
save
```

### Conta_4
```sh
ip 192.168.73.5/24 192.168.73.1
save
```

### Ventas_1
```sh
ip 192.168.74.2/24 192.168.74.1
save
```

### Ventas_2
```sh
ip 192.168.74.3/24 192.168.74.1
save
```

Ventas_3
```sh
ip 192.168.74.4/24 192.168.74.1
save
```

## Comprobaciones de configuración
Se debe de garantizar que los equipos del departamento correspondiente puedan comunicarse con otros equipos del propio departamento pero no con otros departamentos. Por ejemplo Ventas se puede comunicar únicamente con ventas y no con Informática ni Contabilidad
Para eso se hicieron las siguientes validaciones:
### Comprobación de VLAN's creadas en Topología 2 y su distribución a más ESW
ESW4:

![WhatsApp Image 2023-09-12 at 1 39 16 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/b557a687-27f6-4ee5-8d15-60d4a22c2f1d)

ESW11 (De topología 3):

![WhatsApp Image 2023-09-12 at 1 38 46 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/2d47cadf-ab39-4d0c-8f60-8efd21aa5718)
ESW6 (De misma topología):

![WhatsApp Image 2023-09-12 at 1 39 17 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/2af607a3-fbbd-4790-9e9d-2e06bf9f299f)

### Comunicación entre RRHH y error en comunicación con otro departamento
Se muestra ping a RRHH 1, 2 y 3 y error al hacer ping con Informática_2

![WhatsApp Image 2023-09-12 at 1 39 17 PM (3)](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/0d404f6c-e9c6-4282-9148-ce390459e810)

### Comunicación entre Conta y error en comunicación con otro departamento
Se muestra ping a Conta 1, 2 y 3 y error al hacer ping con Informática_1

![WhatsApp Image 2023-09-12 at 1 39 17 PM (4)](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/71bdb80d-30da-4998-a8bc-ec824f4c27b2)

### Comunicación entre Ventas y error en comunicación con otro departamento
Se muestra ping a Ventas 1 y 2 y error al hacer ping con Conta_2

![WhatsApp Image 2023-09-12 at 1 39 34 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/f60f8900-4884-4452-916a-7570f1bbe356)

### Comunicación entre Informática de cliente a server

![WhatsApp Image 2023-09-12 at 1 34 10 PM](https://github.com/Barahona1602/VLAN_Topology/assets/98893615/2333a649-cc8d-4cc5-aaaf-16b620143c28)


