# ThustAdri
Thust-DockerLabs🇪🇸 

**1.1El primer paso es descargar el archivo trust.zip y lo vamos a descomprimir en la terminal**
```bash
 unzip trust.zip 
```
**1.2 Vamos a crear un script de bash el cual nos abra desde docker el autodeploy.sh para poder asi iniciar esta máquina**
```bash
nano auto_deploy.sh
```
**Dentro añadiremos este contenido**

```bash
#!/bin/bash

# Colores
greenColour="\e[0;32m\033[1m"
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
blueColour="\e[0;34m\033[1m"
yellowColour="\e[0;33m\033[1m"
purpleColour="\e[0;35m\033[1m"
turquoiseColour="\e[0;36m\033[1m"
grayColour="\e[0;37m\033[1m"

function ctrl_c(){
    echo -e "\n\n${redColour}[!] Saliendo...${endColour}\n"
    proc_red=$(docker network ls | grep dockerlabs_net | awk '{print $1}')
    if [ ! -z "$proc_red" ]; then
        docker network rm dockerlabs_net > /dev/null 2>&1
    fi
    proc_container=$(docker ps -a | grep trust | awk '{print $1}')
    if [ ! -z "$proc_container" ]; then
        docker rm -f $proc_container > /dev/null 2>&1
    fi
    exit 1
}

# Ctrl+C
trap ctrl_c INT

if [ "$(id -u)" -ne 0 ]; then
    echo -e "\n${redColour}[!] Este script debe ser ejecutado como root (sudo).${endColour}\n"
    exit 1
fi

if [ -z "$1" ]; then
    echo -e "\n${redColour}[!] Uso: $0 <archivo.tar>${endColour}\n"
    exit 1
fi

clear
echo -e "${blueColour}"
echo "    _               _               _          _     "
echo "   | |             | |             | |        | |    "
echo " _ | | ___   _ ____| |  _ ____ ____| |  _ ____| |__  "
echo "/ || |/ _ \ / / ___) | / _  _  _  _) | / _  _  _)  _ \ "
echo "| (_| | |_| | |    | |/ / | | | |  | |/ / | | | | |_) )"
echo " \____|\___/|_|_|    |___/|_|_|_|  |___/|_|_|_|_|____/ "
echo -e "${endColour}"

echo -e "${yellowColour}[+] Comprobando si existen contenedores previos...${endColour}"
docker rm -f $(docker ps -aq --filter name=trust) > /dev/null 2>&1
docker network rm dockerlabs_net > /dev/null 2>&1

echo -e "${yellowColour}[+] Creando red interna dockerlabs_net...${endColour}"
docker network create --subnet=172.18.0.0/16 dockerlabs_net > /dev/null 2>&1

echo -e "${yellowColour}[+] Cargando la imagen de la máquina...${endColour}"
docker load -i $1

echo -e "${yellowColour}[+] Desplegando el contenedor vulnerable...${endColour}"
docker run -d --name trust --net dockerlabs_net --ip 172.18.0.2 trust > /dev/null 2>&1

echo -e "\n${greenColour}Máquina desplegada, su dirección IP es --> 172.18.0.2${endColour}"
echo -e "${redColour}Presiona Ctrl+C cuando termines con la máquina para eliminarla${endColour}\n"

while true; do
    sleep 1
done
```
**1.3.Por último para preparar el docker lo iniciaremos**

```bash
chmod +x auto_deploy.sh
sudo ./auto_deploy.sh trust.tar
```
*Deberá de quedar algo asi*
<img width="975" height="506" alt="image" src="https://github.com/user-attachments/assets/537075fe-4f38-4011-addb-270c8658f3d4" />

**2. Para poder comprobar la comunicación haremos un ping desde otra terminal**
```bash
ping -c3 172.18.0.2
```
<img width="960" height="437" alt="image" src="https://github.com/user-attachments/assets/7172ba55-381c-41e2-a614-b1998909dc25" />
























