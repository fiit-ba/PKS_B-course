# Laboratory practice 4

## Prerequisites

- python

## Task 1 - The basics of socket programming

### Preparation

Form pairs. One student will be responsible for implementing the server, and the other will implement the client. Make sure both devices are connected to the eduroam network.

### 1. Basic Message Sending and Receiving

Practice the basic process of sending and receiving messages between the client and server.
Ensure that the IP addresses are configured correctly before testing the connection.

Client:
```python
import socket
UDP_IP = "A.B.C.D" # server IP address
UDP_PORT = 50601
MESSAGE = b"Hello, World!"
print("UDP target IP: %s" % UDP_IP)
print("UDP target port: %s" % UDP_PORT)
print("message: %s" % MESSAGE)
sock = socket.socket(socket.AF_INET, # Internet
    socket.SOCK_DGRAM) # UDP
sock.sendto(MESSAGE, (UDP_IP, UDP_PORT))
sock.close()
```
Server:
```python
import socket
UDP_IP = "A.B.C.D" # server IP address
UDP_PORT = 50601
sock = socket.socket(socket.AF_INET, # Internet
    socket.SOCK_DGRAM) # UDP
sock.bind((UDP_IP, UDP_PORT))
data, addr = sock.recvfrom(1024) # buffer size is 1024 bytes
print("received message: %s" % data)
sock.close()
```

Capture the communication using Wireshark. What does the line sock.bind() mean?

### 2. Refactoring the Code for an Object-Oriented Approach
In this step, you will refactor your code to follow a more object-oriented design, making it easier to maintain, extend, and reuse.

Client:
```python
import socket
CLIENT_IP = "127.0.0.1" # client host ip A.B.C.D
CLIENT_PORT = 50602 # client port for receiving communication
SERVER_IP = "127.0.0.1" # Server host ip (public IP) A.B.C.D
SERVER_PORT = 50601

class Client:
    def __init__(self, ip, port, server_ip, server_port) -> None:
        self.sock = socket.socket(socket.AF_INET,
        socket.SOCK_DGRAM) # UDP socket creation
        self.server_ip = server_ip
        self.server_port = server_port

    def receive(self):
        data = None
        data, self.server = self.sock.recvfrom(1024) # buffer size is 1024 bytes
        return data #1

    def send_message(self, message):
        self.sock.sendto(bytes(message,encoding="utf8"),(self.server_ip,self.server_port))

    def quit(self):
        self.sock.close() # correctly closing socket
    print("Client closed..")

if __name__=="__main__":
    client = Client(CLIENT_IP, CLIENT_PORT, SERVER_IP, SERVER_PORT)
    data = "empty"
    print("Input your message: ") #1
    client.send_message(input()) # 1
    data = client.receive() # 1
    if data != None: # 1
        print(data) # 1
    else: # 1
        print("Message has not been received") #1
    client.quit() 
```

Server:
```python
import socket
SERVER_IP = "127.0.0.1" # Server host ip (public IP) A.B.C.D
SERVER_PORT = 50601 # Server port for recieving communication
class Server:
    def __init__(self, ip, port) -> None:
        self.sock = socket.socket(socket.AF_INET,socket.SOCK_DGRAM)
        # UDP socket creation
        self.sock.bind((ip, port)) #needs to be tuple (string,int)

    def receive(self):
        data = None
        while data == None:
            data, self.client= self.sock.recvfrom(1024) #buffer size is 1024 bytes
        
        print("Received message: %s" % data)
        return data # 1
        return str(data,encoding="utf-8")

    def send_response(self):
        self.sock.sendto(b"Message received... closing connection",self.client)

    def quit(self):
        self.sock.close() # correctly closing socket
        print("Server closed")

if __name__=="__main__":
    server = Server(SERVER_IP, SERVER_PORT)
    data = "empty"
    data = server.receive() # 1
    if data != None: # 1
        server.send_response() # 1
    else: # 1
        print("Message has not been received") #1
    server.quit()
```

Use Wireshark to capture the network communication. Observe and describe how the program’s behavior has changed.

### 3. We will keep the connection active until the client closes it.

