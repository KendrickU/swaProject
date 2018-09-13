### Group: 6b87bd7cbe948ef26891defd0258c91d1126beb506ce0da4fc5f6dc7467fb26c
# Certbot
-----
Many users may use this system to benefit their software. These users can be anyone from within the government all the way to users at a commercial level. Even users can use CertBot at a personal level too. <br><br>
The users of certbot would expect to be able to reliably utilize Certbot to obtain dependable SSL certificates from LetsEncrypt servers in an automated fashion. 

## Examples 
-----	
* Governments may use Certbot to manage hundreds if not thousands of certificates to make sure they are up to date and secure. Governments rely on up to date certificates otherwise the systems being used may reject their users and it may prevent their users from completing mission critical tasks. 
* Banks are among the users and  it is imperative that they use Certbot to manage their certificates in order to ensure that no personal information is compromised among their customers while bank transactions occur. 
* Companies can use CertBot to securely communicate among their web servers in order to advertise their services on a website available to their customer.

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
