# Instalación y configuración de VPN en linux 

Para configuarar un VPN en linux se debe seguir los siguientes pasos:

- Paso 1

    En nuestra maquina Ubunto darle una dirección ip estatica en nuestro caso sera la siguiente

        Ip 10.0.0.16
        Mascara 255.255.255.0
        Puerta de enlace 10.0.0.1
        Dns 8.8.8.8

- Paso 2

    Instalamos el programa que nos permitirá hacer nuestro SO Ubuntu un servidor VPN.

        apt-get install pptp

- Paso 3

    Abrimos la siguiente configuracion del programa pptpd y nos ubicamos en localip, descomentamos localip y remoteip cualquiera de las dos que se encuentra en el archivo y procedemos a ubicar la ip de la maquina y el rango de ip q se van a conctar.

        nano /etc/pptpd.conf

        localip 10.10.10.1
    
    Guardamos la configuración

- Paso 4

    Entramos al siguiente archivo para cambiarle el nombre de nuestro servidor VPN

        nano /etc/pp/pptpd-options
    
    Por defecto viene como name pptpd (Opcionala)

        name prueba
    
    Luego verificamos que los siguientes **require** se encuentren des comentados

        require-mschap-v2

        require-mppe-128
    
    Después nos ubicamos en el apartado de **Network and Routing** y des comentamos los ms-dns para ubicar los dns de Google

        ms-dns 8.8.8.8
        ms-dns 8.8.4.4

    Para que los clientes cuando se conecte tengan ese DNS por defecto.
    
    Guardamos los cambios

- Paso 5

    Crearemos nuestros usuarios que se van conectara la VPN, entramos al siguiente archivo 

        nano /etc/ppp/chap-secrets

    Una vez dentro crearemos el usuario

        #cliente    server  secret          IP addresses
        brangy      prueba  123456          *
    
    Guardamos 

- Paso 6

    A continuacion configuraremos **iptables** para redirigir el tráfico de todos los paquetes a través de la VPN.

        iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -o enp0s3 -j MASQUERADE

    En este momento crearemos el fichero rc.local dentro del directorio etc y añadiremos las líneas corespondientes para que el comando que acabamos de ejecutar, el de iptables, se ejecute cada vez que iniciamos o reiniciamos nuestro servidor.

        nano /etc/rc.local  

        iptables -t nat -A POSTROUTING -s 10.10.10.0/24 -o enp0s3 -j MASQUERADE

        chmod +x /etc/rc.local  

- Paso 7 

    La ultima configuracion sera habilitar las tablas para que tengan acceso a internet

        nano /etc/sysctl.conf

    Descomentamos la siguiente linea

        net.ipv4.ip_forward=1
    
    Guardamos los cambios

- Paso 8 

    Verificamos el estado del servicio pptpd

        service pptpd status

    En el caso de que este inactivo se reunicia el servidor con el siguiente comando

        sudo /etc/init.d/ pptpd restart

    Y verificamos de nuevo si el servicio esta activo 

        service pptpd status

## LISTO


