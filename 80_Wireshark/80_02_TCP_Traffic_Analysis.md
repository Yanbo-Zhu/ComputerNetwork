
# 1 a 
(a) In Wireshark, start a traffic capture on your local network interface.
Load the web page https://www.tu.berlin/.
Wait for 10 seconds, then stop the capture.
How many packets do you get in total?
How many of them contain TCP? (Hint: Use a display filter.)   display filter: tcp 
How many of them contain HTTP/1.1? (Hint: Use a display filter.)   `http && http.request.version == "HTTP/1.1"`
Include the display filters you use in your solution.



# 2 b 
(b) Now set a display filter to show only packets that correspond to a single TCP connection of your web page request and response.
Include the display filter you use in your solution and briefly explain what it means.


# 3 c
(c) Analyze the obtained data by marking packets belonging to:
• the TCP connection setup
• the transmission of the HTTP request,
• the transmission of the HTTP response
• the tear-down of the connection.
Include a marked screenshot of Wireshark in your solution.


# 4 d

1
Start a new traffic capture and load the page https://www.tu.berlin/.

2
Set a display filter for DNS, and find the queries for tu.berlin. 

dns.qry.name == "tu.berlin"

3
What records does the browser ask for, and what replies does it get?

4
Click on one of the DNS replies and find the IP address in the Answers section. Remove the DNS display filter again, and look at the packets coming afterwards to and from one of the IP addresses associated with https://www.tu.berlin/. (Hint: You can use a display filter.)

5 
What TCP connections do you see? What can you see in them, and what can you not see?
Why?
(No need for a screenshot here, if you can explain it in text.)

