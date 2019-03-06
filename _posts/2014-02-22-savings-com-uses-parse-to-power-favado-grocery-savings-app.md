---
id: 2112
title: Savings.com Uses Parse to Power Favado Grocery Savings App
date: 2014-02-22T00:24:28+00:00
author: courtneywitmer
layout: post
guid: http://blog.parseplatform.org/?p=2112
permalink: /non-technical/savings-com-uses-parse-to-power-favado-grocery-savings-app/
dsq_thread_id:
  - "3704192875"
categories:
  - Customers
tags:
  - US
---
[<img style="border: 0pt none; float: right; padding-left: 10px; padding-bottom: 10px;" src="{{ site.url }}/assets/wp-content/uploads/2014/02/Favado.png" alt="Favado app open on iPhone 5" width="326" height="627" />]({{ site.url }}/assets/wp-content/uploads/2014/02/Favado.png)Saving money has become part of the American way, especially as the economy recovers from the recession of 2008. Thankfully, today’s shoppers looking to make smart purchasing choices have tools such as <a href="http://www.savings.com" target="_blank">Savings.com</a> to help them save time and money.

Currently, Savings.com offers two products to address this goal. The first is the website, www.savings.com, where shoppers will find one of the world’s biggest databases of online coupon codes and deals. Five million shoppers turn to Savings.com every month to access the best deals on the web, generating $1 billion in sales annually for the website’s retail partners.

Now, the company has released a new offering, <a href="https://www.favado.com/" target="_blank">Favado</a>, a mobile app built to help people save money at the grocery store. Leveraging their excellent relationships with the nation’s top grocery bloggers, who sift through circulars and coupon books to find the very best deals and matchups across the country, Savings.com is able to power the coupon data in the app. With the app, the company was confident they could provide a solution that helps everyone save up to 70% on their weekly grocery bill.

Savings.com CTO Joe Zulli says that the goal for Favado is to make it simple and fun to save meaningful amounts of money at the grocery store. To achieve this, they’ve assembled the country’s largest database of grocery store sales and “matchups” from 65,000 stores nationwide. Favado builds these matchups for you, and presents them in a list that is personalized to the products that you often buy. From there, you can select the items that interest you and put them in your weekly shopping list.

Favado is the first mobile app that Joe and his team have built. They turned to Parse after it was recommended to them by a colleague to help solve some of the pain they were experiencing as they worked to get push notifications to work across both iOS and Android. After investigating the option, the team decided to use Parse for sending out push notifications from their backend servers to users. For example, Favado users can subscribe to a store to get notified when new sales and matchups are available for that store. When a batch of sales gets entered into the system, a process gets triggered, which uses Parse to send a push notification to every user who has subscribed to that store that the sales are for

After looking into Push, the Savings.com team experimented with the rest of the Parse platform and decided to use Parse Data for storing miscellaneous reference data, such as their zip code database that is used for geo-spatial lookups. Although the team has a backend of their own as well, Joe says they’ve found that, “by pairing it up with Parse, we have been able to focus our home-grown backend on functionality that is specific to the needs of our particular use case.”

The app also uses Parse Social for login and signup functionality. This gave Joe and his team a quick and easy way to incorporate secure login that works across Android, iOS, and the web, which Joe says, “would have been much more expensive in terms of both resources and money to build by hand.”

In the two months since launch, Favado has been downloaded by hundreds of thousands of people across the U.S. It has excellent reviews in press and in the app stores, but is also spreading powerfully via word-of-mouth. The app is experiencing great engagement, with 60% of shoppers opening the app several times a week. However, the team sees the true measure of success as their users’ ability to save money and are most proud of the fact that they have heard directly from users that the app is saving them **up to 70%** on their weekly grocery bill.

After working with Parse, Joe feels that the main benefit to using the platform is in opportunity cost savings. As he sees it, “time spent on infrastructure is time that you aren’t spending building solutions to delight your users and uniquely position your app in the marketplace. That’s not to say that backend infrastructure isn’t critically important, because it clearly is. There would be no Favado without its backend. However, by offloading the necessary, but “commodity” engineering work, you and your team can be freed up to focus on your users, which benefits everybody.”

Everyone, whether you have never clipped a coupon, or you are a coupon pro, can save big at the grocery store with Favado (available on both <a href="https://itunes.apple.com/us/app/favado-grocery-sales/id703787093?mt=8" target="_blank">iOS</a> and <a href="https://play.google.com/store/apps/details?id=com.savings.android.grocery" target="_blank">Android</a>). You can visit Favado.com to read savings stories from users, as well as check out press coverage from USA Today, Forbes, and the Los Angeles Times.
