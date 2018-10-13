### Group: 6b87bd7cbe948ef26891defd0258c91d1126beb506ce0da4fc5f6dc7467fb26c 
# Assurance cases 
----- 
Below are three top level assurance claims with accompanying diagrams: 
<br><br> 
* Certbot resists attacks to subvert the certificate aquisition process 
* Certbot prevents the unauthorized revocation of certificates 
* Certbot storage mechanism prevents unauthorized disclosure 
<br> 
There is an additional claims which does not warrant a seperate diagram 
 
* Certbot resists attacks to subvert certificate renewal processes 
        * this is similar to the claim for obtaining certificates 
<br><br> 
## Certbot resists attacks to subvert the certificate acquisition process  
-----         
* The context for these assurance cases is that certbot is being used by a bank to manage their TLS certificates 
 
* Top Claim C1 is deried from a standard use case of certbot. It is designed to securely handle obtaining certificates from the Let's Encrypt certificate authority.  
* R1 attempts to contradict C1 by proposing that a tampered or malicious version of certbot may have been 
installed by accident due to a less than stringent adherence to best installation practices outlined in the certbot documentation 
* Claim C1 refutes this, as installing certbot from official sources would largely mitigate R1 by ensuring a legitimate installation of certbot 
* R2 and R3 outlined the possibility that obtaining cerbot from source could be subject to attack, as 
the source repositories may be compromised through a well-targeted phishing attack against certbot itself (and subsequent changes being made to the source code), or the recipient's specific cerbot download via over the wire manipulation of the source files, possibly through a TLS downgrade attack or ARP spoofing. 
        * C2 refutes R2 in that source code changes are monitored and peer-reviewed. Any malicious changes would be readily apparent, and more than likely corrected before a significant percentage of certbot users are affected 
 
EVIDENCE GOES HERE 
 
        * Further evidence lies in the frequently updates issue remediation section on the official Certbot github repository 
 
EVIDENCE GOES HERE 
 
        * C3 repudiates R3 in that certbot communicates with Let's Encrypt over TLS, greatly increases the complexity of attacks against certbot's ability to obtain certificates fro Let's Encrypt 
        * This would be evident from TLS-secured packet capture on the machine hosting certbot, sending requests to  Let's Encrypt. 
 
