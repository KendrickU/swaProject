### Requirements for Software Security Engineering
# Five data flows:
-----
Use and misuse cases detailed in this document are inspired from a hypothetical scenario in which Certbot is used to secure a bank’s web services, from within an internal network as well as on outwards-facing machines. Five of Certbot’s most pertinent functionalities will be detailed below with respect to this scenario:

---1---

## Installation and creating account:

![Diagram 1](https://www.lucidchart.com/publicSegments/view/70f5c6b6-ec1c-4749-8bbc-5eb3dcb00fa4/image.png)

### Installing certbot:

Certbot can be installed either from Linux repositories (Windows installation requires installing Bash for Ubuntu from the Microsoft store first, but still requires the same repositories), or from a Github source. Both of these avenues may be susceptible to attack from well-funded and/or motivated adversaries. 

### Installing from Linux repositories:

#### Use case:

A user would typically install Certbot by following installation instructions at https://certbot.eff.org/ . After specifying a web server and operating system, they would be presented with instructions to add remote repositories, which allow for the requisite certbot release to be pulled upon the next sudo apt-get update. From here on in, certbot will attempt to modify the web server configuration files and start the certificate generation process, as well as asking the user for email addresses for the purposes of creating an account with Let’s Encrypt. 

#### Misuse case:

One obvious misuse case would be if the user is lead to a rogue website, hosting instructions that include adding a “malicious” repository to their system, which would result in the user installing malware onto their system under the assumption that they are using the legitimate Certbot repository.

### Installing from source

#### Use case:

Alternatively, a user may opt to install Certbot from the official Github source. It is strongly recommended that only developers choose to install certbot from source. To do this, one must first clone the official github repository at “https://github.com/certbot/certbot.git”, before moving on with subsequent instructions.

#### Misuse case:

In the same vein as the previous misuse case, a rushed and/or inexperienced system administrator in our bank may stumble upon Joe Blogg’s Certbot Installation Blog, which instead urges to him to clone “https://github.com/certibot/certbot.git”, which supplies a slightly modified version of Certbot complete with a Python-formatted Meterpreter shell in byte format, called at the top of a Certbot installation class. He has installed malware onto his system and he is none the wiser. 

#### Mitigation:

Suppose that the bank then instantiates a policy that Certbot and similar utilities may only be installed from official sources - a new Standard Operating Procedure is set up that mandates the specific URL to use when installing Certbot from source. This would mitigate the previously described attack.


#### Counter mitigation:

So the attack then begins the process of submitting a code contribution to Certbot’s github source. He craftily manages to include malicious code this way, that goes unnoticed by Certbot’s code-review authority. Now, whomever installs certbot from source will unknowingly install malware. The changes of this happening are slim, and it is almost certain that Certbot’s wide user-base will alert the github maintainers of the issue. As such, Cerbot then mandates more stringent code reviews following this incident. Additionally, all new code submissions are now run through VirusTotal, to screen for known malicious payloads. The attacker now has to devise another method of attack to compromise a user attempting to install Certbot. 

### Registering an account:


#### Use case:

Normally, Certbot will prompt a user for their email address, so as to establish a subscriber agreement with Let’s Encrypt. This can be done with the --email and --agree-tos command line parameters to the certbot-auto executable. As it is common with most enterprise environments, a configuration file is used, containing enterprise email accounts in plaintext, as well as other information such as domain names that the eventual certificate should validate. This is done with the following command:

cat config.cfg | certbot-auto certonly --standalone --email

### Misuse case:

The attacker in this case is a disgruntled former co-worker from the system administration team. Neither party paid much attention to company password policies, and as such the former employee has access to the system administrator’s LAN account. He logs into this account, locates config.cfg, and replaces the email address to that of his own. The next time certbot is installed, this email address is used to create a new agreement with Let’s Encrypt, giving the former employee the ability to request certificates for all domains specified in the file at his discretion. 

#### Mitigation:

Following this, config.cfg is submitted to the bank’s internal version control software. Any changes made to it are immediately apparent. 

### Misuse case:

An attacker has successfully obtained the credentials for Certbot’s github account, through phishing or otherwise . They now have free reign to modify Certbot’s source code to their liking. All subsequent users that install Certbot from source are affected, including our bank. 

#### Mitigation: 

Following this, the bank’s internal email team has launched a company-wide email campaign which alerts all employees of high-profile security breaches. Certbot’s large user-base ensures that any security issues inherent to the platform make the top of the list, and all Certbot-related activities are suspended at the bank until an official statement from Certbot is released in response to the issue, as well as any mitigations against future attacks that have been put in place. 

---2--- 
## Creating the Certificate 
![Diagram 2](https://www.lucidchart.com/publicSegments/view/13ae8a98-2e72-4ca2-997b-19b134863c79/image.png)

#### Use Case:

Administrator will need to create a certificate for the bank to show they are in fact a bank. With the request for the certificate made the bank now is validated.

#### Misuse Case:

An disgruntled user has successfully obtained a certificate from a second hand site that was illegally obtained. With this certificate the disgruntled user can impersonate the 

#### Mitigation:

By shortening the window of certificate revocation we the administrator can reduce the chance of an old ticket being compromised. This helps prevent a old certificate from being used to impersonate the bank.

### Transferring Certificate to Bank Servers

#### Use Case:

Once the certificate is made and put on the banks servers. It will allow for it to be used to validate itself. 

#### Misuse Case:

A man in the middle attack can be done during the certificate creation and the time it is passed to the server. During this opening if the man in the middle attack successed the disgruntled user can obtain the certificate and impersonate the bank.

#### Mitigation:

If the bank uses a trusted network this can prevent unwanted guest from being able to listen in on traffic which can prevent a man in the middle attack.

#### Counter Mitigation:

The disgruntled user man in the middle attack has failed because they did not have access to the trusted network. The user now resorts to credential phishing to obtain access to the trusted network. With access to the network they now have the opportunity again to try a man in the middle attack. 

#### Mitigation for Counter Mitigation:
 
The bank should take the time to inform and teach its employees to be vigilant for phishing attempts to help protect the banks systems and their assets. With vigilant employee the threat can be lessened for a phishing attempt.

### Grabbing Certificate from CA Server:

#### Use Case:
The administrator goes to the CA server to get a signed certificate to prove they are a legitimate bank and not a fraud. During this process they have to do a cryptographic handshake to ensure the certificate is going to the right place.

#### Misuse:

The attacker learns that the bank it talking to CA server. The attacker listen to the connection and manage to intercept the handshake and grab the cryptography secret and becomes the CA server.

#### Mitigation:

Using a new shared secret key every time would prevent the an outside user from making the disgruntled user look like the CA server.

---3--- 
## Checking Expiration on Certificates

![Diagram 3](https://www.lucidchart.com/publicSegments/view/7075d0a6-54c0-4976-a110-d28f5b62c867/image.png)

One of Certbot’s main uses includes automatically or manually checking if a certificate has expired, and the relevant steps necessary . We have identified various potential misuse cases in response to this functionality:


### Checking for certificate expiration:

#### Use case:
As stated, this is done either automatically via the “certbot renew” command. Alternatively, it can be done manually with various command line parameters and custom configuration files. Either way, this involves Certbot contacting the DNS server for the domain which the certificate will validate. 

#### Misuse case:

From the perspective of an attacker, DNS offers a multitude of attack vectors. Specific to checking for certificate expiration, an attacker could impersonate the DNS server for the domain to be validated. This can be done by first exploiting a flaw in the bank’s DNS server(s), causing them to respond to DNS queries from the DNS server pointed to by Certbot with valid names, but substitute the expected IP address with one controlled by an attacker. These domain/IP address translations will then be cached in the server, and subsequent certificate revocation lookups by Certbot will result in the certificate to be checked being considered valid for the domain matched to the IP address controlled by the attacker

#### Mitigation:

Certbot supports the use of DNSSEC for the domains validated by the certificate. This would render the above attack scenario invalid, as even if the attacker compromises a DNS server, would have to present the domain’s public key to  Certbot to successfully identify as the domain in question. 

#### Counter mitigation:

A far-fetched but possible way for an attacker to get around DNSSEC would be to instead attack the certificate authority itself. In doing so, any DNS-level security measures would cease to be effective. This would involve somehow listening to some correspondence between the Bank and the CA, and obtaining a shared cryptographic secret used to establish a TLS-secure session between Cerbot and the CA as part of the certificate checking process . This would involve some level of cryptographic attack, such as Heartbleed, and a similarly vulnerable SSL implementation to be in use by either the CA or the Bank. This would need to happen every time the attacker wishes to temporarily impersonate the CA from the perspective of the bank.

#### Counter-Counter mitigation:

One way to prevent this scenario from unfolding successfully would be to use OCSP stapling when checking for expired certificates, using the OCSP protocol. The OCSP protocol is specifically designed for use when conversing with CA’s for the purposes of checking certificate statuses. OCSP stapling was designed in part to specifically reduce the effectiveness of SSL vulnerabilities by providing an additional layer of verification during TLS sessions. It amounts to ensuring that a CA-signed timestamp is included during TLS handshakes. This timestamp is obtained prior to the handshake. This prevents the need for the client to contact the CA directly to validate a certificate for revocation. Therefore, if the CA is taken over, a valid OCSP response from the real CA would only suffice for revoking certificates (assuming a new timestamp is not obtained whilst the CA is compromised).

## Certificate revocation

#### Misuse case:

Due to lazy programming, a wildcard is used in a script that uploads files from a directory from a web development project run by the bank. These files are committed to a publicly viewable github account. Due to the wildcard, all files in this directory are committed, including the certificate used by the project. A hacker then stumbles upon this repository, and takes this opportunity to read the domain name supported by the certificate and spin up his own phishing website under the guise of the project. 

#### Mitigation/Use case:
This could be mitigated by manually revoking this certificate through Certbot. It can be done simply with the command “certbot revoke <path-to-certificate>”.

Alternatively, a new certificate can be generated using similar command line parameters. This would similarly mitigate the above misuse case

---4--- 
## Writing Certificates to the Filesystem & storing them
![Diagram 4](https://www.lucidchart.com/publicSegments/view/87f282c3-9d43-44b4-bf0f-97a3d79bacbf/image.png)

When a certificate is created it is usually stored on a web server’s file system.  This web server then uses this certificate in future communications via web traffic.  This local file needs to be protected as any person with this certificate can host web instances under the guise of the certificate’s domain.  There are several different avenues of attack for exfiltrating this file.

#### Use Case

Certbot can automatically create file structures that can be used in conjunction with common web servers for the purpose of providing HTTPS connections with a secure certificate.  There may be attack vectors that can be exploited for the purposes of obtaining the certificate, if Certbot’s recommended security practices are not adhered to. These include storing certificates in protected directories, away from those hosting web services.

#### Misuse Case

As best practices are not always implemented in corporate or professional settings due to time and/or budget constraints, a system administrator at the Bank has left a web service’s certificate in a non-protected service account folder - the same folder hosting the web service’s resources. An attacker manages to abuse a vulnerability within the web application, and gains access to this service account, and as such the attacker has no problem compromising the web service and obtaining the certificate

#### Mitigation

Using a sufficiently strong access control/directory permissions to house the certificate within a protected directory, away from the hosting web directory, would prevent a low-privilege service account from easily obtaining the certificate.

#### Counter mitigation:

However, the attacker is quick to utilize weak Active Directory/Samba permissions to elevate his privileges to that of a local administrator via token impersonation or some other suitable method, ultimately gaining access to the Administrator NTLM hash and thus all directories within the web server. 

#### Mitigation:

Following this, one-time passwords/multifactor authentication is implemented to prevent an attacker from using obtained account hashes to authenticate as privileged users.

## Ensuring certificate integrity (checking certificates):

#### Misuse case:

Another attacker manages to gain access to a similar web server on the outwards facing network via web exploitation, but instead chooses to replace the current certificate residing on this server with his own - generated with his own Let’s Encrypt account. In doing this, he is able to view, modify, or delete any HTTP data originating from or sent to the web service on this server.


#### Use case:

Following these breaches, a new standard operating procedure was devised within the Bank. It dictates that certificates need to be hashed on a periodic basis, and checked against previous certificate hashes to ensure that no changes have been made.

  


---5--- 
## Revoking Certificates
![Diagram 5](https://www.lucidchart.com/publicSegments/view/06f5f175-5fe1-4d1a-b615-86de60fa0998/image.png)

#### Use Case:

CertBot can check for valid certificates using choice of plugins that CertBot supports. Depending on which plugin is chosen, the automation of checking certificates for validity will be different. In a hypothetical situation, we use Apache Server on CentOs and LetsEncrypt  built into CertBot in a bank setting to check if a certificate has expired and needs to be revoked. This can be done with the following command (Link: https://letsencrypt.org/docs/revoking/): 

certbot revoke --cert-path /etc/letsencrypt/archive/${YOUR_DOMAIN}/cert1.pem

This command is used if you originally issued the certificate and you have control of the account that issued it. 

If you have access to the private key then you can use this command: 

certbot revoke --cert-path /PATH/TO/cert.pem --key-path /PATH/TO/key.pem

If you need to do it from a different authorized account, do this command:

certbot certonly --manual --preferred-challenges=dns -d ${YOUR_DOMAIN} -d nonexistent.${YOUR_DOMAIN}

Then follow the prompted instructions and download the certificate from crt.sh then issue the final command:

certbot revoke --cert-path /PATH/TO/downloaded-cert.pem

Further steps can be used to automate the checking validity and revoking of certificates.

### Misuse Case:

Our attacker is insider threat in IT named Bob who works for the bank. Bob can threaten to  falsify DNS records in order to make revoking certificates not work by falsifying the DNS records and pointing the DNS to the incorrect domain. 

#### Mitigation:

A mitigation technique can be used to stop Bob from falsifying DNS records. The technique that can be used is every 30 days or so a system administrator that works for the bank could go through and make sure that the DNS server points to the correct domaining which makes sure that everything all the DNS records are correct and the DNS server itself is setup properly. 

#### Misuse Case:

If falsifying DNS records doesn’t work for Bob then abusing firewall configurations and leaving ports open intentionally so Bob can get in there and change the configuration to stop revoking certificate is the next plan of action. This is usually done by messing with the IP tables that are setup. 

#### Mitigation:

Bob’s plan of attack can be stopped by manually checking the firewall configurations every 30 days or so which will mitigate the abuse of open ports to allow Bob to stop revocation of certificates. 

#### Misuse Case:

Bob can ARP poison a machine that requests a certificate to check for revocation in which it will make the requesting machine believe that the malicious machine is someone else by inserting the malicious machine into the pipeline making the requesting machine that is in communication think the malicious machine is a good guy and here he can manually revoke the certificate if he is in communication with the requesting machine.

#### Mitigation:

Bob can be stopped by configuring static ARP entries in which these entries will properly map MAC addresses and ip addresses to confirm identities.

#### Misuse Case:

It is hard to do but if he has trouble making the requesting machine believe that the malicious machine is good, then Bob can take the approach of spoofing the MAC address by using a MAC address changer via command line tools. He can then continue on with the ARP poisoning to make it seem like the malicious machine communicating with the requesting machine is good and send back data saying that the certificate should either be revoked or good.

#### Mitigation:

Believe it or not but using IPv6 makes it harder for people to know the IP address because it is hashed which then makes it even harder to spoof the MAC address. 


## OSS Review

Certbot has a configuration file that allows for some security features such as “rsa-key-size = 4096”.  It also includes some configurations that relate to which email account to use for the registration for automated setup and what type of authenticator to use for this account.  The full range of security related configuration commands are:
* Rsa-key-size
  * Size of the RSA key.
* Must-staple
  * Adds the OCSP Must Staple extension to the certificate.
* Redirect
  * Redirect HTTP traffic to HTTPS
* No-redirect
  * Do not redirect HTTP traffic to HTTPS.
* Hsts
  * Force SSL for the domain
* Uir
  * Upgrade insecure requests
* Staple-ocsp
  * Enables OCSP Stapling. A valid OCSP response is stapled to the certificate that the server offers during TLS.
* Strict-permissions
  * Require that all configuration files are owned by the current user


https://certbot.eff.org/docs/using.html#configuration-file

