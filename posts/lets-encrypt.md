Hooray! [After much feedback](https://gearhost.uservoice.com/) Let's Encrypt support is now public and available on all Hobby and up CloudSite applications! Let's Encrypt is a little different than most SSL certificate providers. For starters it's free! Launched in April 2016 Let's Encrypt offers free X.509 certificates via an automated process designed to eliminate the current complex process of manual creation, validation, signing, installation, and renewal of certificates for secure websites.

A Let's Encrypt certificate is a domain-validated only certificate. Organization and EV certificates are not available so while this is a great free certificate option we would **strongly** recommend an Organization cert or EV certificate if you are running an e-commerce application where purchases are made. This validates your company and builds more trust with consumers than a domain-validated certificate.

### What's the catch
There is one small catch. While the certs issued are completely automated and free they last only 90 days. Let's Encrypt [designed this with a purpose](https://letsencrypt.org/2015/11/09/why-90-days.html). On GearHost these certs will expire after 90 days and will need to be reissued. If you are on a Hobby CloudSite this will have to be done through your control panel but Small, Medium and Large CloudSite applications will renew the certificate automatically. The 2nd caveat is your domain has to be pointed your CloudSite before a certificate can be issued but this should be a given right?

### How to enable it
To enable a Let's Encrypt certificate on your CloudSite simply log in to your GearHost control panel and goto the Certificates menu. From there click Create Certificate and follow the wizard. This process takes about 5 minutes or less.

### Thanks
Your feedback is what drives us at GearHost so we want to give a special thanks to those who [up-voted this suggestion](https://gearhost.uservoice.com/forums/147522-gearhost/suggestions/14816673-ssl-let-s-encrypt). Keep your ideas coming and we'll make sure to keep implementing them. Thanks again and enjoy!

By: [Ryan Kekos](https://twitter.com/ryankekos)