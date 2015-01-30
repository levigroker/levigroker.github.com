---
layout: post
title: "Apple's In-App Purchase Receipts"
date: 2015-01-30 15:36:20 -0700
comments: false
categories: 
---
Recently I've been reading up on Apple's In-App Purchase system with the goal of understanding how to support various types of purchases. It seems to me that non-consumables are kind of the "poster child" in-app purchase, for the typical "remove adds" or "unlock content" models. Which makes sense, because, in-app purchases are pretty simple if you are talking about auto-renewing subscriptions, or non-consumable items. Simple because Apple sends you all the information you need in their receipt, and persists that information for you. The only thing you're required to do is allow the user to "restore purchases" which basically gives you the receipt information back as if a purchase was made. Easy.

Things get more complicated when you want to handle the *other* types of purchases. Specifically consumables and non-renewing subscriptions. Apple pretty much says: "Here's the receipt for your purchase... you better keep track of it for your user. Oh, and we're never giving you this information again..." or at least that's what it sounds like to me when reading through the "Persisting Using the App Receipt” section of the [In-App Purchase Programming  Guide](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/StoreKitGuide/Chapters/DeliverProduct.html)

 Where it says:

>Information about consumable products and non-renewing subscriptions is added to the receipt when they’re paid for and remains in the receipt until you finish the transaction. After you finish the transaction, this information is removed the next time the receipt is updated—for example, the next time the user makes a purchase.

So that means, you, as the developer, must have your own server to maintain a record of these kinds of purchases, and allow a mechanism for your user to restore these kinds of purchases (on a new device, for instance). That means building and maintaining your own server to handle this, or integrate with something like [Parse](https://parse.com). Not the end of the world, but...

That's kind of crummy.

Not only do you need to build/integrate with the server component, but now you must ask your users to create an account with your system so you can identify them to associate their purchases with their account. I'm sure users are used to this, but it's still a drag.

So, I searched around and found  [this post](http://www.progressconcepts.com/blog/non-renewing-subscription-app-purchases-ios/) which stated that non-renewing subscription information persisted in the receipt from Apple. Wait, what? This post is obviously crazy, the In-App Purchase Programming  Guide said that didn't happen...
 
But now, while reading through Apple's [Receipt Validation Programming Guide](https://developer.apple.com/library/ios/releasenotes/General/ValidateAppStoreReceipt/Chapters/ReceiptFields.html#//apple_ref/doc/uid/TP40010573-CH106-SW1) there's this statement in the "In-App Purchase Receipt" section:

> The in-app purchase receipt for a non-consumable product, auto-renewable subscription, non-renewing subscription, or free subscription remains in the receipt indefinitely.

Note the *non-renewing subscription* is listed as remaining "in the receipt indefinitely."

So maybe that post wasn't crazy, and we could use the receipt for non-renewing subscriptions(!) without the need for a server component. That's super interesting...

...but you're still out of luck if you want to deal in consumables.

P.S. I've provided Apple with this feedback for their In-App Purchase Programming  Guide documentation; feel free to do so as well. 