Client:
```python
import socket
CLIENT_IP = "127.0.0.1" # client host ip A.B.C.D
CLIENT_PORT = 50602 # client port for recieving communication
SERVER_IP = "127.0.0.1" # Server host ip (public IP) A.B.C.D
SERVER_PORT = 50601
class Client:
    def __init__(self, ip, port, server_ip, server_port) -> None:
        self.sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # UDP socket creation
        self.server_ip = server_ip
        self.server_port = server_port

    def receive(self):
        data = None
        data, self.server = self.sock.recvfrom(1024) # buffer size is 1024 bytes
        # return data #1
        return str(data,encoding="utf-8")

    def send_message(self, message):
        self.sock.sendto(bytes(message,encoding="utf8"),(self.server_ip,self.server_port))

    def quit(self):
        self.sock.close() # correctly closing socket
        print("Client closed..")

if __name__=="__main__":
    client = Client(CLIENT_IP, CLIENT_PORT, SERVER_IP, SERVER_PORT)
    data = "empty"
    #print("Input your message: ") #1
    #client.send_message(input()) # 1
    #data = client.receive() # 1
    #if data != None: # 1
    # print(data) # 1
    #else: # 1
    # print("Message has not been received") #1
    while data != "End connection message received... closing connection":
        print("Input your message: ")
        client.send_message(input())
        data = client.receive()
        print(data)

    client.quit()
```
Server:
```python
import socket
SERVER_IP = "127.0.0.1" # Server host ip (public IP) A.B.C.D
SERVER_PORT = 50601 # Server port for receiving communication
class Server:
    def __init__(self, ip, port) -> None:
        self.sock =socket.socket(socket.AF_INET,socket.SOCK_DGRAM) # UDP socket creation
        self.sock.bind((ip, port)) #needs to be tuple (string,int)

    def receive(self):
        data = None
        while data == None:
            data, self.client= self.sock.recvfrom(1024) #buffer size is 1024 bytes
        print("Received message: %s" % data)
        #return data # 1
        return str(data,encoding="utf-8")

    def send_response(self):
        #self.sock.sendto(b"Message received... closing connection", self.client)
        self.sock.sendto(b"Message received...",self.client)

    def send_last_response(self):
        self.sock.sendto(b"End connection message received... closing connection", self.client)

    def quit(self):
        self.sock.close() # correctly closing socket
        print("Server closed..")

if __name__=="__main__":
    server = Server(SERVER_IP, SERVER_PORT)
    data = "empty"
    #data = server.receive() # 1
    #if data != None: # 1
    # server.send_response() # 1 
    #else: # 1
    # print("Message has not been received") #1
    while data != "End connection":
        if data != "empty":
            server.send_response()
        data = server.receive()

    server.send_last_response()
    server.quit()
```

Use Wireshark to capture the network communication. Observe and describe how the program’s behavior has changed.

### 4. Implement three-way handshake

Create a method on both the server and client sides that, before starting communication, establishes a connection using a (pseudo) three-way handshake.

Do not use protocol flags — it is sufficient to use a text-based format, for example, by sending a message such as:
Syn=1, Ack=1

Capture this communication using Wireshark.

### 5. Implement four-way handshake

Create a method on both the server and client sides that terminates the connection using a (pseudo) four-way handshake.

Do not use protocol flags — it is sufficient to use a text-based format, for example, by sending a message such as:
Fin=1, Ack=1

Capture this communication using Wireshark.


## Task 2 - ICMP Pinger -> [External Link](https://gaia.cs.umass.edu/kurose_ross/programming/Python_code_only/ICMP_ping_programming_lab_only.pdf)
Submit the complete client code and screenshots showing your Pinger output for four target hosts — one host from each of four different continents — to MS Teams.

Fixed `checksum` function, replace it in your code:
```python
def checksum(string):
    csum = 0
    countTo = (len(string) // 2) * 2
    count = 0

    while count < countTo:
        thisVal = string[count + 1] * 256 + string[count]
        csum = csum + thisVal
        csum = csum & 0xffffffff
        count = count + 2

    if countTo < len(string):
        csum = csum + string[len(string) - 1]
        csum = csum & 0xffffffff
    
    csum = (csum >> 16) + (csum & 0xffff)
    csum = csum + (csum >> 16)
    answer = ~csum
    answer = answer & 0xffff
    answer = answer >> 8 | (answer << 8 & 0xff00)
    return answer
```

Fixed `sendOnePing` function, replace it in your code:
```python
def sendOnePing(mySocket, destAddr, ID):
    # Header is type (8), code (8), checksum (16), id (16), sequence (16)

    myChecksum = 0
    # Make a dummy header with a 0 checksum
    # struct -- Interpret strings as packed binary data
    header = struct.pack("bbHHh", ICMP_ECHO_REQUEST, 0, myChecksum, ID, 1)
    data = struct.pack("d", time.time())
    # Calculate the checksum on the data and the dummy header.
    
    myChecksum = checksum(header + data)
    # Get the right checksum, and put in the header
    
    if sys.platform == 'darwin':
        # Convert 16-bit integers from host to network byte order
        myChecksum = htons(myChecksum) & 0xffff
    else:
        myChecksum = htons(myChecksum)

    header = struct.pack("bbHHh", ICMP_ECHO_REQUEST, 0, myChecksum, ID, 1)
    packet = header + data

    mySocket.sendto(packet, (destAddr, 1)) # AF_INET address must be tuple, not str
    # Both LISTS and TUPLES consist of a number of objects
    # which can be referenced by their position number within the object.
```

## Task 3 - Web	Server -> [External Link](https://gaia.cs.umass.edu/kurose_ross/programming/Python_code_only/WebServer_programming_lab_only.pdf)
Submit the complete server code and screenshots of your client browser demonstrating that the HTML file contents are successfully received from the server to MS Teams.