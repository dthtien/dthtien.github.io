# Computer networking
- We use simple abstraction of communication:
Node i --------- message *m* ----------> Node J
- Reality is much more complex:
  - Various network operators:
    eduroam, home DSL, Celluar data, coffee shop wifi, submarine cable, satellite
  - Physical communication:
    Electric current, radio waves, laser, hard drives in a van
- From a distributed system POV, the method of delivering the message is not important: We only see an abstract
  communication channel with a certain latency and bandwidth
  - Latency: delay from the time a message is sent until it is received
  - Bandwidth: the volume of data that can be transferred per unit time.
- The design of distributed algorithms is about deciding what message to send and how to process the message when they
  are recevied
- The web is an example of a distributed system that you use every day
  - There are 2 main types of nodes:
    - Servers host websites
    - Client display websites
  - Since the requests and response can be larger than we can fit in a single network packet, the HTTP protocol runs on
    top of TCP, which breaks down a large chunk of data into a stream of small network packet and puts them pack
    together again at the recipient
  - From distributed systems POV, we treat the request as one message and the response as another message, regardless of
    the number of physical network packets involved in transmitting them
# Good read
  https://www.khanacademy.org/computing/computers-and-internet/xcae6f4aj7ff015e7d:the-internet/xcae6f4a7ff015e7d:transporting-packets/a/transmission-control-protocol--tcp
