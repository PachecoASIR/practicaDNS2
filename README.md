# PracticaDNS2

version: "3.3"
services:
 asir_bind9:
   image: internetsystemsconsortium/bind9:9.16 --> Imagen que usamos para nuestro servidor
   ports:
     - 53:53 --> Puertos que empleamos
   networks:
      br02: --> Red creada por nosotros para que sea usada por el servidor/cliente
        ipv4_address: 10.1.0.4 --> IP fija de nuestro servidor
   volumes:
     - conf2:/etc/bind --> Volumen que emplearemos para nuestro servidor
 asir_cliente:
   image: ubuntu --> Imagen cliente
   networks:
     - br02 --> Red empleada
   stdin_open: true  # docker run -i
   tty: true         # docker run -t
   dns:
     - 10.1.0.4 --> IP del servidor DNS utilizado
volumes:
 conf2: --> Volumen empleado

networks: --> Declaración de la red 
  br02:
    external: true 
    
Crear red antes de los contenedores:
docker network create --subnet 10.1.0.0/24 --gateway 10.1.0.1 br02
docker network ls

Creamos nuestro volumen:
docker volume create conf2

Crear contenedor en una red (no es necesario): 
docker container run -it --ip 10.1.0.11 --network br02 ubuntu

Conectarse a una red: (no es necesario)
docker network connect br02 cliente_dns_bind9

Comprobamos que el cliente haga ping:
ping 10.1.0.4

Una vez tengamos esto abriremos el volumen en ventana modo desarrollador y editaremos los archivos que sean necesarios.
