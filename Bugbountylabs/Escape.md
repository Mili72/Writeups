Escape
***
Hola a todos! Bienvenidos a mi primer CTF en BugBountyLabs, espero que juntos podamos seguir aprendiendo y que os sirva de ayuda.

Bueno para empezar como siempre, un escaneo de puertos:

```bash
┌──(mili㉿kali)-[~/Desktop/BugBounty/Escape]
└─$ sudo nmap -sS --allports --min-rate 5000 -n -Pn 172.17.0.2
[sudo] password for mili: 
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-02-02 01:18 CET
Nmap scan report for 172.17.0.2
Host is up (0.0000050s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE SERVICE
5000/tcp open  upnp
MAC Address: 02:42:AC:11:00:02 (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
```

Podemos comprobar que el puerto 5000 está abierto, asi que vamos a comprobar desde nuestro navegador:


Nos aparece un apartado para ingresar nuestro XSS Reflejado, el cuál nos especifica que para poder explotarlo 
tendremos que escapar el script del atributo HTML. Este atributo podría impedir la ejecución de nuestro script. 
Por lo tanto yo voy a probar lo siguiente:




Y ya lo tendríamos. Agradecer a todos los que contribuyen y a los creadores de la plataforma por permitir que mucha 
gente podamos practicar de una forma tan sencilla y con pocos requisitos. Nos vemos en el siguiente :D!

