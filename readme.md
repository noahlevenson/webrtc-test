https://developer.mozilla.org/en-US/docs/Web/API/WebRTC_API/Connectivity

each peer needs to have a piece of data that describes who it is and how you connect to it - this is called a SESSION DESCRIPTION, and session descriptions are described using SESSION DESCRIPTION PROTOCOL (SDP). 



when it's time to connect:

the peer who wants to initiate the connection creates an OFFER, and the offer includes their SESSION DESCRIPTION.

the receiver of this call then sends back an ANSWER, and the answer includes their SESSION DESCRIPTION.

Then INTERACTIVE CONNECTIVITY ESTABLISHMENT (ICE) negotiates the connection using the information in both peers' sessions.  

after the connection is established, each peer keeps a copy of both their own session description and their connected peer's session description.





LET'S SAY I'M A NEW PEER, AND I WANT TO CONNECT TO OTHER PEERS AND START SHARING SHIT:

1.  I need to contact a STUN server to learn about ice candidates, which are just peers who are available to try to negotiate a connection with

2.  i create an offer (which is my local session description) and i set that offer as the description of my local session too

3.  i transmit this offer to the intended recipient of the call.  this is done in ANY WAY I WANT TO DO IT - like, i can just email the offer to the recipient if I want - but a more automated way to do it, for example, would be to have a node.js server running websockets that could transmit data in an event driven way

4.  the intended recipient receives the offer and stores the offer (which is just a session description) as the remote description

5.  the recipient creates an ANSWER (which is really just their session description) and sets it as their local description

6.  the recipient transmits the answer back to the caller using the signalling server (or whatever method)

7.  the caller receivers the answer and sets it as their remote description - now we have a connection