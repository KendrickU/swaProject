### Group: 6b87bd7cbe948ef26891defd0258c91d1126beb506ce0da4fc5f6dc7467fb26c 
# Assurance Cases

The context for these assurance cases is that Certbot is being used by a bank to manage their TLS certificates<br>

-----
Below are three top level assurance claims with accompanying diagrams: 
<br>

* Certbot resists attacks to subvert the certificate acquisition process
* Certbot prevents the unauthorized revocation of certificates 
* Certbot storage mechanism prevents unauthorized disclosure

There is an additional claims which does not warrant a separate diagram
* Certbot resists attacks to subvert certificate renewal processes 
  * This is similar to the claim for obtaining certificates

We have found that Certbot does not have enough features to properly cover 5 assurance claims. At Certbot's core, a certificate can be acquired, stored, revoked,  and renewed. Certbot also allows specific plugin functionality in order to accommodate certain features to work well with Certbot. This is why we have 4 claims.

----

## Certbot resists attacks to subvert the certificate acquisition process

We chose our first claim, Certbot resists attacks to subvert the certificate acquisition process, 
because it is one of the core functions of Certbot. Acquiring certificates that mitigate attacks are vital to
systems such as bank systems because it ensures upmost security. It allows an extra layer of security which makes various
entities believe that the Certbot system will work as expected.

