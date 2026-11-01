# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL

## AIM
To write a Python program to implement the Sliding Window protocol using socket programming.

## ALGORITHM
1. Start the program.
2. Run the server (receiver) and connect the client (sender) to it.
3. Get the number of frames and the window size from the user.
4. The client sends frames continuously until the window is full.
5. The server receives each frame in order and sends back its ACK.
6. When an ACK arrives, the client slides the window forward and sends the next frame.
7. Repeat until all frames are acknowledged, then the client sends END.
8. Stop the program.

## PROGRAM

**Server (run first):**
```python
import socket

s = socket.socket()
s.bind(('localhost', 1234))
s.listen(1)
c, _ = s.accept()
f = c.makefile('rw')

for line in f:
    if line.strip() == "END":
        break
    print("Frame", line.strip(), "received")
    f.write(line)
    f.flush()
c.close()
s.close()
```

**Client:**
```python
import socket

s = socket.socket()
s.connect(('localhost', 1234))
f = s.makefile('rw')

n = int(input("Number of frames: "))
w = int(input("Window size: "))

base = nxt = 0
while base < n:
    while nxt < base + w and nxt < n:
        print("Sent frame", nxt)
        f.write(f"{nxt}\n")
        f.flush()
        nxt += 1
    ack = int(f.readline())
    print("ACK", ack)
    base = ack + 1

f.write("END\n")
f.flush()
s.close()
```

## OUTPUT
<img width="1917" height="257" alt="image" src="https://github.com/user-attachments/assets/74b94e9e-3d36-4d1f-b89b-cdc9f3e21ded" />


## RESULT
Thus, the Python program to implement the Sliding Window protocol was successfully executed. Frames were sent within the window and the window slid forward as each acknowledgement was received.
