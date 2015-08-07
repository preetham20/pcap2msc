Purpose of this project is to generate mscgen-compatible (http://www.mcternan.me.uk/mscgen/) files based on pcap files.

It depends on tshark (http://www.wireshark.org/) and mscgen if you want to draw the message sequence chart.

**How does it work?**
tshark output is read line by line. A simple regex is applied on every line. Tshark gives a good one-line summary of packet, hence pcap2msc uses it to be written on the arrow standing for an IP packet.

**Why Python?**
No real rationale, but a clean and easy to learn language.

**How do I use it?**
Got avses-tosip.cap from pcapr (http://pcapr.net/home).

$ pcap2msc ./avses-tosip.cap sip | mscgen -T png -o avses-tosip.png

This generates PNG sequence chart from cap file. pcap2msc uses tshark behind the scenes.

**What mscgen format looks like?**
```
msc {
  u0[label="148.147.33.67"],u1[label="148.147.33.5"],u2[label="148.147.33.65"];
  u0=>u1 [ label = "SIP Request: SUBSCRIBE sip:3521@mtcsv.avaya.com" ] ;
  u0<=u1 [ label = "SIP Status: 202 Accepted" ] ;
  u0<=u1 [ label = "SIP Request: NOTIFY sip:3521@148.147.33.67;transport=tcp" ] ;
  u0<=u1 [ label = "SIP Request: NOTIFY sip:3521@148.147.33.67;transport=tcp" ] ;
  u0=>u1 [ label = "SIP Status: 200 OK" ] ;
  u0=>u1 [ label = "SIP Status: 200 OK" ] ;
  u0=>u1 [ label = "SIP Request: SUBSCRIBE sip:3521@mtcsv.avaya.com" ] ;
  u0<=u1 [ label = "SIP Status: 202 Accepted" ] ;
  u0<=u1 [ label = "SIP Request: NOTIFY sip:3521@148.147.33.67;transport=tcp" ] ;
  u0<=u1 [ label = "SIP Request: NOTIFY sip:3521@148.147.33.67;transport=tcp" ] ;
  u0=>u1 [ label = "SIP Status: 200 OK" ] ;
  u0=>u1 [ label = "SIP Status: 200 OK" ] ;
}
```

**Is final output pretty?**

![http://pcap2msc.googlecode.com/files/avses-tosip.png](http://pcap2msc.googlecode.com/files/avses-tosip.png)

**How could I force tshark to decode tcp on port 31416 as HTTP?**

tshark options can be given on command line between pcap file and display filter. To force what has been asked, use

$ pcap2msc ./bug.pcap -d tcp.port==31416,http ip > bug.msc