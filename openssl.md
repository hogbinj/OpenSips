
Run
openssl s_client -showcerts -debug -connect [dns address of opensips]:[tls port] -no_ssl2 -bugs

The /etc/opensips/tls/user-cert.pem Certificate may need you to copy in the intermediate certificate.  
It goes 2nd place in the file
