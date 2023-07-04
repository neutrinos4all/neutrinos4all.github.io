---
title: "Learning to value my time"
date: 2018-04-02
slug: 'valuing-time'
tag: 'opinion'
---

This is one of the most important lessons I learned while working from home. If you don't value your own time, who will?
<!--more-->

I've worked from home for the past two years. I'm nearly finished with grad school and am in the final stages of dissertation writing, and for the past two years I worked part-time as a remote employee for an engineering company in Pittsburgh, PA. Balancing a secondary job with a research assistantship and my own research and writing, while living 90 minutes south of Buffalo, has meant that I do nearly all of my work from home and have to balance my duties pretty carefully.

At the same time I use Linux as my primary OS and I really enjoy tinkering with technology, a combination which roughly translates to "sometimes my tweaks break the system and I have to spend three hours troubleshooting, which I actually enjoy because I usually learn something new." I don't mind diagnosing system quirks, but as my schedule has progressively filled itself with more important tasks, I found about a year ago that I did mind how much time these things took when I had more pressing tasks to complete. 

Working from home and having an hourly rate that I charged for my services, I saw that I could either earn money on work tasks or spend money fixing my self-inflicted wounds or puttering with unnecessary activities. This analysis recast my time-consuming tinkering into a money-consuming, non-essential diversion. This is particularly true because Linux system administration is not my career and I therefore cannot approach this tinkering, and the money it costs, as a paying-for-learning endeavor. Because I dislike wasting money, my perspective on excessive tinkering has changed.

I've thought of myself an adult generally since 2012, when I was suddenly put in charge of the future of 185 students trying to pass physics and graduate high school. Even after returning to grad school for engineering, I still worked for an actual engineering company with deadlines and personnel management requirements, paid taxes, published papers, took vacations, had a car payment, got engaged, and participated in other activities of adulthood. However, I think this might be the last vestige of my earlier days where time spent screwing with dot-files or writing Bash scripts "just to see if I could" was without penalty. Those days may be over.

It also means that I'm a little more willing to pay for software if its cost is a net savings over my time. For example, after some testing I recently bought a filesystem backup program license from [Cloudberry](https://www.cloudberrylab.com/). I'm using it to back up some critical files to AWS [Glacier](https://aws.amazon.com/glacier/) as a catastrophic data loss contingency plan, and it has several features that I like: deduplication, file versioning, compression, and client-side encryption, among others. Because you need to write code anyway for most interactions with Glacier, my needs aren't that complex, and Linux provides native utilities for handling most of these tasks individually, I could probably write a Bash script to handle most of them, then add it to crontab and schedule an automated backup. Presto, homebrew Cloudberry.

But once I started thinking about valuing my time, I realized that even if it took an hour to write up this script the price for a license would come in well under my hourly rate. I also didn't want to take the chance of writing something incorrectly and somehow end up corrpupting my catastrophic, last-ditch file storage solution. So I ended up paying for it and I felt good doing so. I anticipate doing so in the future.

Anyway, this post really isn't supposed to be more than a bit of self-reflection on what I've learned, and it certainly does provide a handy heuristic for managing my time and certain expenditures in the future. 