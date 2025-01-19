
# AWS ACM Hands-On Troubleshooting Workshop

This AWS Labs is about hands on techniques to troubleshoot scenarios for their Amazon ACM environment.

### Prerequisite for this lab
1. **AWS Account :** Please create AWS free account 
2. **IAM user:** Please assign administrator role or access
3. **Domain Name:** Please purchase a new domain

### Services
1. **IAM:** 
2. **Route53:** 
3. **ACM - AWS Certificate Manager:** 

if you've already signed up for Amazon Web Services (AWS) and already have an IAM user or role with administrative permissions, you can jump next section.

### DNS Knowledge
It is necessary you shloud have some DNS knowledge and DNS Terms.
To perform this lab a registered domain name (preferably in Route53, but a 3rd party DNS provider should still work).
This troubleshooting lab will be using shaikhwaseem.com as an example. You will not be able to use this domain. You must have your own registered Domain name.
To register your own domain name with Route53, please see Registering a new domain .

ACM DNS validation (and Email validation) requires a Registered Domain. 

A Public hosted zone/DNS zone.
Route53 supports Public Hosted zones . Please also be aware of the pricing for Route53 
3rd party DNS providers can be used as long as they meet the requirements defined here 
Private DNS zones/hosted zones cannot be used for ACM domain validation.

## Troubleshooting DNS validation for ACM public certificates

This troubleshooting scenario will go over common issues with DNS validation for the AWS Certificate Manager service.

Background
AWS Certificate Manager, also known as ACM , is an AWS service that allows you to create, store and renew public and private X.509 certificates. When requesting public certificates, ACM recommends you use DNS validation . With DNS validation, ACM provides you with a CNAME record pair for each domain in the certificate. Each CNAME record pair must be placed in the Public DNS zone/hosted zone. ACM will check for these CNAME records and validate that they are present. Once all domains are validated, ACM will issue the certificate. You can use the ACM certificate with ACM Integrated Services .

Terms to know
ACM Public Certificate: DNS or Email validated certificates that you request through ACM are obtained from Amazon Trust Services , an Amazon managed public certificate authority (CA) .

DNS (Domain Name System): translates human readable domain names (e.g example.com) to machine readable IP addresses (e.g 192.168.1.100).

Public DNS zone/hosted zone: container for records such as A, CNAME, NS, and CAA. These records contain information about how you want to route traffic on the internet for a specific domain, such as example.com, and its subdomains.

Registered Domain: a domain name that has been registered with a domain name registrar. A registrar can be Route53 Domains or another organization that manages domain names. When domain registration is complete, depending on the registrar, you can begin to edit records in its DNS zone/hosted zone.

Fully Qualified Domain Name(FQDN): The complete address of a host or computer. (e.g dev.example.com).

CNAME record: a type of DNS record that maps one domain name to another. In ACM DNS validation, a CNAME record is used for domain validation.

NS record: a type of DNS record that identifies the authoritative nameservers of a given domain or subdomain. Authoritative nameservers have the definitive information about which Nameservers contain the DNS records for a given domain or subdomain.

ACM Integrated Services: Certain AWS services use ACM certificates. These include Elastic Load Balancing, Amazon CloudFront and Amazon API Gateway. The entire list is shown here .

## Requesting a Public Certificate
This section shows how you can request a public ACM certificate. In the scenario, you will be asked to request a public ACM Certificate. Since DNS validation is the recommended validation method, we will be using DNS validation. This workshop shows how to request an ACM public certificate in the AWS console or the AWS CLI. Please select the tab for instructions.

1. Navigate to the Certificate Manager (ACM) console  in us-east-1. We will be using the us-east-1 region (N.Virginia) for this scenario.

https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/Troubleshooting-DNS-Validation/RequestPublicCertificate/ACM-Console-1.png

2. Select “Request” in the top right corner.

https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/Troubleshooting-DNS-Validation/RequestPublicCertificate/ACM-Console-2.png

3. Choose “Request a public certificate”.

https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/Troubleshooting-DNS-Validation/RequestPublicCertificate/ACM-Console-3.png

4. Next, choose the fully qualified domain name(s) that you would like to specify in the certificate. This workshop uses the example.com domains. Replace them with your domain(s).

example.com
dev.example.com
build.example.com

