# Proyecto_dhcp

- **Objetivos**
En está práctica voy a implementar un servidor DHCP Linux en un contenedor y probar su funcionalidad con un cliente.

- **Proceso**
Normalmente empezariamos por instalar y configurar una imagen __networkboot/dhcpd__, pero en mi caso voy a utilizar una imagen ya configurada, descargandola de la pagina official 
```
https://hub.docker.com/r/networkboot/dhcpd/
```
Para confirar nuestro __DHCP__ empezamos creado un __**Docker-compose-yml**__ y introducimos el siguiente código 

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
- **Conclusión**

Una vez hecho estó ya tendríamos todo listo para comenzar a usar nuestro servidor DHCP

![This is an image](https://github.com/Jacobo1234556/Proyecto_dhcp/blob/main/Escritorio/Proyecto_DHCP/Imagenes/Captura%20de%20pantalla%20de%202022-11-30%2020-14-01.png)
