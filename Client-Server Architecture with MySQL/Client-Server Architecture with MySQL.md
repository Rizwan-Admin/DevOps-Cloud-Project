# Client-Server Architecture with MySQL

* ## Understanding Client-Server Architecture

  * Client-Server refers to an architecture in which two or more computers are connected together over a network to send and receive requests between one another.
 
  * n their communication, each machine has its own role: the machine sending requests is usually referred as "Client" and the machine responding (serving) is called "Server".
  * A simple diagram of Web Client-Server architecture is presented below.
 
  ![image](https://github.com/user-attachments/assets/6315b41a-3be8-48bb-9db3-0a5fc79ec353)
In the example above, a machine that is trying to access a Web site using Web browser or simply 'curl' command is a client and it sends HTTP requests to a Web server (Apache, Nginx, IIS or any other) over the Internet.

If we extend this concept further and add a Database Server to our architecture, we can get this picture:

![image](https://github.com/user-attachments/assets/a64c84d6-bc30-432c-80f0-2bcb6e8d6fe3)


In this case, our Web Server has a role of a "Client" that connects and reads/writes to/from a Database (DB) Server (MySQL, MongoDB, Oracle, SQL Server or any other), and the communication between them happens over a Local Network (it can also be Internet connection, but it is a common practice to place Web Server and DB Server close to each other in local network).



