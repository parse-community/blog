---
id: 2588
title: 'Parse Push Experiments: Re-engage more powerfully and more creatively with A/B testing'
date: 2014-11-03T18:39:08+00:00
author: stanleywang
layout: post
guid: http://blog.parse.com/?p=2588
permalink: /learn/parse-push-experiments-re-engage-more-powerfully-and-more-creatively-with-ab-testing-2/
post_format:
  - feat-image
feature_image_style:
  - small
dsq_thread_id:
  - "3612855842"
image: /wp-content/uploads/2014/11/push_experiment_header-1024x416.png
categories:
  - Learn
---
Today, we're excited to announce Parse Push Experiments, a new feature to help you confidently evaluate your push messaging ideas to create the most effective, engaging notifications for your app. With Parse Push Experiments, you can conduct A/B tests for your push campaigns, then use your app's realtime Parse Analytics data to help you decide on the best variant to send.

As most developers know, push is one of the best ways to re-engage people in your mobile app. Parse already makes it easy to engage with people through push — in fact, in the past month, apps have sent 2.4 billion push notifications with Parse. We built Parse Push Experiments to make push engagement even simpler and more powerful, and to solve a common problem you may have experienced while designing your push strategy.

## Parse Push Experiments in Action

Say you've just built a beautiful app on Parse and successfully launched it in the app store. You're confident about the in-app experience, but want to make sure your users stay engaged with new content in your app, so you come up with some creative ideas about how to enrich your app with push messaging. Now you need a reliable way to tell which of your ideas will generate a better open rate. You could send one message today and the other one tomorrow, then see which one performs better — but what if some external factor, like getting featured in the media, spikes your app's popularity tomorrow? That might lead you to incorrect conclusions. In order to fairly compare two push options, you need a way to conduct a push messaging experiment while holding all external factors constant, and only changing the thing that's being tested — the notifications themselves.

Here's how it works with Parse Push Experiments:

<ol class="standard-list">
  <li>
    For each push campaign sent through the Parse web push console, you can allocate a subset of your users to two test groups. You can then send a different version of the message to each group. <div style="margin-bottom: 30px; margin-top: 20px; margin-left: -10px;">
      <table>
        <tr>
          <td>
            <img class="aligncenter size-full wp-image-2589" src="{{ site.url }}/assets/wp-content/uploads/2014/11/push_experiment_enable-e1414950172814.png" alt="push_experiment_enable" />
          </td>
          
          <td>
            <img class="aligncenter size-full wp-image-2590" src="{{ site.url }}/assets/wp-content/uploads/2014/11/push_experiment_messages-e1414950142517.png" alt="push_experiment_messages" />
          </td>
        </tr>
      </table>
    </div>
  </li>
  
  <li>
    Afterwards, you can come back to the push console to see in real time which version resulted in more push opens, along with other metrics such as statistical confidence interval. <div style="text-align: center; margin-top: 20px; margin-bottom: 30px;">
      <img class="size-full wp-image-2592" src="{{ site.url }}/assets/wp-content/uploads/2014/11/experiment_results.png" alt="push_experiment_results" />
    </div>
  </li>
  
  <li>
    Finally, you can select the version you prefer and send that to the rest of the users (e.g. the “Launch Group”). <div style="text-align: center; margin-left: 2px; margin-top: 20px; margin-bottom: 30px;">
      <img class="size-full wp-image-2592" src="{{ site.url }}/assets/wp-content/uploads/2014/11/experiment_launch.png" alt="push_experiment_launch" />
    </div>
  </li>
</ol>

## A/B Testing with Parse Push Experiments

In addition to testing content, you can use Parse Push Experiments to A/B test when you send push notifications. This is useful if your app sends a daily push notification and you want to see which time of day is more effective. You can also constrain your A/B test to run only within a specific segment of your users; for example, you might want to run an A/B test only for San Francisco users if your push message is location-specific. Finally, A/B testing works with our other push features such as push-to-local-time, notification expiration, and JSON push content to specify advanced properties such as push sound. For more details, check out our <a title="Parse Push Guide" href="https://www.parse.com/docs/push_guide#experiments/iOS" target="_blank">push guide</a> and read tips for designing push notifications <a href="http://blog.parse.com/2012/11/26/dont-be-pushy-10-useful-tips-for-awesome-push-notifications/" target="_blank">here</a>.

Push A/B testing works with existing Parse SDKs that you may already have in your app (iOS v1.2.13+, Android v1.4.0+, .NET v1.2.7+). To try it, simply use our web push console and enable the "Use A/B Testing" option on the push composer page. So let your creativity flow, brainstorm new push campaign ideas, and experiment! We can't wait to see what you'll create.