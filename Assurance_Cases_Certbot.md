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
	* C3 repudiates R3 in that certbot communicates with Let's Encrypt over TLS, greatly increases the complexity of attacks against certbot's ability to obtain certificates fro Let's Encrypt
	* This would be evident from TLS-secured packet capture on the machine hosting certbot, sending requests to  Let's Encrypt.
	* however, one undermining point against this evidence would involve somehow bruteforcing or otherwise obtaining cryptographic secrets used during the handshake phase of TLS traffic between cerbot and Let's Encrypt. This could result in a compromised channel of communication, and thus the malicious tampering of certificates from Let's Encrypt
	* C5 refutes this, as a new shared secret upon every handshake, as defined in RFC 8446 (TLS 1.3, found at https://tools.ietf.org/html/rfc8446), outlines that a new secret would be generated upon every TLS request, thus making the above attack infeasible for any meaningful purpose. 

	* the Inference Rule derived from claim C1 states that if the previously described rebuttals are mitigated, as described and exemplified with relevant evidence, then certbot will reliably be able to obtain certificates.
	* Unless a 0-day that allows for certbot's correspondence with Let's Encrypt becomes widely known.
	* Nevertheless, Certbot actively remediates issues as shown on the official github repositories, thus reducing the effectiveness of a powerful 0-day


-----------------------------------------------
<br><br>
## Certbot prevents the unauthorized revocation of certificates 
-----	

* Top Claim C1 postulates that Certbot's certificate revocation features prevent the unauthorized revocation of certificates

* This can be rebutted by R1, which asserts that compromised DNS servers could result in Certbot sending revocation requests to a CA which
impersonates Let's Encrypt, which would in turn disallow the revocation of certificates
* However, C1 mentions the OCSP protocol used to revoke certificates. The attacker would need to acocunt for altered OCSP responses being accepted by certbot, along with DNS records
* R2 states that the OCSP protocol is still vulnerable to replay attacks. This is especially possible with DNS-based attacks, as previously accepted OCSP responses from
Let's Encrypt could be leveraged by an attacker to deny or allow certificate revocation at their command.

	* C2 refutes R2, as OCSP can be extended with OCSP stapling (https://tools.ietf.org/html/rfc6960, page 18-19). This is a security extension to the OCSP protocol that
	sends CA-signed timestamps with OCSP responses, which are then validated by a third-party OCSP server. This is similar in principal to the Kerberos authentication 		mechanism in that the CA is not contacted directly upon verification of the timestamps, thus mitigating risks associated with a compromised CA for certificate revocation

	* C3 repudiates R2 in a different manner. Instead of a signed timestamp being verified by a third party, a cryptographic nonce is sent
 
	* This would be evident from TLS-secured packet capture on the machine hosting certbot, sending requests to  Let's Encrypt.
	* however, one undermining point against this evidence would involve somehow bruteforcing or otherwise obtaining cryptographic secrets used during the handshake phase of TLS traffic between cerbot and Let's Encrypt. This could result in a compromised channel of communication, and thus the malicious tampering of certificates from Let's Encrypt
	* C5 refutes this, as a new shared secret upon every handshake, as defined in RFC 8446 (TLS 1.3, found at https://tools.ietf.org/html/rfc8446), outlines that a new secret would be generated upon every TLS request, thus making the above attack infeasible for any meaningful purpose. 

	* the Inference Rule derived from claim C1 states that if the previously described rebuttals are mitigated, as described and exemplified with relevant evidence, then certbot will reliably be able to obtain certificates.
	* Unless a 0-day that allows for certbot's correspondence with Let's Encrypt becomes widely known.
	* Nevertheless, Certbot actively remediates issues as shown on the official github repositories, thus reducing the effectiveness of a powerful 0-day

## Security Features 
-----
* The private key is generated locally on your system.<br>
* Can get domain-validated (DV) certificates.<br>
* Can revoke certificates.<br>
* Adjustable RSA key bit-length (2048 (default), 4096, ...).<br>
* Can optionally install an http to https redirect, so your site effectively runs https (Apache only).<br>
* Configuration changes are logged and can be reverted.<br>
* Various DNS authentication plugins built into CertBot to provide extra protections.<br>

## Github and Kanban
-----
https://github.com/KendrickU/swaProject<br>
https://github.com/KendrickU/swaProject/projects/4<br>

