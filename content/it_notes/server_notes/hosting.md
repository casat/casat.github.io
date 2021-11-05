---
title: "WordPress hosting notes"
weight: 1
draft: true
---
---

This section is specifically for configuration examples and notes about the setup of our WordPress servers. Currently all of our servers reside on Virtual Private Servers (VPS) that are hosted by [DigitalOcean.](https://digitalocean.com) Beyond the physical hosting of our servers, DigitalOcean also handles the Networking (DNS) of all of our WordPress websites while the actual domain names are purchased in [Namecheap.](https://namecheap.com)

## Namecheap and domain names

When setting up a new website or alias we purchase our domain names through Namecheap, where all of our other domains are. Once a domain name is purchased the Nameservers need to be updated to reflect where the actual server will be hosted. In most cases this means that the Nameservers, in Namecheap, will need to point to DigitalOcean. You can do this by finding the domain in Namecheap and updating the DNS section to 'custom DNS' and then adding DigitalOcean's nameservers to the list.

{{< hint info >}}
If you need more information here's where you can find [Namecheap's tutorial here](https://www.namecheap.com/support/knowledgebase/article.aspx/10375/2208/how-do-i-link-a-domain-to-my-digitalocean-account/), and here's another [from DigitalOcean.](https://www.digitalocean.com/community/tutorials/how-to-point-to-digitalocean-nameservers-from-common-domain-registrars)
{{< /hint >}}

## DigitalOcean and server hosting

Currently we have projects spread out across providers; Linode - roar.nevadaprc.org, AWS - training.casat.org, and DigitalOcean - everything else, but DigitalOcean is our 'primary' provider.
