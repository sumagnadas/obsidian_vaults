
# Request format
**First line** => Request-Line = **Method** SP **Request-URI** SP **HTTP-Version** CRLF(\n)
	Method             = one of {`GET`, `HEAD`, `POST`, etc}
	Request-URI    = where we want the data received from/sent to
	HTTP-Version  = Which version of HTTP the request is following.
## Methods
### GET
**GET** method retrieve whatever there is in the location of the Request-URI. If the Request-URI is a data-producing service instead of a data resource, then return the OUTPUT of the data-producing service, instead of the source text of the service. (unless the service returns its own source code, then return it).
### HEAD
**HEAD** method is basically a GET request but without a body. It works same as GET and returns only the header information of the response without the content body (Entity-Body).
### POST
**POST** method is for sending data to the server to the location of the Request-URI, which processes it and does something like:-
- Append to a database.
- Post on a notice board (on web), etc
It has a request body, which gives it the required data, that can be form data or even an image. Since it has a content body, it requires the Content-Length header (kinda, but not exactly mandatory).

---
# Response format
First line => Status-Line = **HTTP-Version** SP **Status-Code** SP **Reason-Phrase**
	HTTP-Version  = Which version of HTTP the server is responding back in.
	Status-Code    = what happened with the request after receiving i.e. was it accepted or rejected, etc
	Reason-Phrase = Tell the reason of the code in few words
## Status Codes
- **1xx**:  *Informational* -> Not used, but reserved for future use
- **2xx**: *Success* -> Request was successfully received, understood and accepted
	- **200**: OK => the server found the correct resource and returned it
	- **201**: Created
	- **202**: Accepted
	- **204**: No content
- **3xx**: *Redirection* -> Further action must be taken in order to complete the request. (Example: 301 Permanent Redirect tells the client that the server does not have the requested service in the same location anymore and then tells the location to go to for that request)
	- **301**: Moved Permanently => the resource you are searching for has moved and will never be here again. you might want to check in the location it moved to.
	- **302**: Moved Temporarily => the resource you are searching for has moved and will be here again sometimes in the future. until then, you might want to check in the location it moved to.
	- **304**: Not Modified
- **4xx**: *Client Error* -> The request has bad syntax or is an invalid request
	- **400**: Bad Request
	- **401**: Unauthorized
	- **403**: Forbidden
	- **404**: Not Found
- 5xx: *Server Error* -> The server couldn't fulfill what the request wanted
	- **500**: Internal Server Error
	- **501**: Not implemented
	- **502**: Bad Gateway
	- **503**: Service Unavailable
*Note*: SP = space 

## Body
- ***Form data format***: URL encoded => Example: `name=Sumagna%20Das&age=15`
