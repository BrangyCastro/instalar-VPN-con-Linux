# Instalacion y configuracion de VPN en linux 

Para configuara un VPN en linux se debe seguir los siguientes pasos:

- Paso 1

    En nuestra maquina Ubunto darle una direccion estatica en nuestro caso sera la siguiente

        Ip 10.0.0.16
        Mascara 255.255.255.0
        Puerta de enlace 10.0.0.1
        Dns 8.8.8.8

- Paso 2

    Instalamos el programa que nos permitira hacer nuestro SO Ubuntu un servidor VPN.

        apt-get install pptpd

- Paso 3

    Abrimos la siguiente configuracion del programa pptpd y nos ubicamos en localip, descomentamos localip y remoteip cualquiera de las dos que se encuentra en el archivo y procedemos a ubicar la ip de la maquina y el rango de ip q se van a conctar.

        nano /etc/pptpd.conf

        localip 10.0.0.16
        remoteip 10.0.0.17-30,10.0.0.245
    
    Guardamos la configuración

- Paso 4

    Entramos al siguiente archivo para cambiarle el nombre de nuestro servidor VPN

        nano /etc/pp/pptpd-options
    
    Por defecto viene name pptpd

        name prueba
    
    Luego verificamos que los siguiente require esten descomentados

        require-mschap-v2

        require-mppe-128
    
    Despues nos ubicamos en el apartado de Network and Routing y descomentamos los ms-dns para ubicar los dns de Google

        ms-dns 8.8.8.8
        ms-dns 8.8.4.4

    Para que los cliente cuando se conecte tengan ese DNS por defecto.
    
    Guardamos los cambios

- Paso 5

    Crearemos nuestros usuarios que se van conectara la VPN, entramos al siguiente archivo 

        nano /etc/ppp/chap-secrets

    Una vez dentro creameros el usuario

        #cliente    server  secret          IP addresses
        brangy      prueba  123456          *
    
    Guardamos 

- Paso 6 

    Verificamos el estado del servicio pptpd

        service pptpd status

    En el caso de que este inactivo se reunicia el servidor con el siguiente comando

        sudo /etc/init.d/ pptpd restart

    Y verificamos de nuevo si el servicio esta activo 

        service pptpd status

- Paso 7 

    La ultima configuracion sera habilitar las tablas para que tengan acceso a internet

        nano /etc/sysctl.conf

    Descomentamos la siguiente linea

        net.ipv4.ip_forward=1
    
    Guardamos los cambios

## LISTO


