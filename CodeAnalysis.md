### Group: 6b87bd7cbe948ef26891defd0258c91d1126beb506ce0da4fc5f6dc7467fb26c

# <b>Automated Testing Analysis Tools</b>

We used <b>Pylint</b>, and <b>DHS Swamp</b> as our automated testing static analysis tools. We found that their uses were to remove unnecessary spacing and empty line removals. DHS Swamp was able to tell us that import statements were unused which we then went through each one and confirmed that they are indeed unused.

## Pylint
Automated tool but went through every individual file then documented:
This has no link to any CWEs<br><br>
In tests/modification-check.py:<br>
* Line 18: It found it has a space at the end of the line<br>
* Line 52: It found it has a space at the end of the line<br>
* Line 55: It found it has a space at the end of the line<br>
* Line 74: It found it has a space at the end of the line<br>

certbot/ocsp.py<br>
* Line 120: Empty line at end of file removed<br>

certbot/error_handler.py<br>
* Line 157: Empty line at end of file removed<br>

certbot-postfix/certbot_postfix/util.py<br>
*	Line 292: Empty line at end of file removed<br>

certbot-postfix/certbot_postfix/postconf.py<br>
*	Line 151-152: Empty line at end of file removed<br>

certbot-postfix/certbot_postfix/installer.py<br>
*  Line 288: Empty line at end of file removed<br>

certbot-dns-rfc2136/certbot_dns_rfc2136/dns_rfc2136.py<br>
*  Line 233: Empty line at end of file removed<br>

certbot-dns-linode/certbot_dns_linode/dns_lonide.py<br>
*  Line 73: Empty line at end of file removed

## DHS SWAMP:<br>

