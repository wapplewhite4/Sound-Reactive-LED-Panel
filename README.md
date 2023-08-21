# Sound-Reactive-LED-Panel
Sound Reactive Wifi Enabled LED Panel - Will Applewhite

Components
Hardware:
ESP32 Wifi/Bluetooth Enabled Microcontroller (x2)
INMP441 Omnidirectional Microphone MEMS Module
16 x 16 LED Matrix Panel Individually Addressable (x4)
8 x 32 LED Matrix Panel Individually Addressable
5V 60A 300W Power Supply/Converter
ATM Inline Fuse Holder
40A Automotive Fuse
TP-Link AC1200 Wifi Range Extender
Plug In Socket Watt Meter
	Software:
WLED 0.13.3 SR Fork
	Other Materials:
½ in x 1.5 in Oak Board
Minwax Tung Oil Finish
¼ in Plywood
18 ga Primary Wire (Red and Black)
JST SM 3 Pin Connectors
Mini Breadboards and Jumper Wires
3 Wire Lever Connectors
Heat Shrink Tubing
Solder
Electrical Tape
Gorilla Glue
Gorilla Glue Double Sided Tape
Cord/Cable Clip Organizers
Sawtooth Hanging Bracket
Felt Furniture Pads

Challenges:
Panels of Different Dimensions - The main “flaw” in my design turned out to be that I was attempting to use four 16x16 LED panels and an 8x32 LED panel to form one rectangular display. Unfortunately the WLED software does not support doing this (all panels must be identical) which I didn’t realize until I was far along in the project. I could have started over, but I chose to work with this issue instead. After a lot of trial and error, and many attempts at different ways around this issue (manually configuring LED ordering, multiple outputs from one microcontroller, etc.) I finally decided that it would be best to add a second ESP32 microcontroller to handle the top rectangular panel. The two panels/microcontrollers can be synced via a UDP connection, and the first microcontroller transmits audio data to the second so that only one mic is necessary and the sound response is synced. This is why the top of the panel shows a separate animation from the bottom square, and I actually think this effect is nice if the right animations are chosen, so ultimately it turned out well I think.

Microphones - Another issue I ran into was with my choice of microphone module. At first I was using an LM393 based analog mic which converted sound to an analog output. This did not work very well for my application because it only responded to changes in volume when a threshold was met. This severely limited the panel's capacity for music reactivity. After doing some research I decided to switch to an INMP441 MEMS microphone. This worked significantly better because the module communicated via an I2S (digital) connection, which greatly expanded the amount of data available to influence the animations.

Network Connection - I had a lot of trouble getting the network connection to be stable and the ESP32s were constantly dropping connection, which was annoying and also made it difficult to control the panel. Part of this was due to my home network 2.4ghz band being weak in the room of the house I was working in. I discovered it was also due to a Wifi “sleep” function which I have now disabled on the ESP32s. To solve the issue of poor connection I got a Wifi range extender and set up a dedicated 2.4ghz network for the ESP32s. I also had to assign static IPs to each of the microcontrollers. They maintain connection now and I have had no further issues with connectivity. 

Panel Wiring/Ordering - I had some trouble getting the panels wired in the correct order so that animations would display correctly. I finally realized it was because my panels were wired vertically instead of horizontally (the way most panels are wired). After learning this I wired them in a vertical serpentine fashion and this worked.

Lessons Learned:
ALWAYS DOUBLE CHECK YOUR CONNECTIONS - On one occasion I fried four LED panels and an Arduino UNO by accidentally connecting a 12V power supply to a 5V system. On another occasion I fried an ESP32 Microcontroller by unknowingly connecting it to a faulty power cable. My main takeaway from this whole project is to always double and triple check your power supply/connections before testing on a system. This lesson was learned the hard way.

Moving Forward:
Animations - While I did not write the base code for the animations I have demonstrated, I did do a lot of configuration to get them to respond in the manner displayed (tweaking input levels, speed, sensitivity, X and Y values, mirroring, etc.) I am working on writing code for my own animations which can then be uploaded to WLED and used on the panel. Since WLED is open source I am able to have full access to the code for animations available as examples, and I can also edit the code in any way I like. I will be uploading my own animations to GitHub when complete.

Remote Access - As it currently stands the LED panel control interface is only available on my local network. In the future though I may configure it to allow remote access using port forwarding so that the panel could be controlled over the internet. I have no personal practical uses for something like this, but I think it would be cool to set up.
