#Certbot
-----
2-3 page markdown document that describes the following:
What are the security needs of users from this software in its intended threat environment (e.g., home, office, enterprise, bank, government, etc.)? If there are none or very few, then re-evaluate your selection.
Many users may use this system to benefit their software. These users can be anyone from within the government all the way to users at a commercial level. Even users can use CertBot at a personal level too. 
The users of certbot would expect to be able to reliably utilize Certbot to obtain dependable SSL certificates from LetsEncrypt servers in an automated fashion. 

Examples: 
Banks are among the users and  it is imperative that they use Certbot to manage their certificates in order to ensure that no personal information is compromised among their customers while bank transactions occur. 
Commercial companies can use CertBot to securely communicate among their web servers in order to ad. 
Develop a list of security features in the software. Again, if there are none or very few, then re-evaluate your choice.
## What Certbot Does 
The private key is generated locally on your system.
Can get domain-validated (DV) certificates.
Can revoke certificates.
Adjustable RSA key bit-length (2048 (default), 4096, ...).
Can optionally install an http to https redirect, so your site effectively runs https (Apache         only)
Configuration changes are logged and can be reverted.
Various DNS authentication plugins built into CertBot to provide extra protections

## Motivation for selecting this project
Our motivation for selecting this project originates from us trying to find an open-source project that is mainly Python based development and focuses greatly on security dealing with web servers. We wanted to target a large scale open-source project in need of some improvement in security. After reviewing the project, Certbot, on Github, we realized CodeTriage lists almost two thousand issues.This is largely due to its status as open source software under tight scrutiny, due to being a piece of widely used security software. We are very excited to help our community fix or improve these issues. 
Open source project description (What is it?, Contributors, Activity, Use, Popularity, Languages used, platform, documentation sources, etc.)
In order to offer a more secure internet environment the Electronic Frontier Foundation (EFF) has partnered with Let’s Encrypt.  Let’s Encrypt allows for the generation of a free certificate for validated domains and when combined with Certbot, EFF helps offer an automated way to generate a valid certificate for a website such that it can then offer https.  By lowering the barrier of entry when enabling https for smaller organizations the EFF moves to create a safer surfing environment for customers and helps promote a more secure internet.  While the EFF has partnered with Let’s Encrypt with their Certbot they do allow interfacing with all other ACME compliant services such as: Entrust, Buypass, and GlobalSign.
Certbot is EFF’s tool to obtain certificates from Let’s Encrypt and (optionally) auto-enable HTTPS on your server (if the application is supported by their automation scripts). It can also act as a client for any other CA that uses the ACME protocol. Gathering and maintaining these digital certificates from trusted third parties called certificate authorities (CA), will allow nginx and other web servers to securely communicate over the web by providing proper authentication.

## Statistics about the Certbot Github Page:
Contributors- 331 unique contributors
Activity- It has 8,736 commits and 59 releases.
Use- Wide spread use for those that want to automatically generate a certificate for their website.  Since it generates a Let’s Encrypt certificate it is fast and easy to create an https website.
Popularity- The project is starred 23,135 times which shows that it is widely followed and also has 891 people watching the repository for updates.
Languages used- Python 85.9%, Shell 10.3%, Batch File 1.7%, Make File 1.6%, Augeas 0.2%, Docker File 0.2%.
Platform- All major and current releases of Unix and even some non-unix devices.  For web applications the supported platforms are: Apache, Nginx, Haproxy, Plesk and others not listed.  Since this is a certificate generation service it can apply to all web based applications that support https.

## Documentation sources-

Discuss License, procedures for making contributions, and contributor agreements (Links to an external site.)Links to an external site.
https://certbot.eff.org/docs/contributing.html - Certbot contribution guidelines
https://raw.githubusercontent.com/certbot/certbot/master/LICENSE.txt - Apache License Version 2.0
License -  Subject to the terms and conditions of this License, each Contributor hereby grants to you a perpetual, worldwide, non-exclusive, no-charge, royalty-free, irrevocable copyright license to reproduce, prepare Derivative Works of, publicly display, publicly perform, sublicense, and distribute the work and such Derivative Works in Source or Object form.
Procedure for making contributions - https://certbot.eff.org/docs/contributing.html To contribute to Certbot a contributor first must get their code to run in a virtual environment provided by the Certbot creator to make sure it will work. Also, the code must adhere 





Summary of security-related history (E.g., known vulnerabilities, security-related engineering decisions, security feature additions/removal, etc. )
Link to your team GitHub repository that shows your internal project task assignments and collaborations to finish this task.
Github Project Boards,  (Links to an external site.)Links to an external site.
Links to an external site.Kanban boards (Links to an external site.)Links to an external site


.
