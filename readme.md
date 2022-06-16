HOW TO USE THIS THING:

Set up two computers.  Let's call one of them Alice and the other one Bob.  We're going to 
establish a WebRTC connection between them, using email as the signaling mechanism.

STEPS:

1.  On both Alice and Bob, host index.html on a local webserver, and navigate to it in the browser.
Open up the console. All the important information will be logged there.

2.  On Alice, click "make offer". This generates SDP metadata for the offering peer, and a few ms
later, you'll see ICE candidates start to roll in, as the WebRTC API uses your browser's integrated
STUN client to perform NAT traversal and apply ICE heuristics to rank your connectivity options.

3.  Copy the offer -- it's the object that begins with "type: offer" -- and the topmost candidate
object, and paste them into an email.  We're signalling that information to Bob!

4.  On Bob, grab the aforementioned email.  Cut and paste the offer object into the input box, and
click "accept offer". This will generate an answer, and you'll see ICE candidates roll in as before.
Now cut and paste the candidate object from the email into the input box, and click "add ice candidate".

5.  We're not done with Bob yet: Copy the answer -- it's the object that begins with "type: answer"
-- and paste it into an email.  We're signalling that information to Alice!

6.  On Alice, grab the aforementioned email. Cut and paste the answer object into the input box and
click "accept answer". You should see a message indicating that the data channel is open, and the
"send msg" button should become enabled. You can now type into the input box on both Alice and Bob
and send messages back and forth without an intermediary.  Note that for some reason, we didn't
have paste Bob's ice candidate to Alice -- I'm not sure why that is.


AREAS FOR THINKING/INNOVATION:

1.  The STUN/TURN server requirement is centralizing and kind of defeats the purpose here.  Is there a way to create a distributed STUN/TURN server?  What about alternative methods to getting around NAT (like hole punching?) Can we get around NAT in a different way and construct a valid session description?

2.  The signalling server is also centralizing. In the case where peers gather around a website, you might imagine that the website would provide the signalling server (and also probably the STUN/TURN server). But for other cases, what are less centralized means of doing this and bootstrapping the first peer connection?  Is this a case where distributed hash tables might be useful?




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
