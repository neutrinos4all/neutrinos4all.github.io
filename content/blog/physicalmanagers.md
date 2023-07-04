---
title: "We should recommend physical password managers, too"
date: 2017-04-24
slug: 'physical-versus-digital-password-managers'
tag: 'cybersecurity'
---

Depending on user needs, an actual "password book" might not be a bad idea. It's certainly better than rampant password reuse.
<!--more-->

Sweeping, one-size-fits-all advice on digital security is not a good idea. It's never been a good idea, and as the number and variety of [people](https://time.com/money/3896219/internet-users-worldwide/) and [devices](http://spectrum.ieee.org/tech-talk/telecom/internet/popular-internet-of-things-forecast-of-50-billion-devices-by-2020-is-outdated) that come online [continue to increase](http://www.internetlivestats.com/internet-users/#trend), so goes our need to provide variegated recommendations for security best practices. Rather than telling users that they *all* need to use specific tools or follow particular strategies, we have to acknowledge that different users will require different solutions.

If not, we risk alienating users and driving them back to insecure behavior.

Consider the use of password managers. It's easy to find resources online [forbidding users](https://www.dataprivacyandsecurityinsider.com/2016/09/privacy-tip-53-valuable-lesson-dont-write-down-passwords/) from writing down their passwords, but I disagree. There *are* cohorts of users to whom we should be recommending physical storage of passwords. First, a basic explanation of digital password managers is given, followed by my argument. If you're familiar with digital password managers, feel free to skip the first section.

### What's a digital password manager? ###

These are tools that help users keep track of the passwords they use for different websites and programs. There are many different managers out there, including [some](http://keepass.info/) that don't sync to the cloud and [others](https://www.lastpass.com/) that practically live in your browser. The general premise behind password managers is the same: humans are really bad at routinely and accurately remembering the long strings of characters that are passwords, so let a program do a majority of the work for you. Rather than remembering all of them on your own, why don't you just remember one or two that you can use to access all of your other passwords? Then you can copy and paste them into the websites and programs you log into. It shifts a lot of the cognitive work off of the user and onto the program, which is much better suited for it than a human brain.

The alternative to outsourcing that cognitive work is to do it all with our brain, and because most humans aren't very good at it, we take shortcuts. One of the most widely-used shortcuts is password reuse, where we reuse one or a few different passwords everywhere. It's convenient, because using the same password means that I'll probably stand a better chance of remembering it, and no matter what I'm doing on the internet I won't forget my password.

The problem here is that, even if you use one very strong password, that password compromised on one service means that all other services using the same password can also be compromised. As more of our lives move online and hackers steal and then sell [enormous dumps of personal information](http://www.informationisbeautiful.net/visualizations/worlds-biggest-data-breaches-hacks/) (including [usernames](https://haveibeenpwned.com/) and passwords) online, cyber criminals can buy millions of passwords at a time, then [go try them](https://www.lifehacker.com.au/2016/09/dropbox-hack-this-is-why-you-shouldnt-reuse-your-passwords/) on a massive scale at the other most commonly used services on the internet.

Say, for example, you made an account at [ClixSense](https://www.clixsense.com/), a website where users could (and still do? really?) make accounts, watch ads, take surveys, and earn money doing so. In September 2016, [that site was hacked](https://arstechnica.com/security/2016/09/plaintext-passwords-and-wealth-of-other-data-for-6-6-million-people-go-public/) and criminals stole 6.6 million usernames, passwords, and email addresses that were stored in plaintext on the company servers. In other words, there was absolutely no data protection in place: no encryption at rest, no hashing, nothing. If people habitually reuse the same passwords across most or all of their different accounts, then threat actors can play the odds game and suppose that at least some of those people probably also had a Gmail account. So if someone registered with a ClixSense username `draco_malfoy` and a ClixSense password `awesomedrake33`, it's pretty trivial for cyber criminals to go try these account credentials *en masse* against Gmail logins and find at least a few users that did this very thing.

Suddenly Draco Malfoy's Gmail account, despite Google's strong track record of data security, gets pwn3d because some no-name website with crappy security (but that he trusted with his information) had their databases dumped with ease by hackers. This wouldn't have happened if he had used different passwords.

The solution is then to use different passwords for every website. Because that's burdensome and not realistic for most users to do with human memory alone, we encourage users to take advantage of digital password managers. [Don't write them down](https://security.illinois.edu/content/passwords).

### Why we should be recommending physical managers, too ###

This one-size-fits-all piece of advice is neither useful nor the best option for a plurality of users. Because there are so many people online, with varying levels of need and technological savvy, it's not realistic to expect everyone to use digital password managers. There will be corner cases, where perhaps the existing solutions are unreliable (like in-browser copy/paste functionality) or are needlessly complicated (like retirees that really maybe use Facebook, an email service, and perhaps some connectivity or healthcare apps [on their tablets](https://dcurt.is/the-death-of-the-tablet)). Chastising people for not using password managers, but then providing only complex software solutions, probably isn't beneficial for the users that need it most.

An actual 'password book' might be useful and workable security advice in certain circumstances.

Consider the retiree, a user less likely to follow strict digital security best practices or use a complex digital password manager. If we were to develop a threat model for an elderly user, someone stealing their password through a data breach and trying it elsewhere is probably much more likely than someone breaking into their home and stealing that password book. The cognitive burden of remembering different passwords for different services also means that elderly users may use only one or a few unique passwords. As an alternative, the pen-and-paper medium is probably more familiar and intuitive than a digital password manager for these users. If they maybe only use a handful of different online services (as compared to the [dozens](https://www.buzzfeed.com/josephbernstein/survey-says-people-have-way-too-many-passwords-to-remember) that a typical netizen might have), then the bother-to-benefit ratio for the retiree might skew towards a book, rather than a program. It therefore makes sense to recommend that these users create unique passwords and keep them written down.

This is probably a workable solution for most home users, too. Again, the threat model for home users should probably emphasize the risk around password reuse and [petty online criminals](http://www.zdnet.com/article/terrified-about-cyber-ninjas-you-may-be-missing-the-real-threat/), rather than the risk of someone breaking into their home and stealing their passwords. If a user isn't comfortable with digital managers and it would keep them from using the same password everywhere, then we should be encouraging them to use a book. If it improves their security posture and makes sense for the user, we should be recommending it---particularly if the alternative is rampant password reuse.

However, there do exist caveats for recommending physical password managers. First, this is not a good solution for workplace password management. Insider threats are now a thing, and criminals breaking into businesses could do a significant amount of corporate damage with the right credentials; in other words, the threat model is different, so infosec management practices must also change. Second, physical managers aren't helpful if the same password is still used everywhere. We're trying to fight password reuse, so recommending physical managers on the grounds of usability if only helpful if no reuse is made.

The bottom line is that human factors engineering is about finding solutions to problems for *every* human, rather than just a *majority* of them. We can't just neglect a small portion of our userbase because they're hard to design for, because it probably encompasses vulnerable users in particular need of tenable, reasonable solutions to serious problems. If a rising tide lifts all ships, then a variety of strategies for improving user digital security is essential. Digital security experts must keep this in mind, particularly because the majority of problems these professionals face can be highly technical in nature.

When giving security recommendations to modern humans in a complex world, simple solutions can sometimes be all that we need.