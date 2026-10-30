---
title: PHP2Shell and Number Manipulation
date: 2026-10-30T22:19:46
draft: false
slug: php2shell-and-number-manipulation
categories:
  - Uncategorized
tags:
  - php2shell
  - Security
---
When you think about security, often times big numbers are attractive because they indicate something, well, big. We focus on the highest numbers we can find because it looks impressive. 

> **Our firewall blocked 7 million attacks yesterday.** 

This sounds great to an executive because it gives the impression there were actually 7 million attacks yesterday. But the reality is that a portion of those were not actual attacks. Put anything on the internet and scanners start hitting it right away. 

Now this isn't to say that there aren't valid attacks happening, because there are. But the numbers probably aren't as representative of reality as we want to think. This goes for most numbers in security. 

## PHP2Shell
The recent release of PHP2Shell got me thinking more about this because I saw some posts indicating that 70% of the internet runs on PHP. 

I am not here to argue for or agains that number, actually it is irrelevant for my discussion.  The number is used to imply how devastating this issue is by how much of the internet is affected. However, I would argue that it really doesn't tell us much. 

70% of the internet is a lot, I don't deny that. However, a lot doesn't mean critical. So of that 70%, how much of that is critical infrastructure? I don't have exact numbers, but I would guess that WordPress makes up a hefty portion of PHP driven sites. A large portion of those are probably personal or marketing sites. 

Those types of sites create risk, but it is more indirect than say the hospital, banks, or utilities shutting down. These sites end up being used in social engineering attacks. They create an opportunity for watering hole attacks or ways to trick people into giving up credentials or other sensitive information. 

A risk, yes. Is it the end of society? Probably not. 

## Mitigations
I would also like to point out that this specific issue was known and shared with many major vendors that provide WAF protections for websites. It was very public when the patches were going to be released so people could be ready to patch. having major CDNs or WAFs applying mitigating rules automatically helped reduce the initial footprint of this issue while people worked to patch.

## Is It Just FUD?
So the question is whether or not the 70% push is a true reason for concern, or is it an overreaction to sell more security. This is referred to as Fear, Uncertainty, and Doubt (FUD). 

It is honestly difficult to say which way it is because there really isn't a lot of information.  To really understand the effects, we would have to know what applications/services are running on PHP to determine if they are critical or not. 

I have no doubt there are some critical applications running on PHP. I doubt it is anywhere near 70%. 

## Wrap Up
The real purpose of this post is not about PHP2Shell and whether or not it is serious or not. The goal is to get people thinking about the way numbers are used in cybersecurity and to start asking more questions. Dig into these values to understand what they actually mean. 

Saying that you are blocking 7 million events a day paints a really blurry picture.  How many of those are your own scanners?  How many are just default bots just probing the internet collecting data.  What about bots and crawlers. For most of us, most of these are not attempting malicious exploits against us. 

As you start to get more answers, you get a clearer picture of what your risk actually looks like. In the end, that is going to matter more than the random number of events blocked at some edge device. 