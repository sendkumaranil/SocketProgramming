# SocketProgramming
Chat Program Example

<p>The Java Chat application we are going to build is a console application that is launched from the command line. The server and clients can run on different computers in the same network, e.g. Local Area Network (LAN).</p>

<p>There can be multiple clients connect to a server and they can chat to each other, just like in a chat room where everyone can see other users’ messages. There’s no private chat between two users, for simplicity.</p>

<p>After getting connected to the server, a user must provide his or her name to enter the chat. The server sends a list of currently online users to the new user.</p>

<p>Every user is notified when a new user arrives and when a user has gone. Each message is prefixed with the username to keep track who sent the message.</p>

<p>And finally, the user says ‘bye’ to quit the chat.</p>

<p>The application consists of two parts: server and client. Each part can run independently on separate computers.</p>

# Chat Server Program
<p>The server is implemented by two classes: ChatServer and UserThread.</p>

<p>The <b>ChatServer</b> class starts the server, listening on a specific port.</p>

<p>When a new client gets connected, an instance of <b>UserThread</b> is created to serve that client.</p>

<p>Since each connection is processed in a separate thread, the server is able to handle multiple clients at the same time.</p>

<p>As you can see, the ChatServer class has two Set collections to keep track the names and threads of the connected clients. Set is used because it doesn’t allow duplication and the order of elements does not matter:</p>

    private Set<String> userNames = new HashSet<>();
    private Set<UserThread> userThreads = new HashSet<>();

<p>An important method in the ChatServer class is <b>broadcast()</b> which deliver a message from one client to all others clients:</p>

    void broadcast(String message, UserThread excludeUser) {
        for (UserThread aUser : userThreads) {
            if (aUser != excludeUser) {
                aUser.sendMessage(message);
            }
        }
    }

<p>The <b>UserThread</b> class is responsible for reading messages sent from the client and broadcasting messages to all other clients.</p>
<p>First, it sends a list of online users to the new user. Then it reads the username and notifies other users about the new user.</p>

<p>Then it enters a loop of reading message from the user and sending it to all other users, until the user sends ‘bye’ indicating he or she is going to quit. And finally it notifies other users about the disconnection of this user and closes the connection.</p>

# Chat Client Program

<p>The client is implemented by three classes: <b>ChatClient, ReadThread and WriteThread.</b></p>
<p>The <b>ChatClient</b> starts the client program, connects to a server specified by hostname/IP address and port number.</p>
<p>Once the connection is made, it creates and starts two threads <b>ReadThread</b> and <b>WriteThread.</b></p>
<p>The <b>ReadThread</b> is responsible for reading input from the server and printing it to the console repeatedly, until the client disconnects.</p>
<p>And the <b>WriteThread</b> is responsible for reading input from the user and sending it to the server, continuously until the user types ‘bye’ to end the chat.</p>

<p>The reasons for running these two threads simultaneously is that the reading operation always blocks the current thread (both reading user’s input from command line and reading server’s input via network). That means if the current thread is waiting for the user’s input, it can’t read input from the server.</p>
<p>Therefore, two separate threads are used to make the client responsive: it can display messages from other users while reading message from the current user.</p>

# Run the Chat Server

<b>Run Server:</b>
D:\ChatServer>java ChatServer 8181
<br><br><br>
<b>Run Client-1:</b>(opne new command prompt)<br>
D:\ChatClient>java ChatClient localhost 8181
<br><br>
<b>Run Client-2:</b>(opne new command prompt)<br>
D:\ChatClient>java ChatClient localhost 8181
<br><br>
<b>Run Client-3:</b>(opne new command prompt)<br>
D:\ChatClient>java ChatClient localhost 8181
<br><br>
<b>Screen Shot refrence:</b><br>
Server:
![alt tag](https://github.com/sendkumaranil/SocketProgramming/blob/master/ServerWindow.JPG)
<br>
Client-1:
![alt tag](https://github.com/sendkumaranil/SocketProgramming/blob/master/Chat_User1.JPG)
<br>
Client-2:
![alt tag](https://github.com/sendkumaranil/SocketProgramming/blob/master/Chat_User2.JPG)
<br>
Client-3:
![alt tag](https://github.com/sendkumaranil/SocketProgramming/blob/master/Chat_User3.JPG)
<br>
Thanks You:
