Download Link: https://assignmentchef.com/product/solved-project-socket-programming-part-2
<br>
Part 2 of this project is an exercise to create a simple FTP application.

<strong> </strong>Section I: Goal

Design a File Transfer Protocol (FTP) application for communication over a computer network by means of TCP protocol, with client-server network architecture using socket programming. You can build over your code of project part 1, by making necessary changes and additions as described in the requirements of this part of the project.

<h1>Section II: File Transfer Protocol</h1>

File Transfer Protocol (FTP) is a standard internet protocol, used to transfer files between two hosts. Clients initiate communication with servers. Using FTP, a client can read, write, move, copy, rename and delete a file on a server. In this project we will create a simple FTP application where a client can perform a read/write from/to a file at the server.

<h1>Types of Server</h1>

A stateless server does not maintain any information about client requests (e.g., the Network File System (NFS)). The client requests service by sending a message to the server. The server executes the request and sends back a response. It’s possible that the request or the response might get lost. Both of these cases look the same to the client — it doesn’t get an answer. If the client does not receive an answer within a timeout period, it re-issues the request. If the requests to the server are idempotent, meaning that the request can be executed any number of times with the same effect, then re-executing the request doesn’t cause a problem.

However, not all services can be made idempotent. Some servers must remember the state of the client and are therefore called <em>stateful</em>. For example, in a lock server, executing a request twice can have very bad results. Suppose a client requests a lock on file F. The server grants the lock, but the response is lost. The client times out and re-issues the request. The server responds with a message saying that the file is currently locked. Now, the file is permanently blocked because the client will never send the unlock command.




<strong><u>Section III: Socket Programming Project Part 2 Details:</u></strong>

<u>Step 1.</u> Write the following two programs using C/C++ on Linux.

<ol>

 <li>Create a concurrent server program using TCP.</li>

 <li>Create a client program using TCP</li>

</ol>

<u>Step 2.</u> Client and Server requirements (5%)

<ol>

 <li>Server program should be able to accept port number from command line argument. If a port number is not provided then your program should display an error message and exit.</li>

</ol>

<h1>server p</h1>

<ol>

 <li>Client program should be able to accept hostname <strong>h</strong> (string), client number <strong>c</strong> (integer) and port number <strong>p</strong> (integer) from command line arguments. If all the above said arguments are not provided then your program should display an error message and exit. <strong>client h c p </strong></li>

</ol>

<strong><em> </em></strong>

The client(s) and the server communicate using port <strong>p</strong>, hence the clients and server must be started with the same port number. Each client must have a unique client number.




Assumptions:

<ol>

 <li>We are assuming, for this project, that if a client has contacted the server once, it will not contact the server again. Therefore, you can keep a track of all client numbers in your list and deny connections to each duplicate client number.</li>

 <li>We are assuming that the order of command line arguments is correctly entered by the application user. Therefore, your program does not require to check for it.</li>

</ol>




<u>Step 3.</u> FTP application requirements (5% + 5% + 25% + 20%)

<ol>

 <li>Implement the server, such that it can handle multiple connections using either fork() system call or threads. <em>A maximum of 5 clients. </em></li>

</ol>




<ol>

 <li>Further, each connection between the client(s) and the server can have a series of requests. <em><u>How to implement:</u> </em>For each connection, server will ask the client if the client wants to go ahead with another request, expecting a reply of <strong>Y</strong> or <strong>N </strong>for yes and no, respectively. Thereafter the server will act in accordance: If client replies <strong>Y</strong>, server will expect another request coming its way. If the client replies <strong>N</strong>, server will close the socket for that client.</li>

</ol>




<ol>

 <li>FTP Client</li>

</ol>

Client will send a request to server for reading/writing to a file. This request will consist of a &lt;filename&gt; and &lt;mode_of_operation&gt;. Mode of operation can be “r” for reading a file from the server, and “w” for writing a file to the server. Each client request will be of the following form:

&lt;filename&gt;, &lt;mode&gt;

Example: file1.txt, r

<ol>

 <li>If client is reading a file from the server, it should recreate the file received from the server, line by line. Finally, closes the file.</li>

 <li>If client is writing to a file to a server, it should read from a file existing at its client end write the file line by line at the server, and finally close the file.</li>

</ol>

FTP Server

The server should receive the name of the file from the client. If the file cannot be opened in the current directory, then the server should return the string “File not found.” to the client.

<ol>

 <li>If the file can be opened and a read is requested, then the server should send each line of the file to the client until the end of the file is reached. After sending the complete file to the client, server closes the file.</li>

 <li>If the client has requested write to a file. A server opens a new file, receives each line of file from client and writes it to the file at its server end, and finally closes the file.</li>

</ol>




<em><u>How to check that your application has transferred the file correctly:</u></em> you can use “diff” command on the original file and the one that has been received through the FTP transfer application.




<ol>

 <li>When a client is reading/writing a file to/from server, that file cannot be accessed by any other client. Therefore, it is said that the server will put a lock on the file.

  <ol>

   <li>When a client is reading a file from the server, server will put a read lock on that file. This will allow any other client to access the same file for reading only (not for writing).</li>

   <li>When a client is writing to a file on the server, server will put a write lock on the file. This will not allow any other client to access the same file in any mode.</li>

  </ol></li>

</ol>

<em><u>How to implement:</u></em> assign a lock in the database, i.e., table/vector/array/data structure to record the &lt;filename&gt; which is currently being written to client and &lt;lock&gt; to restrict the usage of this file, depending on the mode, by any other client. Once, the file transfer is complete, your program will need to release the lock from the given file.