https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/Troubleshooting-DNS-Validation/RequestPublicCertificate/ACM-Console-4.png


4. For "Validation method", choose “DNS validation - recommended”. Then, select the Key Algorithm. This workshop will use RSA 2048. The Key algorithm is the algorithm of the public and private keypair that your certificate will use. In addition to RSA, you can also use Elliptic Curve (ECDSA). It’s important to point out that some AWS services may require only RSA-type keypairs or ECDSA-type keypairs of a particular size. An example is Network Load Balancers . They cannot support RSA keys larger than 2048-bit or ECDSA. Next, you can optionally add any tags. To finish the request, select “Request”.
https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/Troubleshooting-DNS-Validation/RequestPublicCertificate/ACM-Console-5.png

6.After requesting, navigate to the certificate. You may need to refresh the screen.
After selecting the certificate, you will see the fully qualified domain name(s) you specified. There will be a CNAME record pair for each fully qualified domain name. Since three fully qualified domain names were used in this workshop, there are three CNAME record pairs. Make note of these CNAME records.


## Adding CNAME records


After requesting the certificate, you must validate each fully qualified domain name to get it issued. To validate these domains, you should place in each ACM CNAME name / value into its corresponding authoritative public DNS zone.

The first question of the workshop is:

How do you determine exactly where to put the ACM CNAME records?

Hint
You will need to determine the authoritative nameservers for each domain. For each domain in the certificate, you can use the dig or nslookup command-line tools.

Resolution
You can perform a DNS query using dig or nslookup for each fully qualified domain name. You can specify the DNS query to be type “NS” for Nameserver. Nameservers (NS) indicate the authoritative nameserver(s) for the given domain.


$ dig NS example.com
Output:
; ANSWER SECTION:
example.com. 60 IN NS ns-100.amazondomains.com
example.com. 60 IN NS ns-200.amazondomains.com
example.com. 60 IN NS ns-300.amazondomains.com
example.com. 60 IN NS ns-400.amazondomains.com

Both dig and NSlookup return the same results. It indicates that example.com nameservers are: ns-100.amazondomains.com, ns-200.amazondomains.com, ns-300.amazondomains.com, ns-400.amazondomains.com

These results mean that one should put in the ACM CNAME records for example.com in the DNS zone/hosted zone that contains those nameserver values ns-100.amazondomains.com, ns-200.amazondomains.com, ns-300.amazondomains.com, ns-400.amazondomains.com.


In Route53, you can use the AWS console or the AWS CLI to insert CNAME records.


In the Route53 console you can add the CNAME records by following these steps.

1.Navigate to the Route53 console  and select your Hosted zone.
2.Select “Create record”.
https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/route53/createrecord.png

3.Select Simple Routing, then "Next".
4. Select Define simple record. The Record type should be CNAME. Enter in the CNAME name and value.

https://static.us-east-1.prod.workshops.aws/public/31a7db0c-69b4-46cb-ba45-8c2290460d65/static/images/route53/cname.png


## Checking Your Work

Now that the CNAME records are added to their respective DNS zone/hosted zone, one might question if everything is correctly in place.

How would one verify that the CNAME records are in the correct DNS zone/hosted zone?

Hint
dig and nslookup can solve this. Think of what type of DNS record type should be used. It's not NS.
Resolution
Make a DNS query of type CNAME, specifing the CNAME name, like _a1b2c3d4567890abcdef-EXAMPLE11111.example.com. The reply should be the corresponding CNAME value. For example: uoc1c1qsw7poexampleewjeno1pte3rw.3ym756xh7yj.acm-validations.aws. If the output is blank, the CNAME record might be in the incorrect hosted zone.

It is important to note that some DNS providers can take 24–48 hours to propagate DNS records.
After the CNAME record has propagated in DNS and you can successfully query the CNAME record. When ACM validates the domain name(s), the Validation status is updated to Success. After the certificate is issued, the certificate status is updated to "Issued".

It may take up to several hours for ACM to validate and issue the certificate.

dig

nslookup
$ dig CNAME _a1b2c3d4567890abcdef-EXAMPLE11111.example.com
Output:

;; ANSWER SECTION:
1.
_a1b2c3d4567890abcdef-EXAMPLE11111.example.com 300 IN CNAME
2.

## Scnario
