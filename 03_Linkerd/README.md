# Securing Pod-to-Pod Communication with mTLS 

mTLS :- Mutual TLS 

Mutual TLS provides authentication and encryption to communication between pods. Both the client and the server have a certificate, and each validate the identity of the other. Verification is essetial, as an encrypted connection with a malicious party isn't going to help you much. 

The identity component is responsible for generating certificates for proxies when they ae created. The proxy creates a Certificate Signing request, or CSR and sends it, alongwith the service account token, to the identity component via it's API endpoint. 

The identity service validates the token to verify the proxy's identity and then creates a certificate for that proxy. The identity component then sends the signed certificate back to the proxy for it to use. 

These certificates expire after 24 hours, at which point the linkerd proxy asks for a new one. This process minimizes the damage from certificate leaks. By rotating certificates, you know that any leaked credentials useless after 24 hours. 

This process ensures all eshed pods communicates using TLS. It enables a zero trust architecture. All services know whow they are talking to.   

Linkerd has one overriding principle for security: 
Keep it simple :- The makers of Linkerd believe that complexity is the enemy of security. The more complex feature is to use, weather it's tons of options or complicated bells and whistles, the less likely you are to use it. They have put this principle into practice by making Mutual TLS the default for meshed pods. This is good news for administrators. You don't have to configure Mutual TLS at all. Linkerd turns it on by default. Install Linkerd and mesh some pods, and they will be protected with mTLS automatically. 