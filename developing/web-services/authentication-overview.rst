Authentication Overview
=======================

When a client website makes a read or write request to the warehouse's web 
services, it is necessary to authenticate the request. In an ideal world a 
secure connection would be used so that the password can be passed between the 
client and server to authenticate. However, in Indicia there is no guarantee 
that the server will be on a secure socket layer so passing a password across 
the connection would be insecure.

Therefore, Indicia uses digest authentication, in which the following steps are
taken:

# The client requests a public token from the server, known as a nonce.
# The server generates the public token and returns it to the client.
# The client combines the public token with the password and then hashes this 
  combined token using a non-reversible algorithm.
# The client sends this hashed token with the public token when performing the
  web-services request which is to be kept secure.
# The server receives the request and combines the public token with the 
  expected password, then hashes it using the same algorithm as the client.
# This hashed token is compared with the one sent from the client to determine
  if the client has sent the correct token and therefore knows the password. 
  Note that at no point does the password get sent across the connection so this
  approach is safe for insecure connections to web services.