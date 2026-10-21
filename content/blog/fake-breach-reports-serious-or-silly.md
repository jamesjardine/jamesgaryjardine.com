---
title: "Fake Breach Reports: Serious or Silly?"
date: "2026-06-14T13:59:59"
slug: "fake-breach-reports-serious-or-silly"
categories: []
tags: ["Breach", "breach report", "Security", "security awareness"]
---

There was an interesting report that recently came out regarding fake data breach reports being submitted to the Main Data Breach Reporting Portal. The fake reports were focused on VRChat and Discord. According to the reports (article linked below), a user was able to submit a breach notification to the portal of a company they are not a part of. It might not seem like a big deal, but the more I think about it, the more I consider different risks and costs for the companies at hand. In addition, there is a potential risk for a sophisticated social engineering attack from this. Let’s talk about it.

**User Perspective**

It might not seem apparent, but there was a specific threat scenario I thought of right away. Since the actual issue is not directly related to the end user, it will typically lead us to a social engineering attack of some sort. One of the things that makes a social engineering attack more credible is using official sites. In this case, it is the Main Data Breach Reporting Portal.

Imagine that an attacker has identified the company they want to target. They create a fake data breach report and get it on an official, trusted site. The stage is set. Not there are a few different options that could happen (and of course these are not all of them). Two potential options are to send an email asking users to login in to change their password and reference the legitimate site. However, rather than take them to the real site for password reset, they are directed to a malicious site controlled by the attacker, that looks like the official site. The attacker captures the credentials and takes over the account.

In scenario two, the attackers alert the user to the breach as if it is from the official company. The user is then told they have received free identity protection services and are provided a link to sign up. The link is to the attacker controlled site and may collect personal details that shouldn’t be shared.

These are just two examples, and if you know me, you know how much I hate how we default to social engineering as the the only attack vector. Don’t get me wrong, I do not see this as a common attack vector, but more so just highlighting how this could be used.

**Company Perspective**

Let’s switch gears and talk about the target company concerns when a fake breach report is submitted. I want to start with the idea that a company is often guilty until proven innocent. What I mean by that is people are quick to believe what they hear. If someone starts the rumor that a company was breached, that will spread quickly without any fact checking. Add in this situation where a simple fact check to a trusted site confirms the report and now a company has to start playing catchup.

This comes with many potential costs, both from monetary and resource related. I can already see the panic setting in when they get this word that they have been breached. Sure, you can say that you, or no one else from your company submitted that report, but in today’s landscape, that is going to be tough. Even if you are able to quickly say it is not true, there is going to be some level of effort to verify that before responding.

From a business perspective, there is going to be some level of effort to verify no breach actually occurred. Additionally, the company may have to try to contain any reputational harm. What goes on the internet, stays on the internet. It is tough to put a genie back in a bottle. So when you get negative exposure there is going to be work to try and correct that.

We do see this already with researchers publicly announcing vulnerabilities and issues with organizations. I would like to hope that in many of those cases they have the evidence that shows their claims are real. I think this is different then a false report on a trusted site.

**Wrap Up**

This reminds me a little of a few years ago when hackers filed a complaint with the SEC when a company didn’t want to play ball with them and the company failed to file an 8K report (link below). We have seen some interesting tactics over the years. This is interesting because we don’t really know the motive behind it. Maybe it was just because they were upset with the companies targeted, bored, or had some other malicious intent.

It does raise questions about how these state breach portals work and verify the claims that are submitted. Maine ended up taking their site offline to review how the process works so that they can make proper changes to stop this from happening in the future. It will be interesting to see what process changes happen to help solve the problem. Like all new tactics we see, this is another mole that has popped its head up that security will whack down while we wait to see what else pops up.

Events like this are interesting, but not earth shattering. I think it is valuable to be aware of these things as it raises our awareness, but shouldn’t be seen as critical events until we actually see evidence that warrants that.

**References:**

> [Maine Takes Data Breach Reporting Portal Offline After Fake VRChat and Discord Filings](https://cybersecuritynews.com/maine-data-breach-portal-offline/)

> [Hackers Complain to SEC Company They Hacked Failed to Disclose the Incident](https://www.dandodiary.com/2023/11/articles/cyber-liability/hackers-complain-to-sec-company-they-hacked-failed-to-disclose-the-incident/)
