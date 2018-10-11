### Group: 6b87bd7cbe948ef26891defd0258c91d1126beb506ce0da4fc5f6dc7467fb26c
# Assurance cases
-----
Below are five top level assurance claims with accompanying diagrams:
<br><br>
* Certbot resists attacks to subvert the certificate aquisition process
* Certbot prevents the unauthorized revocation of certificates
* Certbot storage mechanism prevents unauthorized disclosure
<br>
Here are two additional claims which do not warrant seperate diagrams

* Certbot resists attacks to subvert certificate renewal processes
	* this is similar to the claim for revoking certificates
<br><br>
## Certbot resists attacks to subvert the certificate acquisition process 
-----	
### The context for these assurance cases is that certbot is being used by a bank to manage their TLS certificates
-----
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

## Security Features 
-----
* The private key is generated locally on your system.<br>
* Can get domain-validated (DV) certificates.<br>
* Can revoke certificates.<br>
* Adjustable RSA key bit-length (2048 (default), 4096, ...).<br>
* Can optionally install an http to https redirect, so your site effectively runs https (Apache only).<br>
* Configuration changes are logged and can be reverted.<br>
* Various DNS authentication plugins built into CertBot to provide extra protections.<br>

## Motivation for selecting this project
-----
Our motivation for selecting this project originates from us trying to find an open-source project that is mainly Python based development and focuses greatly on security dealing with web servers. We wanted to target a large scale open-source project in need of some improvement in security. After reviewing the project, Certbot, on Github, we realized CodeTriage lists almost two thousand issues.This is largely due to its status as open source software under tight scrutiny, due to being a piece of widely used security software. We are very excited to help our community fix or improve these issues. <br><br>
In order to offer a more secure internet environment the Electronic Frontier Foundation (EFF) has partnered with Let’s Encrypt.  Let’s Encrypt allows for the generation of a free certificate for validated domains and when combined with Certbot, EFF helps offer an automated way to generate a valid certificate for a website such that it can then offer https.  By lowering the barrier of entry when enabling https for smaller organizations the EFF moves to create a safer surfing environment for customers and helps promote a more secure internet.  While the EFF has partnered with Let’s Encrypt with their Certbot they do allow interfacing with all other ACME compliant services such as: Entrust, Buypass, and GlobalSign.<br><br>
Certbot is EFF’s tool to obtain certificates from Let’s Encrypt and (optionally) auto-enable HTTPS on your server (if the application is supported by their automation scripts). It can also act as a client for any other CA that uses the ACME protocol. Gathering and maintaining these digital certificates from trusted third parties called certificate authorities (CA), will allow nginx and other web servers to securely communicate over the web by providing proper authentication.<br><br>

## Statistics about the Certbot Github Page
-----
* Contributors- 331 unique contributors<br>
* Activity- It has 8,736 commits and 59 releases.<br>
* Use- Wide spread use for those that want to automatically generate a certificate for their website.  Since it generates a Let’s Encrypt certificate it is fast and easy to create an https website.<br>
* Popularity- The project is starred 23,135 times which shows that it is widely followed and also has 891 people watching the repository for updates.<br>
* Languages used- Python 85.9%, Shell 10.3%, Batch File 1.7%, Make File 1.6%, Augeas 0.2%, Docker File 0.2%.<br>
* Platform- All major and current releases of Unix and even some non-unix devices.  For web applications the supported platforms are: Apache, Nginx, Haproxy, Plesk and others not listed.  Since this is a certificate generation service it can apply to all web based applications that support https.<br>

## Documentation sources-
Apache License Version 2.0 - https://raw.githubusercontent.com/certbot/certbot/master/LICENSE.txt<br><br>
License -  Subject to the terms and conditions of this License, each Contributor hereby grants to you a perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable copyright license to reproduce, prepare Derivative Works of, publicly display, publicly perform, sublicense, and distribute the work and such Derivative Works in Source or Object form. <br><br>
Procedure for making contributions - https://certbot.eff.org/docs/contributing.html To contribute to Certbot a contributor first must get their code to run in a virtual environment provided by the Certbot creator to make sure it will work. Also, the code must adhere PEP8 Python Style Guide and also Sphinx Documentation Style. Once the the contributor thinks the code is correct the Certbot Developer guide gives several commands that should be ran against the code to make sure it is formatted correctly and documented correctly. Once the code show that it is working in the virtual environment the contributor is to make a Pull Request that will then be reviewed.<br><br>
Contributor Agreements - Anyone who intentionally submit code for inclusion into Certbot shall be put under the License. Meaning all rights are given up for free use of the code.<br><br>


## Summary of security-related history
-----
Certbot currently has 997 issues open issues on its github page. Amongst these, recent security concerns such as a lack of configuration options for nginx servers for enabling TLS 1.3- the current and increasingly widespread version of TLS - could dissuade users from continuing to use/adopt certbot in enterprise settings, as the use of weak or out-of-date cipher suites in TLS-enabled protocols is increasingly considered a vulnerability in enterprise systems. Other security issues include permission errors in directories created by certbot, as well as a lack of OCSP response caching, which would decrease the efficiency of checking certificate validity. These issues highlight areas for improvement from a software engineering perspective. 

## Github and Kanban
-----
https://github.com/KendrickU/swaProject<br>
https://github.com/KendrickU/swaProject/projects/1?add_cards_query=is%3Aopen<br>

