# Hack The Box Bug Bounty Hunter Path Notes:

## Module 1: WEB REQUESTS (Basics of the Basics)

##		Module1Topic1 (M1T1): HTTP - an application-level protocol used to access WWW. HTTPS is the successor with an S(ecured).
			Flaw: all communication is transferred in plain-text format

					MT1H1: URL (structure) with example

							http://admin:password@example.com:80/dashboard.php?login=true#status

							http:// | https://  -	scheme(protocol)
							admin:password		-	user
							exmaple.com 		-	host
							:80					-	port
							/dashboard.php		-	path
							?login=true			- 	query string
							#status				-	Fragments (processed by browsers on the cliend-side to locate secitons within primary resources.)

					M1T1H2: HTTP Flow (textual)
							Image : https://academy.hackthebox.com/storage/modules/35/HTTP_Flow.png

						Steps: 
							User provides a domain (through browser) >> browser queries the DNS server to location of the domain >> DNS server provides an IP address >> Browser makes a GET request to the recieved IP >> Web Server of the domain(IP) responds accordingly with the web page >> Webpage loads.
		
						Note:
							Before querying the DNS server, the browser looks for the domain in the local "hosts" file (/etc/hosts) to save the processing time and if not present, then DNS server
							Records can be manually added in the hosts file for DNS resolution.
							Location: 	Linux   : /etc/hosts
										Windows : C:\Windows\System32\drivers\etc\hosts

					M1T1H3: cURL (client URL)
						a) Tool to send web requests to Web server in cli(terminal). 
						b) Does not render Js/CSS/HTML for faster loading and easy-to-read request-response data.
						flags: 
							1. -O : download the page/file. Takes the name of remoe file and stores it on the local machine.
							2. -o : similar to -O but the name of file needs to be specified.
							3. -h : open the help menu
							4. -A : specify the User-Agent
		


## M1T2 : HTTPS: Overcomes the flaw of HTTP by encrypting all the communications. (https://)
        	.
					M1T2H1 : HTTPS Flow:
						Image : https://academy.hackthebox.com/storage/modules/35/HTTPS_Flow.png

						Steps: 
							If we type http:// instead of https:// to visit a website that enforces HTTPS, the browser attempts to resolve the domain and redirects the user to the webserver hosting the target website. A request is sent to port 80 first, which is the unencrypted HTTP protocol. The server detects this and redirects the client to secure HTTPS port 443 instead. This is done via the 301 Moved Permanently response code, which we will discuss in an upcoming section.

							Next, the client (web browser) sends a "client hello" packet, giving information about itself. After this, the server replies with "server hello", followed by a key exchange to exchange SSL certificates. The client verifies the key/certificate and sends one of its own. After this, an encrypted handshake is initiated to confirm whether the encryption and transfer are working correctly.

							Once the handshake completes successfully, normal HTTP communication is continued, which is encrypted after that. This is a very high-level overview of the key exchange, which is beyond this module's scope.
		
					M1T2H2 : cURL for HTTPS: 
						cURL https://example.com (Goes ahead and check for Certificate, -k to skip the check)
		
		

## M1T3: HTTP Requests and Responses: In layman Termns - The client ( browser) sends the request 	and receive the response. Then the browser renders the Js/CSS/HTML.


					M1T3H1: HTTP Request
						Image: https://academy.hackthebox.com/storage/modules/35/raw_request.png

						Contents of a request: for a GET request to 		http://inlanefreight.com/users/login.html
						the request contains: Line 1: GET 	/users/login.html 	HTTP/1.1
													(method) 	(Path)			Version
		
						and then the contents of the request (Headers and body of the page)
		
						Note: HTTP version 1.X sends requests as clear-text, and uses a new-line character to separate different fields and different requests. HTTP version 2.X, on the other hand, sends requests as binary data in a dictionary form.
		
		
					M1T3H2: HTTP Responses
						Image: https://academy.hackthebox.com/storage/modules/35/raw_response.png

						Contents of a response: The servers sends a request in the format
		
													HTTP/1.1	 		200	OK
													(Version)		(status code)			
					
					M1T3H3: cURL
						the difference b/w browser and cURL is that browser renders the request and response for the user whereas cURL prints the raw request/response in the temrinal for easier reading.

					M1T3H4: Browser DevTools
						Most modern web browsers come with built-in developer tools (DevTools), which are mainly intended for developers to test their web applications. However, as web penetration testers, these tools can be a vital asset in any web assessment we perform, as a browser (and its DevTools) are among the assets we are most likely to have in every web assessment exercise. In this module, we will also discuss how to utilize some of the basic browser devtools to assess and monitor different types of web requests.
						
## M1T4: HTTP Headers	

				Headers can have one or multiple values, appended after the header name and separated by a colon. We can divide headers into the following categories:

					1. General Headers
					2. Entity Headers
					3. Request Headers
					4. Response Headers
					5. Security Headers
					
					M1T4H1: General Headers (used in both HTTP request and response)
					
						Data	    : Holds date+time at which message originated
						Connection  : Dictates if current network should stay alive
								   
								   
					M1T4H2: Entity Headers (Used in both HTTP request and response to describe the message rather than its contents transferred by message)
					
						1. Content-Type   : Used to describe the type of resource
						2. Media-Type	  : similar to Content-Type. Can play a crucial role in making the server interpret the input. The charset field may also be used with this header.
						3. Boundary	  	  : Acts as marker to separate content when there is more	than one message
						4. Content-Length : The type of encoding being used should be specified using the Content-Encoding header.
						
					M1T4H3: Request Headers
					
						1. Host		  :Used to specify the host being queried for the resource. 
						2. User-Agent : used to describe the client requesting resources
						3. Referer	  : Denotes where the current request is coming from.
						4. Accept	  : describes which media types the client can understand
