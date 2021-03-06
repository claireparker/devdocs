---
layout: default 
group: release-notes
subgroup: 05_techbull
title: Image Resize Issue with Magento version 2.1.6
menu_title: Image resize issue Magento version 2.1.6 (April 27,2016)
menu_node: 
menu_order: 4
version: 2.1
github_link: release-notes/tech_bull_216-imageresize.md
---

*Technical bulletin first published on April 28, 2017.*

These instructions apply to anyone upgrading to Magento Community Edition (CE) or Magento Enterprise Edition (EE) version 2.1.6 whose installation runs custom themes. 

### Who needs this information?
You need to read this bulletin if you've upgraded to Magento 2.1.6 (CE or EE) and are experiencing any of the following issues: 

* all images on the product front end appear reduced in size, including the base image

* images do not have uniform sizes -- some are greatly reduced, while others appear as expected. (This particular anomaly occurs after you've run the `bin/magento catalog:images:resize` command.)

You cannot change the size of the base image within the gallery by editing `etc > view.xml`. Instead, using this method increases image size as well as gallery size, which renders the image out of alignment.


### Background

Magento 2.1.6 introduced a change in how the platform handles images. Previous versions of Magento did **not** set this value  to **true** by default in the blank {% glossarytooltip d2093e4a-2b71-48a3-99b7-b32af7158019 %}theme{% endglossarytooltip %}. However, Magento 2.1.6 now sets the value of the `<frame></frame>` option to **true** by default. 

#### What does the `<frame></frame>` element do?

The `<frame></frame>` element adds white space around your image. Using this option will impose a specific width and height in `view.xml` while preserving the desired {% glossarytooltip 0b54d17c-5c5d-4f6e-b8f7-f7efb860713a %}aspect ratio{% endglossarytooltip %}. 

If an image doesn't contain a `<frame>` element, Magento uses the `product_image_white_borders` option in the Catalog module. This setting typically has a value of **1**,  but is overridden to **0** in the default Luma and Blank themes. If your custom theme doesn't include this option, then Magento will rely upon the setting in the Catalog module for the value of  the `frame` option.


### Suggested workaround

<div class="bs-callout bs-callout-warning" markdown="1">
The third step of the following procedure can be highly time-consuming, and Magento performance will be degraded while the workaround is in progress. However, you can run `catalog:image:resize` on your staging server to avoid the downtime in your production environment. After you've done that, copy the resized images to `pub/media/catalog/product/cache/*` on your production server.
</div> 

To prevent Magento from resizing images after upgrade, you must include `<frame>0</frame>` within that image definition in `view.xml`.  

1. Update your `view.xml` file by confirming that the parameters related to the image frame contain these two settings: 

	* `<frame>0</frame>` 

	*  `<vars module="Magento_Catalog">
        <var name="product_image_white_borders">0</var>
    	</vars>`

2. Clean your full-page cache. See [Manage the Cache](http://devdocs.magento.com/guides/v2.0/config-guide/cli/config-cli-subcommands-cache.html){:target="_blank"} for more information on Magento {% glossarytooltip 0bc9c8bc-de1a-4a06-9c99-a89a29c30645 %}cache{% endglossarytooltip %} types and procedures for managing caches. 

3. Manually resize all your images using the `bin/magento catalog:images:resize` command.  


### More information

For further discussion of this issue, see [GITHUB-9385](https://github.com/magento/magento2/issues/9385){:target="_blank"} and [GITHUB-9395](https://github.com/magento/magento2/issues/9395){:target="_blank"}

## Credits
Dear community members, thank you for your prompt identification of this issue.  