!["Diagram 1"](https://www.lucidchart.com/publicSegments/view/e3663a71-90b1-421d-8424-fe667a11c9ff/image.png)
 
* Top Claim C1 is derived from a standard use case of Certbot. It is designed to securely handle obtaining certificates from the Let's Encrypt certificate authority.  
* R1 attempts to contradict C1 by proposing that a tampered or malicious version of Certbot may have been 
installed by accident due to a less than stringent adherence to best installation practices outlined in the Certbot documentation 
  * Claim C1 refutes this, as installing Certbot from official sources would largely mitigate R1 by ensuring a legitimate installation of Certbot 
* R2 and R3 outlined the possibility that obtaining Certbot from source could be subject to attack, as 
the source repositories may be compromised through a well-targeted phishing attack against Certbot itself (and subsequent changes being made to the source code), or the recipient's specific Certbot download via over the wire manipulation of the source files, possibly through a TLS downgrade attack or ARP spoofing. 
  * C2 refutes R2 in that source code changes are monitored and peer-reviewed. Any malicious changes would be readily apparent, and more than likely corrected before a significant percentage of Certbot users are affected 
 
!["Evidence 1"](https://cdn.discordapp.com/attachments/481986866162302976/500054533527830528/evidence_for_claim1_evidence_2.png)
 
  * Further evidence lies in the frequently updated issue remediation section on the official Certbot Github repository 
 
!["Evidence 2"](https://cdn.discordapp.com/attachments/481986866162302976/500055699016122379/Evidence_Frequently_updated_issue_remediation_suggestions_in_github_repository..png)
 
  * C3 repudiates R3 in that Certbot communicates with Let's Encrypt over TLS, greatly increases the complexity of attacks against Certbot's ability to obtain certificates from Let's Encrypt 
  * This would be evident from TLS-secured packet capture on the machine hosting Certbot, sending requests to  Let's Encrypt. 

!["TLS Evidence"](https://cdn.discordapp.com/attachments/481986866162302976/500496155294171176/tls_certbot_wireshark.png)
 
  * However, one undermining point against this evidence would involve somehow brute forcing or otherwise obtaining cryptographic secrets used during the handshake phase of TLS traffic between Certbot and Let's Encrypt. This could result in a compromised channel of communication, and thus the malicious tampering of certificates from Let's Encrypt 
  * C5 refutes this, as a new shared secret upon every handshake, as defined in RFC 8446 (TLS 1.3, found at https://tools.ietf.org/html/rfc8446), outlines that a new secret would be generated upon every TLS request, thus making the above attack infeasible for any meaningful purpose.  
 
!["Evidence 4"](https://cdn.discordapp.com/attachments/481986866162302976/500495699859734528/TopClaim1E4.PNG)

  * The Inference Rule derived from claim C1 states that if the previously described rebuttals are mitigated, as described and exemplified with relevant evidence, then Certbot will reliably be able to obtain certificates. 
  * Unless a 0-day that allows for Certbot's correspondence with Let's Encrypt becomes widely known. 
  * Nevertheless, Certbot actively remediates issues as shown on the official Github repositories, thus reducing the effectiveness of a powerful 0-day 
 
----------------------------------------------- 

## Certbot prevents the unauthorized revocation of certificates

Our second claim, Certbot prevents the unauthorized revocation of certificates, is important as it brings attention to the 
detrimental effects of what could happen if a user is given unauthorized access to revoking certificates. This may include malicious users
having the ability to revoke valid certificates and malicious users who refuse to revoke invalid certificates in order to try to trick the system into believing that these certificates are valid. Preventing the authority to revoke certificates becomes very important as the system's architecture is strengthened.

!["Diagram 2"](https://www.lucidchart.com/publicSegments/view/cd5b6e10-743f-49c2-9a7f-f82f65ef04b1/image.png)
 
* Top Claim C1 postulates that Certbot's certificate storage features prevent the unauthorized revocation of certificates. Certificates are stored  
  * R1 refutes this claim with the suggestion that this certificate storage feature impersonates Let's Encrypt, which would in turn disallow the revocation of certificates 
* However, C1 mentions the OCSP protocol used to revoke certificates. The attacker would need to account for altered OCSP responses being accepted by Certbot, along with DNS records 
* R2 states that the OCSP protocol is still vulnerable to replay attacks. This is especially possible with DNS-based attacks, as previously accepted OCSP responses from Let's Encrypt could be leveraged by an attacker to deny or allow certificate revocation at their command. 
  * C2 refutes R2, as OCSP can be extended with OCSP stapling (https://tools.ietf.org/html/rfc6960, page 18-19). This is a security extension to the OCSP protocol that sends CA-signed timestamps with OCSP responses, which are then validated by a third-party OCSP server. This is similar in principal to the Kerberos authentication mechanism in that the CA is not contacted directly upon verification of the timestamps, thus mitigating risks associated with a compromised CA for certificate revocation 
* Evidence for this would be signed timestamps obtained from OCSP servers 
         
!["Evidence 1"](https://cdn.discordapp.com/attachments/481986866162302976/500495719229161472/TopClaim2E1.PNG)
 
  * C3 repudiates R2 in a different manner. Instead of a signed timestamp being verified by a third party, a cryptographic nonce is sent with requests and responses between the OCSP server and Certbot. Because the nonce differs on each request/response, replay attacks are far more difficult to pull off. 
  * Evidence for this comes in the form of the `id-pkix-ocsp-nonce` object included in revocation responses/requests (4.4.1, RFC 6960) 

!["Evidence 2"](https://cdn.discordapp.com/attachments/481986866162302976/500499241811312640/unknown.png)

----------------------------------------------- 

## Certbot certificate storage mechanism prevents unauthorized disclosure.

The third claim, Certbot storage mechanism prevents unauthorized disclosure, is important as it shows that Certbot has functionality and control with how certificates are stored. A malicious user who has the ability to read certificates to modifying these certificates in an unauthorized manner weakens the assurance of the system. Certbot provides excellent assurance and it does the job it was designed well because it has a mechanism in place to securely store these certificates. 

!["Diagram 3"](https://www.lucidchart.com/publicSegments/view/1a366405-1600-4faa-a0d6-2e83515bc756/image.png)       
 
* Top Claim C1 claims that Certbot prevents unauthorized disclosure of certificates obtained from Let's Encrypt. By default, certificates are stored in the `/etc/letsencrypt/` directory on Linux systems, which is accessible only by root in most environments unless other users are given similar permissions for this directory. Modifying these permissions or locations can be done with "hook scripts", supplied as command line arguments when renewing or obtaining a certificate
  *  However, R1 claims that should this mechanism be exploited, and thus the security of certificate storage can be compromised. This can potentially be done in various ways. For example, changing various parameters in the hook scripts (should the hook scripts be stored in a less than secure location, like /tmp), or changing directory permissions of the /etc/letsencrypt directory through a filesystem vulnerability 
  * Claim C1 refutes R1. Certbot has a command line parameter called "strict_permissions", which ensures that all configuration files inherit the permissions of the user issuing the Certbot command.  
 
!["Evidence 1"](https://cdn.discordapp.com/attachments/481986866162302976/500495729144496129/TopClaim3E1.PNG)
 
  * R2 shows how lax installation procedures when setting up initial storage locations could result in insecure certificate storage.  
  * However, C2 argues against R2, as the service accounts used to install Certbot should have sufficiently high permissions, as per best practices during Certbot's installation. 
  * R3 goes against C2. In Windows, an unquoted service path containing file or directory names with spaces could result in an attacker abusing filesystem vulnerabilities to compromise certificates. For example, if the icacls utility for Windows lists Certbot's executable as residing at `C:\Program Files\Certbot.exe` instead of `"C:\Program Files\Certbot.exe"`, then an attacker could drop a malicious executable in `C:\Program EvilDirectory\cerbot.exe`. If run with the same permissions as the legit Certbot executable - as would probably be the case if certificate revocation and renewal is done via an automated service, then this executable will be able to pull certificates from the "secure" storage location and send them to the attacker, hypothetically. 
  * Claim C3 argues against this. If proper installation instructions are followed, then Certbot should be installed in the `C:\inetpub\` directory, or a similar directory name without spaces in the name. This would prevent unquoted service path attacks using Certbot, even if the path is unquoted. 
         
!["Evidence 2"](https://cdn.discordapp.com/attachments/481986866162302976/500495738279690260/TopClaim3E2.PNG)
         
-----------------------------------------------

## Certbot resists attacks to subvert certificate renewal processes  
-----

We chose our fourth claim, Certbot resists attacks to subvert certificate renewal processes, 
because it is one of the core functions of Certbot. Acquiring certificates that mitigate attacks are vital to
systems such as bank systems because it ensures upmost security. Being able to renew the certificates are essential as it adds continuous assurance that
the system will work as expected.

This assurance case does not have an accompanying diagram. This is because this claim is largely similar to the first outlined assurance case concerning certificate revocation. At Certbot's core, a certificate can be stored, revoked, obtained, and renewed. The latter two functionalities are largely encompassed in the diagrams and claims shown, and as such the corresponding claims and rebuttals can be applied to them as well.  


## Github and Kanban 
----- 
https://github.com/KendrickU/swaProject<br> 
https://github.com/KendrickU/swaProject/projects/4<br> 


## Sources
-----
https://en.wikiversity.org/wiki/Wireshark/HTTPS
