# Getting started with The Wishlist #
## Developers' guide ##

### What is the Wishlist ###

The Wishlist is a microservices-based, API-enabled engine that empowers retailers to understand and manage list of products their customers want to purchase.

**Automated notifications** can be configured and sent to customers when products are low in stock, back in stock, or available at a discount.

The Wishlist also integrates to **Point Of Sale (POS)** in-store. This makes it an ideal way for retailers to bridge online and offline services.

The Wishlist integrates web, store, marketing and ERP platforms and operations, and is designed to be flexible and scalable.

At its core is the wishlist (The Wishlist platform provides a **master record** for the wishlist), customers and products. 

We also **integrate inventory, pricing, stores and staff details**. This means we provide effective and timely marketing communications and reporting of wishlist activities.

### Basic concepts ###

- The Wishlist provides world-class wishlist capabilities for **ecommerce platforms** and in-store retailers
- Developers integrate the Wishlist into their ecommerce applications using the Wishlist's **REST and JavaScript APIs** 
- Typically The Wishlist platform gives you **real-time integration with ecommerce, marketing, and often Point of Sale systems**. Wishlist data is exchanged between these systems, with The Wishlist as the middleware platform
- You can also gain real-time or near real-time integration with customer, pricing and inventory systems. We integrate with these systems so **notifications** can be sent to retailers' customers when an item on their wishlist is out of stock, back in stock, or on sale.
- We also may have **integration with the retailer's sales, product, staff and store data**. These data types are for marketing, attribution and reporting. They're typically integrated via batch processing/synchronisation.
- In the wishlist, customers are identified by, at minimum, their email address (this **must be unique**, with no other account attached to the email address) 

![Common identifier](docs/assets/common-identifier.png)

### Platform components ###

In addition to the core Wishlist platform, you get the following modules:

- **Store Owner portal** allows retailers to customise system configurations and analyse system performance
- **Shopify app** is a turnkey solution for Shopify merchants. Provides both frontend customisation and backend integration to The Wishlist
- **Point Of sale elements** provide an optional React-based framework allowing retailers to offer wishlist service in-store, plus broader clienteling features to in-store staff
- **Marketing integrations** allow third-party platforms &ndash; such as Emarsys, Klaviyo and DotDigital &ndash; to be used as the platform for Wishlist notifications. The Wishlist also offers an inbuilt email marketing platform.

![Microservices](docs/assets/microservices.png)

### Notifications ###

The Wishlist automatically sends marketing communications to customers based on events and the settings retailers create in the **Store Owner Portal**. 

For example, you can notify customers based on:

- Regular schedules (eg reminder emails)
- Stock levels (eg when an item is low in stock or back in stock)
- Price changes (eg when an item is on sale)

Key considerations for notifications:

- **Product must exist for notifications to be triggered**<br> 
Customers can add products to their wishlist, even if the product isn't available in the product catalogue. However, the product must exist with attributes such as image, price and description. These attributes have to be sent to the marketing platform with the notification payload. Otherwise the notification won't trigger.
- **Stock-based notifications**<br>
The inventory must be loaded and synchronised into The Wishlist. For price-based notifications, pricing must be loaded and synchronised into The Wishlist.
- **Emarsys integrations**<br>
The Wishlist sends all wishlist changes, with wishlist notifications managed in Emarsys.
- **Klaviyo**<br>
Notifications are managed in The Wishlist. Retailers create **flows** in Klaviyo, and The Wishlist creates metrics that are used in the flows. The Wishlist then sends events and payloads to Klaviyo, triggering the appropriate flow.
- **DotDigital**<br>
Notifications are managed in The Wishlist. Retailers register the template associated with the notification in the Store Owner Portal. When a notification is triggered, The Wishlist sends the payload, along with the appropriate template iD, to DotDigital.
- **Email platform**<br>
Retailers have the option of using The Wishlist's built-in email platform. Retailers create templates in the Store Owner Portal.

## Data types ##

| Data type | Use | Special considerations |
| :---| :---|---|
| Customer | Wishlists must belong to a customer. A customer must exist before a wishlist can be created| Email is unique, and required
| [Wishlist](https://the-wishlist-co.github.io/docs/authenticationsvcApi.html) | A customer can have one or more wishlists.|The Wishlist Platform is assumed to be the master data manager of Wishlists|
|[Wishlist Items](https://the-wishlist-co.github.io/docs/authenticationsvcApi.html)|Each wishlist can have one or more wishlist items||
|Products|Marketing and notifications|A product does not need to exist before being added to a wishlist. However, it must exist before it can be sent as a customer notification| 
|Pricing|Marketing and notifications|Required for pricing notifications|
|Orders (sales)|Determining which wishlist items have converted to sales|Required for conversion and sales reporting|
|Quotes|A special type of wishlist allowing a business to offer quotes for usage in reminders and tracked for conversions|Optional module. Behaves a little differently to wishlists, and has its own set of notifications|
|Inventory| Track items low in stock or back in stock|Required for pricing notifications|
|Staff| Ability to attribute sales and conversions to staff|Required for staff attribution and optional clienteling module|
|Stores|Ability to attribute sales and conversions to store|Required for staff attribution and optional clienteling module|

## Using the REST APIs ##

### Authentication ###
The wishlist uses bearer authentication. Tokens expire after **5 hours**. For tenant details, contact The Wishlist team. [View more about Authentication API](https://the-wishlist-co.github.io/docs/authenticationsvcApi.html#authentication-api)

### Rest APIs ###
[View REST API documentation](https://the-wishlist-co.github.io/docs/#welcome-to-the-wishlist)

### Notes & special use cases ###

**Identifiers:** All The Wishlist platform entities have a unique TWC internal identifier. This is typically identified by the appendix id (eg ```customerId```), and an optional unique identifier specified by the customer (typically identified by the appendix reference, eg ```customerRef```). For customers, the email must be unique and is a required field.

**Deleting items:** When using the delete APIs, The Wishlist platform typically marks items for deletion without deleting them from the database. This is because the items are often needed for historical reporting.

**Automatically delete upon item purchase:** The Wishlist doesn't automatically delete an item after it's been purchased by a customer. If you don't want to leave the item in a wishlist, your developer must delete it manually.

**Added from cart:** For abandoned carts, you have the option to add all items to a wishlist. The Wishlist provides a flag ```addedFromCart``` for developers to record which items have been automatically added to The Wishlist. 

**Pre-release item:** This can be when retailers have pre-release items or collections. They may be accessed from a special URL, or only available to VIP customers. The Wishlist allows items to be flagged as "pre-release" when added to The Wishlist, so developers can identify these items in the future.

## Extended attribute model ##
The Wishlist gives developers the power to load additional unique attributes onto our standard entities, for their custom needs. These are **extended attributes** and are defined as key-value pairs.

For example, a developer may want to track a specific custom field against an entity. Maybe a supplier's name, expiry data, or other identifying fields. An item can be recorded and retrieved using standard APIs, but it's not possible to retrieve a record from only these values, because they can't be indexed by the platform.

## Shopify integration ##
[View Shopify integration documentation](https://the-wishlist-co.github.io/shopify-integration/app-configuration-wishlist-page.html)
