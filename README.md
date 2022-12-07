# Proyecto_dhcp

- **Objetivos**

En está práctica voy a implementar un servidor DHCP Linux en un contenedor y probar su funcionalidad con un cliente.

- **Proceso**

Normalmente empezariamos por instalar y configurar una imagen __networkboot/dhcpd__, pero en mi caso voy a utilizar una imagen ya configurada, descargandola de la pagina official 
```
https://hub.docker.com/r/networkboot/dhcpd/
```
Para configurar nuestro __DHCP__ empezamos creado un __**Docker-compose-yml**__ y introducimos el siguiente código 

```
version: '3.4'
services:
  dhcpd:
    container_name: DHCP
    network_mode: "host"
    image: networkboot/dhcpd
    volumes:
      - ./data:/data


```
Ahora creamos una carpeta Data y cargamos el docker compose con el comando __Docker-compose up__, si hemos esto correctamente se creara  dentro de la carpeta un documento llamado __dhcpd.conf__ una vez hecho esto escribimos el siguiente codigo

*Ojo este codigo es unico para la ip de mi ordenador, para  usarlo en otro equipo modificar la ip*
```
subnet 10.0.9.0 netmask 255.255.255.0 {    
    range 10.0.9.190 10.0.9.200;   

    option domain-name-servers 8.8.8.8, 193.146.96.2, 193.146.96.3;
    option domain-name "Server DHCP";
    option routers 10.0.9.24;    
    option domain-name-servers 10.0.2.10, 10.0.2.11;
    option subnet-mask 255.255.255.0;
    option broadcast-address 10.0.9.255;    

    default-lease-time 86400;
    max-lease-time 172800;
}
```
- **Comprobación**

Una vez hecho estó ya tendríamos todo listo para comenzar a usar nuestro servidor DHCP

![This is an image](https://github.com/Jacobo1234556/claseASIR/blob/main/Escritorio/Proyecto_DHCP/Imagenes/Captura%20de%20pantalla%20de%202022-11-30%2020-14-01.png)

Para comprobar que funciona lo que voy a hacer es, desde una maquina virtual, voy a pedir una ip dhcp a mi servidor de Docker.

Primero comienzo poniendo mi maquina en modo bridge y elimino la ip del router de clase con el comando __ipconfig/release__ 

Ahora veo la mac del equipo con el comando __getmac__ y la introduco en mi archivo dhcp.conf  y le doy una ip fija para la maquina de la siguiente manera.

```

  group {

     default-lease-time 604800;
     max-lease-time 691200;

     host apache {
         hardware ethernet 08:00:27:62:43:8A;
         fixed-address 10.0.9.201;
     }

  }
```
Si hemos hehco todo bien nuestra maquina habra recibido la ip que le hemos indicado, en mi caso, 10.0.9.201


![This is an image](https://github.com/Jacobo1234556/claseASIR/blob/main/Escritorio/Proyecto_DHCP/Imagenes/Captura%20de%20pantalla%20de%202022-12-07%2018-07-59.png)