EVIDENCE GOES HERE 
        * however, one undermining point against this evidence would involve somehow bruteforcing or otherwise obtaining cryptographic secrets used during the handshake phase of TLS traffic between cerbot and Let's Encrypt. This could result in a compromised channel of communication, and thus the malicious tampering of certificates from Let's Encrypt 
        * C5 refutes this, as a new shared secret upon every handshake, as defined in RFC 8446 (TLS 1.3, found at https://tools.ietf.org/html/rfc8446), outlines that a new secret would be generated upon every TLS request, thus making the above attack infeasible for any meaningful purpose.  
 
 
EVIDENCE GOES HERE 
 
        * the Inference Rule derived from claim C1 states that if the previously described rebuttals are mitigated, as described and exemplified with relevant evidence, then certbot will reliably be able to obtain certificates. 
        * Unless a 0-day that allows for certbot's correspondence with Let's Encrypt becomes widely known. 
        * Nevertheless, Certbot actively remediates issues as shown on the official github repositories, thus reducing the effectiveness of a powerful 0-day 
 
NO EVIDENCE CUZ OF BLACK DIAMOND OF UNCERTAINTY  
 
 
----------------------------------------------- 
<br><br> 
## Certbot prevents the unauthorized revocation of certificates  
-----         
 
* Top Claim C1 postulates that Certbot's certificate storage features prevent the unauthorized revocation of certificates. Certificates are stored  
 
        * R1 refutes this claim with the suggestion that this certificate storage feature 
impersonates Let's Encrypt, which would in turn disallow the revocation of certificates 
* However, C1 mentions the OCSP protocol used to revoke certificates. The attacker would need to acocunt for altered OCSP responses being accepted by certbot, along with DNS records 
* R2 states that the OCSP protocol is still vulnerable to replay attacks. This is especially possible with DNS-based attacks, as previously accepted OCSP responses from 
Let's Encrypt could be leveraged by an attacker to deny or allow certificate revocation at their command. 
 
        * C2 refutes R2, as OCSP can be extended with OCSP stapling (https://tools.ietf.org/html/rfc6960, page 18-19). This is a security extension to the OCSP protocol that 
        sends CA-signed timestamps with OCSP responses, which are then validated by a third-party OCSP server. This is similar in principal to the Kerberos authentication                 mechanism in that the CA is not contacted directly upon verification of the timestamps, thus mitigating risks associated with a compromised CA for certificate revocation 
        * Evidence for this would be signed timestamps obtained from OCSP servers 
         
EVIDENCE GOES HERE 
 
        * C3 repudiates R2 in a different manner. Instead of a signed timestamp being verified by a third party, a cryptographic nonce is sent with requests and responses between the OCSP server and certbot. Because the nonce differs on each request/response, replay attacks are far more difficult to pull off. 
        * Evidence for this comes in the form of the `id-pkix-ocsp-nonce` object included in revocation responses/requests (4.4.1, RFC 6960) 
 
EVIDENCE GOES HERE 
  
 
----------------------------------------------- 
<br><br> 
## Certbot certficate storage mechanism prevents unauthroized disclosure.  
-----         
 
* Top Claim C1 claims that cerbot prevents unauthorized disclosure of certificates obtained from Let's Encrypt. By default, certificates are stored in the `/etc/letsencrypt/` directory on Linux systems, which is accessible only by root in most environments unless other users are given similar permissions for this directory. Modifying these permissions or locations can be done with "hook scripts", supplied as command line arguments when renewing or obtaining a certificate 
 
        *  However, R1 claims that should this mechanism be exploited, and thus the security of certificate storage can be compromised. This can potentially be done in various ways. For example, changing various parameters in the hook scripts (should the hook scripts be stored in a less than secure location, like /tmp), or changing directory permissions of the /etc/letsencrypt directory through a filesystem vulnerability 
        * Claim C1 refutes R1. Certbot has a command line parameter called "strict_permissions", which ensures that all configuration files inheret the permissions of the user issuing the Certbot command.  
 
        EVIDENCE GOES HERE 
 
        *  R2 shows how lax installation procedures when setting up initial storage locations could result in insecure certificate storage.  
        * However, C2 argues against R2, as the service accounts used to install certbot should have sufficiently high permissions, as per best practices during Certbot's installation. 
        * R3 goes against C2. In Windows, an unquoted service path containing file or directory names with spaces could result in an attacker abusing filesystem vulnerabilities to compromise certificates. For example, if the icacls utility for Windows lists certbot's executable as residing at `C:\Program Files\certbot.exe` instead of `"C:\Program Files\certbot.exe"`, then an attacker could drop a malicious executable in `C:\Program EvilDirectory\cerbot.exe`. If run with the same permissions as the legit cerbot exeuctable - as would probably be the case if certificate revocation and renewal is done via an automated service, then this exeutable will be able to pull certificates from the "secure" storage location and send them to the attacker, hypothetically. 
        * Claim C3 argues against this. If proper installation instructions are followed, then cerbot should be installed in the C:\inetpub\ directory, or a similar directory name without spaces in the name. This would prevent unquoted service path attacks using certbot, even if the path is unquoted. 
         
        EVIDENCE GOES HERE 
         
 
 
----------------------------------------------- 
<br><br> 
## Certbot resists attacks to subvert certificate renewal processes  
-----         
This assurance case does not have an accompanying diagram. This is because this claim is largely similar to the first outlined assurance case concerning certificate revocation. At Cerbot's core, a certificate can be stored, revoked, obtained, and renewed. The latter two functionalities are largely encompassed in the diagrams and claims shown, and as such the corresponding claims and rebuttals can be applied to them as well.  
 
 
 
 
## Github and Kanban 
----- 
https://github.com/KendrickU/swaProject<br> 
https://github.com/KendrickU/swaProject/projects/4<br> 
