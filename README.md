# SSLPinningSample

How to make your iOS apps more secure with SSL pinning?

At a glance
SSL (Secure Socket Layer) ensures encrypted client-server communication over HTTP - specified by HTTPS (HTTP over SSL). The encryption is based on PKI (Public Key Infrastructure) and a session key. The session key was introduced because encrypting and decrypting a public/private key uses a lot of processing power and it would slow down the whole communication process.

Instead of having to asymmetrically encrypt data at the source and decrypt it at the destination, a symmetric session key, which is exchanged with the SSL handshake when the communication starts, is used.

SSL SECURITY - IDENTIFICATION
The security aspect of SSL is based on the certificate's "chain of trust". When the communication starts, the client examines the server's SSL certificate and checks if the received certificate is trusted by the Trusted Root CA store or other user-trusted certificates.

MITM
Although SSL communication is considered pretty much secure and unbreakable in realistic time frames, the man-in-the-middle attack still poses an actual threat. It can be carried out using several methods, which include ARP cache poisoning and DNS spoofing.

With ARP cache poisoning, it is possible to carry out an MITM attack by using the nature of the Address Resolution Protocol which is responsible for mapping the IP address to the device's MAC address.

For example, let's describe a simple network containing these 3 main actors:

a common user's device U
the attacker's device A
and the router R
Device A can send the ARP reply packet to the device U, introducing itself as the router R. To complete the MITM attack, A sends another ARP reply to R introducing itself as the device U. In this way, the attacker's device A is in the middle of the communication between the device U and router R and it can eavesdrop or block it. IP forwarding is often used on the attacker's device to keep the communication flowing seamlessly between the user's device and router.

DNS spoofing includes a broad range of attacks aimed at corrupting the name server's domain name mapping. The attacker tries to find a way to force the DNS to return an incorrect IP address and divert traffic to their computer.

SSL pinning
We use SSL pinning to ensure that the app communicates only with the designated server itself. One of the prerequisites for SSL pinning is saving the target's server SSL certificate within the app bundle. The saved certificate is used when defining the pinned certificate(s) upon session configuration.

We will be covering SSL pinning using NSURLSession, AlamoFire and AFNetworking (using AFHTTPRequestOperationManager).

NSURLSESSION
Things are a bit more tricky when it comes to NSURLSession SSL pinning. There is no way to set an array of pinned certificates and cancel all responses that don't match our local certificate automatically. We need to perform all checks manually to implement SSL pinning on NSURLSession. We'll happily use some of the Security's framework C API (like all other true hackers do).

We can start by instantiating an NSURLSession object with the default session configuration.


Reference: https://infinum.co/the-capsized-eight/how-to-make-your-ios-apps-more-secure-with-ssl-pinning
