# What is mTLS?

 Mutual transport layer security(mTLS) establishes an encrypted tls connection in which bot parties use X-509 digital
 certificates to authenticate and verify each other. mTLS can help mitigate the risk of moving services to the cloud and
 prevent malicious third parties from imitating genuine apps.

 For example:
 When Alice wants her web browser to connect to bob's secure web server the transport layer security tls
 protocol scrambles and protects their commnications and verifies that the server indeed belongs to bob. She's never
 visited bob's webstise before but her browser trusts bob's identity instantly, Why? Because a certificate authority
 created and issued an x-509 digital certificate to bob once he proved he owns that domain name.

 What if Alice and Bob need stronger authentication? in that case, they might want to mutually authenticate each other,
 not only does Alice verify Bob's identity but Bob authenticates and verifies Alice's identity. Over the web, the
 authentication process can be performed using digital certificates and our old friend mTLS.

mTLS is really useful in microservices-based apps and for fully securing cloud-based services such as web application
firewall or cdns.