In [flake8certbot.xml](https://github.com/KendrickU/swaProject/blob/master/xmlFiles/Flake8CertBot.xml)<br><br>
These important statements are dead code. CWE-561.

Link to pull requests:

* Bug Instance ID 271: import ‘sys’ but not used
* Bug Instance ID 3142: import ‘sys’ but not used
*	Bug Instance ID 3300: import ‘sys’ but not used
*	Bug Instance ID 3406: import ‘sys’ but not used
*	Bug Instance ID 1049: import ‘shlex’ but not used (Not included in pull request)

<b> https://github.com/certbot/certbot/pull/6546 </b>

In [Banditcertbot.xml](https://github.com/KendrickU/swaProject/blob/master/xmlFiles/BanditCertBot.xml)<br><br>
Can’t figure out what CWE shell=True would link to. It should usually be shell=False<br>
* Bug Instance ID 19: Use of Shell=True in process open call<br>
  * An attempt was made to leverage the instances of the “shell=True” parameters assigned to the Popen() call on line 243 in certbot/hooks.py for command execution. Assigning shell=True for subprocess, Popen, etc in Python is usually avoided due to the risk of allowing arbitrary code execution ( see https://stackoverflow.com/questions/3172470/actual-meaning-of-shell-true-in-subprocess). Manual testing of certbot with the “--manual” flag (for an interactive shell) was conducted in an attempt to leverage semicolons, carriage returns, and similar delimiters to invoke arbitrary command execution, but this could not be achieved. This parameter was likely put in place to allow for various command line functionalities like piping to files, etc to be possible, and thus these instances of “shell=True” may be warranted.
* Bug instance ID 12 : Use of “verify=False” when using Requests:
  * This method call is found in the function “request_with_no_cert_validation”, indicating that this is an intended option.
* Bug Instance ID 70: Insecure function
  * The code structure shows that a safety check is done to make sure the config_value is only 1 of 3 possible values.  This is then evaluated in a non-safe way but because of this check the non-safe way becomes safe and does not need to use ast.literal_eval().
  * https://github.com/certbot/certbot/blob/master/certbot/renewal.py#L157
  * https://docs.python.org/2/library/ast.html#ast.literal_eval
* Bug Instance ID 33: Use of MD5 (can be linked to: CWE-916)
  * https://github.com/certbot/certbot/blob/master/certbot/account.py
  * Doing code analysis on this section reveals that the MD5 function is used for hashing the user’s public key which could lead to collisions but is highly unlikely to matter since the information is the users Public key
  * Maybe use stronger algorithm

All other findings reported by Bandit did not warrant an investigation since they were mostly minor stylistic choices or Try/Assert statements found in the testing modules for CertBot.

# Code Review Strategies:

Type Checking: Went through the entire code base. Type checking meets PEP (484) standards. Static analysis type checking was done by PyCharm.

Ad-hoc: Not appropriate here
Scenario-based: Not doing this

We compiled a check-list ourselves that we thought would be good for CertBot<br>
## Compliance Checklist: (Use checklist to view certain files that are high value)<br>

CWE-20: Improper Input Validation<br>
CWE-35: Path Traversal<br>
CWE-88: Argument Injection or Modification<br>
CWE-113: Improper Neutralization of CRLF Sequences in HTTP Headers (‘HTTP Response Splitting’)<br>
CWE-134: Use of Externally-Controlled Format String<br>
CWE-201: Information Exposure Through Sent Data<br>
CWE-269: Improper Privilege Management<br>
CWE-285: Improper Authorization<br>
CWE-288: Authentication Bypass Using an Alternative Path or Channel<br>
CWE-311: Missing Encryption of Sensitive Data<br>
CWE-326: Inadequate Encryption Strength<br>
CWE-327: Use of a Broken or Risky Cryptographic Algorithm<br>
CWE-363: Race Condition Enabling Link Following<br>
CWE-367: Time-of-Check Time-Of-Use (TOCTOU) Race Condition<br>
CWE-385: Covert Timing Channel<br>
CWE-398 Code quality<br>
CWE-441: Unintended Proxy or Intermediary (‘Confused Deputy’)<br>
CWE-514: Covert Channel<br>
CWE-521: Weak Password Requirements<br>
CWE-561: Dead Code<br>
CWE-601: URL Redirection to Untrusted Site (‘Open Redirect’)<br>
CWE-644: Improper Neutralization of HTTP Headers for Scripting Syntax<br>
CWE-780: Use of RSA Algorithm without OAEP (3.1)<br>
CWE-863: Incorrect Authorization<br>
CWE-916: Use of Password Hash with Insufficient Computational Effort<br>

--------------------------------------------------------------------------------------------------------------------------------

We picked 4 out of a list of 25 CWEs we created that pertain to Certbot and checked to see if these CWEs

The file picked is certbot/certbot/util.py because it is the utility file for all of Certbot.

#### CWE-311: Missing Encryption of Sensitive Data

Some of the comments in this file mentions encryption of sensitive data but there is no actual encrypting of data in here.

#### CWE-398 Code quality

Code quality is very subjective because it depends who looks at it. This code follows many standards such as PEP 484 and PEP8 standards. It has been through the linting process and even has a linting module in it that is being used.

#### CWE-561: Dead Code

This CWE was tested against and put this CWE in here in the rare off chance that I would see it in there. All code works as intended and has a purpose. No code is presents that doesn’t have a purpose.

#### CWE-916: Use of Password Hash with Insufficient Computational Effort

I thought that some of these files would have some password hashing in them but they don’t. It is probably the account.py or some other authorization management python file. This has been checked over and is non-applicable to this file.

--------------------------------------------------------------------------------------------------------------------------------------------

The file picked is certbot/certbot/crypto_util.py because it is the utility file for all of Certbot.

#### CWE-311: Missing Encryption of Sensitive Data

Line 106 - 240
``` python
# WARNING: the csr and private key file are possible attack vectors for TOCTOU
# We should either...
# A. Do more checks to verify that the CSR is trusted/valid
# B. Audit the parsing code for vulnerabilities
```

The above are comments found in this file. As stated these csr and private key files are possible attack vectors for TOCTOU which leaves the system very vulnerable. Not all sensitive data is being encrypted properly. We should investigate this more and see what vulnerabilities we can come up with.

#### CWE-367: Time-of-Check Time-Of-Use (TOCTOU) Race Condition

Line 106 - 240
``` python
# WARNING: the csr and private key file are possible attack vectors for TOCTOU
# We should either...
# A. Do more checks to verify that the CSR is trusted/valid
# B. Audit the parsing code for vulnerabilities
```

Certbot is actually vulnerable to TOCTOU attacks as the comments hint at above. This CWE actually applies to this method of checking the csr and private key file. As of right now, Certbot is working on a fix themselves. Very interesting find. Definitely looking at changing the parsing code could fix this weakness.

#### CWE-398 Code quality

As stated before with another file, this code quality is very subjective because it depends who looks at it. This code follows many standards such as PEP 484 and PEP8 standards. It has been through the linting process and even has a linting module in it that is being used. This code is as efficient as it can be using functions such as os path changes that are safer than ones out there which is good. Code quality also depends on the client’s requirements as well.

#### CWE-561: Dead Code

This CWE was tested against and put this CWE in here in the rare off chance that I would see it in there. All code works as intended and has a purpose. No code is presents that doesn’t have a purpose.


#### CWE-916: Use of Password Hash with Insufficient Computational Effort

Certbot at the very least uses sha256sum for computing a hash of a file and leaves password cracking very expensive which is a nice feature to have.

--------------------------------------------------------------------------------------------------------------------------------

The file picked is certbot/certbot/auth_handler.py because it is the utility file for all of Certbot.

#### CWE-311: Missing Encryption of Sensitive Data

There are several calls to encrypt.util one such call is in line 80. To make sure the certificate is renewable cert. At a manual check perspective, this code encrypts almost all sensitive data that it needs to. There is mention of the encryption process in the comments.

#### CWE-398 Code quality

The code follows standards such as PEP 8 and PEP (484 - Type Checking Compliant). This code also properly follows standards for docstrings and comments are helpful too. There is not much wasted aera. All code is doing something and it doesn’t seem to be meaningless. All the code has passed the pylint and it follows it.

#### CWE-561: Dead Code

This CWE was chosen in the off chance they forgot code or left old code in the code base. However, in this file all code was used and was checked against pylint. This being said I doubt we would find any code that was not being called or being used.

#### CWE-916: Use of Password Hash with Insufficient Computational Effort

This file didn’t use much in the sense of password hash because it really does not deal with passwords. It makes a call to see if the profile is verified that is it. This file doesn’t not handle the passwords.

--------------------------------------------------------------------------------------------------------------------------------

The file picked is certbot/certbot/cert_manager.py because it is the utility file for all of Certbot.

#### CWE-311: Missing Encryption of Sensitive Data

There are calls to encrypt.util one such call is in line 80. To make sure the certificate is renewable cert. There are also many time where it verifies the cert. But only one time when it encrypts the cert. This being said I think the code does handle sensitive data with care and it does not leave it out in the open.

#### CWE-398 Code quality

Code quality is very subjective because it depends who looks at it. This code follows many standards such as PEP 484 and PEP8 standards. It has followed the guidelines and passes the pylint that is required to submit to there repo.

#### CWE-561: Dead Code

This CWE was chosen in the off chance they forgot code or left old code in the code base. However, in this file all code was used and was checked against pylint. This being said I doubt we would find any code that was not being called or being used.

#### CWE-916: Use of Password Hash with Insufficient Computational Effort

There were no passwords or mention of passwords and its hashing in this file. The manager deals with handling the certificates and the authentication process. It does not do any sort of hashing in this file for passwords or certs.

Findings from manual code review of critical security functions identified in misuse cases, assurance cases and threat models

As said earlier, MD5 hashing function is used on the public key. The attacker can get a hold of the public key as the MD5 hashing algorithm is fairly weak. This is a weakness linked to CWE-916. This can be an attack vector in our renewal misuse case diagram.
“Note that options provided to Certbot renew will apply to every certificate for which renewal is attempted; for example, certbot renew --rsa-key-size 4096 would try to replace every near-expiry certificate with an equivalent certificate using a 4096-bit RSA public key. If a certificate is successfully renewed using specified options, those options will be saved and used for future renewals of that certificate.” - Official Certbot Documentation
With this public key information exposed, an attacker could possibly falsify the renewal of certificates.

-------------------------------------------------------------------------------------------------------------------------------

A potential time-to-check-to-time-of-use (TOCTOU) bug has been identified by the developers of the certbot/init_save_csr.py class. On line 95, the method init_save_csr() attempts to generate a certificate signing request, followed by a line to generate and verify the permissions of a directory needed to store the resultant .pem file.. A comment regarding this method suggests the possibility of these steps resulting in a TOCTOU situation. We believe this is because the CSR request could stall/is asynchronous in nature, and thus the subsequent code to add the certificate to a directory path could fail.
This seems to be somewhat remediated in the subsequent function, valid_csr(). In this method, a try/except block uses the crypto.load_certificate_request() call to either return None if crypto.verify() passes, or raises an exception and returns False if the CSR fails to be loaded. This seems to handle any discrepancies with the validity of the obtained CSR.

Swamp’s flake8 (a more stringent version of PEP8) picked up an instance of excessive file permissions (755) for the directory that houses the CSR. This seems slightly excessive given the importance of the files the directory contains. However we could not accurately ascertain whether these permissions could result in a compromise. It stands to reason that they would not, as CSRs are temporary, and the ability to read a CSR would not offer an attack much after the CSR has been used.

-------------------------------------------------------------------------------------------------------------------------------

If we look at our misuse case for installation/creating an account, we mention file permission weaknesses. Certbot has a method to verify file permissions are 755 on a directory. Giving read access to everyone could be a problem if a file in that directory should not be known. CWE-275: Permission Issues (3.1) or CWE-732: Incorrect Permission Assignment for Critical Resource are two CWEs we can assign to Certbot. Maybe verifying that each directory is 751 is a better option.

-------------------------------------------------------------------------------------------------------------------------------

On our misuse case for checking expiration, it shows falsifying DNS records. These CWEs could tie into CWE-941: Incorrectly Specified Destination in a Communication Channel or CWE-350: Reliance on Reverse DNS Resolution for a Security-Critical Action. This is if a disgruntled admin decided to point this at a false DNS server where DNS records are held.

-------------------------------------------------------------------------------------------------------------------------------


Our assurance cases and misuse cases don’t pertain to as many attack vectors as we originally thought were possible.


---------------------------------------------------------------------------------------------------------------------------

Link to our group kanban: https://github.com/KendrickU/swaProject/projects/6
