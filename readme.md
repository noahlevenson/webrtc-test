# HOW TO USE THIS THING:

Set up two computers. Let's call one of them Alice and the other one Bob. We're going to 
establish a WebRTC connection between them.

STEPS:

1. On both Alice and Bob, host index.html on a local webserver. (`python3 -m http.server 9000` works).
Navigate to it in the browser. Open up the console. All the important information will be logged there.

2.  On Alice, click "make offer". You'll see a few things quickly get logged to the console, but
under the hood, we're actually executing a few discrete steps. First, you'll see your offer SDP 
without any ICE candidates attached. Then you'll see us kick off ICE candidate gathering, and candidates 
trickle in until the "null" candidate arrives. And finally, we attach those candidates to your 
offer SDP, and we again show your offer SDP, but now with ICE candidates attached.

3. Copy that console output and figure out how to get it to Bob. Emailing it to yourself works. 
You're a signaling server, bro!

4. You can decide to signal in trickle or non-trickle mode. In trickle mode, you'd paste Alice's
offer - the one without ICE candidates - into the box, and click "accept offer." Bob's console
will now show the same output we saw from Alice. Copy that console output and figure out how
to get it to Alice. Then you'll paste each ICE candidate individually, clicking "add ice candidate"
each time. In non-trickle mode, you'd instead paste Alice's offer -- the one **with** ICE candidates -- into 
the box, and click "accept offer." This saves you the trouble of inputting the ICE candidates separately.

5. Back on Bob, follow the steps described in #4 - that is, decide whether you're doing trickle
or non-trickle mode, and paste in the appropriate information from Alice. You should see a message
indicating that the data channel is open, and the "send msg" button should become enabled. You can
now type into the input box on both Bob and Alice and send messages back and forth without an
intermediary.
