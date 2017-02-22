Client to server keep-alives should not be sent more often than once every 
20 seconds, unless intra-connection meta-data is being conveyed inside the 
'# ...' keep-alive line. It is suggested that clients send a keep-alive line 
at least once every 24h, with typical rates in the 10m-1h range. Client to
server keep-alives are purely optional; servers may instead choose to 
implement client time-out based on their L4 TCP behavior, and clients may 
instead choose to tolerate being periodically dropped from the server as an 
inactive connection.  
