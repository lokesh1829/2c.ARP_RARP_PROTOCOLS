# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
ARP_server.py
~~~
import socket

def arp_server():
    # Predefined ARP Table mapping IP -> MAC
    arp_table = {
        "192.168.1.1": "00:1A:2B:3C:4D:5E",
        "192.168.1.2": "A1:B2:C3:D4:E5:F6",
        "192.168.1.3": "12:34:56:78:9A:BC",
        "192.168.1.4": "AA:BB:CC:DD:EE:FF"
    }

    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(('127.0.0.1', 8080))
    server_socket.listen(1)
    print("ARP Server is running and listening on port 8080...")

    conn, addr = server_socket.accept()
    print(f"Connected to client: {addr}")

    # Receive requested IP address
    ip_address = conn.recv(1024).decode().strip()
    print(f"Received IP Address: {ip_address}")

    # Lookup MAC address
    mac_address = arp_table.get(ip_address, "MAC Address Not Found")

    # Send response back to client
    conn.send(mac_address.encode())
    print(f"Sent MAC Address: {mac_address}")

    conn.close()
    server_socket.close()

if __name__ == '__main__':
    arp_server()
~~~
ARP_client.py
~~~
import socket

def arp_client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect(('127.0.0.1', 8080))

    ip_address = input("Enter IP Address to resolve: ")
    client_socket.send(ip_address.encode())

    mac_address = client_socket.recv(1024).decode()
    print(f"Resolved MAC Address: {mac_address}")

    client_socket.close()

if __name__ == '__main__':
    arp_client()
~~~
## OUPUT - ARP

<img width="883" height="281" alt="image" src="https://github.com/user-attachments/assets/0f9640c9-6d5e-48e5-b0e8-f0c13bfa3118" />



## PROGRAM - RARP
RARP_server.py
~~~
import socket

def rarp_server():
    # Predefined RARP Table mapping MAC -> IP
    rarp_table = {
        "00:1A:2B:3C:4D:5E": "192.168.1.1",
        "A1:B2:C3:D4:E5:F6": "192.168.1.2",
        "12:34:56:78:9A:BC": "192.168.1.3",
        "AA:BB:CC:DD:EE:FF": "192.168.1.4"
    }

    server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    server_socket.bind(('127.0.0.1', 8081))
    server_socket.listen(1)
    print("RARP Server is running and listening on port 8081...")

    conn, addr = server_socket.accept()
    print(f"Connected to client: {addr}")

    # Receive requested MAC address
    mac_address = conn.recv(1024).decode().strip()
    print(f"Received MAC Address: {mac_address}")

    # Lookup IP address
    ip_address = rarp_table.get(mac_address, "IP Address Not Found")

    # Send response back to client
    conn.send(ip_address.encode())
    print(f"Sent IP Address: {ip_address}")

    conn.close()
    server_socket.close()

if __name__ == '__main__':
    rarp_server()
~~~

RARP_client.py
~~~
import socket

def rarp_client():
    client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    client_socket.connect(('127.0.0.1', 8081))

    mac_address = input("Enter MAC Address to resolve: ")
    client_socket.send(mac_address.encode())

    ip_address = client_socket.recv(1024).decode()
    print(f"Resolved IP Address: {ip_address}")

    client_socket.close()

if __name__ == '__main__':
    rarp_client()
~~~
## OUPUT -RARP

<img width="890" height="248" alt="image" src="https://github.com/user-attachments/assets/38561154-5abc-47e8-b868-ff432a6e1f9e" />


## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
