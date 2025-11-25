---
title: Setup Shopify Domain On Third-party DNS
date: 2025-11-24
tags:
- shopify
- DNS
- email
- email deliverability
description: Setting up a domain on a third party DNS server for optimal email deliverability
permalink: posts/{{ title | slug }}/index.html
---

## DNS Zone configuration
If you are just running an online store and the site is going to host your cool shop then set up an
A record like this.

- Type: A
- Name: @
- Data / IP: 23.227.38.65
- TTL: 1 hour / 3600 seconds

If you have a site and you just want a small store to sell a cool widget you started making then you
will need to set up a CNAME record for a sub-domain. eg. mycoolsite.com -> shop.mycoolsite.com

- Type: CNAME
- Name: shop / store / dealers choice
- Data: shops.myshopify.com
- TTL: 1 hour / 3600 seconds

[Shopify domain setup](https://help.shopify.com/en/manual/domains/add-a-domain/connecting-domains/connecting-to-godaddy)

## E-mail Setup
The sender email address is the one that will show up in the From field of the recipients email.

Steps:
1. From your Shopify admin, go to Settings > Notifications
2. In the Sender email section, enter your email address.
3. Click Save.

Add CNAME records to your third-party domain

You can help to ensure that your email messages reach your customers by adding CNAME records to your
third-party domain to connect it to the Shopify SPF and DKIM records. You don't need to add any IP
addresses to your domain - the CNAME records are sufficient for authentication.

To add CNAME records to your domain, you need to have the following information:

your login information for your third-party domain provider account associated with your sender email
your sender email, which is the email address that you use to communicate with your customers

Steps:

1. From your Shopify admin, go to Settings > Notifications.
2. In the Sender email section, click authenticate your domain.
3. Follow the instructions to enter the new CNAME records into your third-party domain manager.

Changes can take up to 48 hours to complete. If the domain verification fails, then verify that the 
DNS records that you entered into your third-party domain provider match the records that were 
provided in your Shopify admin.

If you need help to add a CNAME to your domain, then contact your domain provider.

[Shopify email setup](https://help.shopify.com/en/manual/intro-to-shopify/initial-setup/setup-your-email#change-your-sender-email-address)
