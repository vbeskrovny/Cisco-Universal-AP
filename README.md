# Easy Cisco Universal AP Priming procedure

How to prime universal Cisco Access Point

I spent two full days on reverse engineering of the Priming process for the Universal Access Points made by Cisco. I think it worth to share my experience once you face the same issue.

I will not describe the network lab I built for the cracking procedure, all the steps with decoding SSL, DTLS, e.t.c. Let's go straight to the priming.

# Prerequesities

You should have alredy pre-configured:
1. WLC
2. SSID with PSK (the easiest way) attached to the AP group (I used "Priming")
3. Universal AP assigned to the AP group (I used "Priming")
4. Laptop should be connected to the broadcasted by Unprimed AP SSID
5. Open AP settings on WLC, go to the second tab after "General" -> "Credentials"
6. Create custom authentication credentials for the AP, such as User:Secret1234

# Verification

1. Ensure that you connected to the SSID broadcasted by Unprimed AP
2. Try to ping IP address of the Unprimed AP (let's assume the IP is: 1.2.3.4)
3. Open in browser: http://1.2.3.4/get_universal_ap_reg_domain.xml -> you should be able to see the info of the AP

# Implementation

Now it starts to be a bit tricky. You need to send HTTP POST request as follows:

URL: http://1.2.3.4/set_universal_ap_reg_domain.shtml
Params: configMode=1&location=DE&regDomain5=-E&regDomain24=-E
Basic authentication data: User:Secret1234

For the network stuff I work mostly in Linux, so it is easy as invoking the CURL command:

curl -v -X POST "http://1.2.3.4/set_universal_ap_reg_domain.shtml?configMode=1&location=DE&regDomain5=-E&regDomain24=-E" -u "User:Secret1234"

PS: in our case I am using "DE" as a country which I am going the prime the AP for. Feel free to use any country you like: US/DE/FR/SE/e.t.c. Pay attention to the regDomain param value: it is -E for Europe. You probably have to adapt it for your region as well.

PPS: Starting from now you can open the following link: http://htmlpreview.github.io/?https://github.com/vbeskrovny/Cisco-Universal-AP/blob/master/prime.html on your smartphone, then connect to the SSID, fill in the data and click "Prime it" button...

Cheers ;)
