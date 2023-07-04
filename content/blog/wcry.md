---
title: "WCry, ransomware, and the challenge of legacy software"
date: 2017-05-17
slug: 'wcry-and-legacy-software'
tag: 'cybersecurity'
---

If organizations would just stop using Windows XP and patch their software, WCry wouldn't have been an issue. Unfortunately, that's often easier said than done for safety-critical systems built with now-retired software.
<!--more-->

Last week the WannaCrypt0r ransomware [blew through Europe and Asia](https://www.theregister.co.uk/2017/05/13/wannacrypt_ransomware_worm/), targeting unpatched and legacy Windows systems vulnerable to a code execution bug within its SMB filesharing protocol. By the time the initial round had been stopped, nearly 200,000 machines across 150 different countries had been affected, crippling the UK's National Health Service network of hospitals, as well as digital networks for transportation, energy, banking, and shipping industries worldwide.

Microsoft [issued a patch](https://technet.microsoft.com/en-us/library/security/ms17-010.aspx) for systems still in production in March, but WCry still inflicted major damage. A number of causes are at play here, but the two most serious are inconsistent update application and the continued use of legacy operating systems. Windows XP officially reached End of Life nearly three years ago, but **more than half of all businesses worldwide still run [at least one machine](http://www.techradar.com/news/more-than-half-of-businesses-still-rely-on-windows-xp) with Windows XP.** That's a staggering number of computers that are dangerously vulnerable to security flaws. Microsoft did issue a patch for XP systems, but it's not clear whether this will happen if another ransomware firestorm springs up. It certainly doesn't address any of the other XP vulnerabilities that are exploitable; WCry just happened to get a substantial amount of press due to its severity and global impact.

### How hard could it possibly be to just upgrade your systems? ###

It's usually right about here, where people are running around with their heads on fire, that cries of disbelief at still running XP swell to a roar. *It's dangerous and irresponsible! You're losing money and risking hacking, identity theft, and fraud! You must update your operating systems now! How could you be so short-sighted?!*

These are all reasonable concerns, and none of them are necessarily false. The vulnerabilities are known and the consequences are well-documented. Running software that's so drastically out of date is pretty dangerous, especially if its used to store any sensitive data or do any important work -- that's everything from email on up. It would be much safer to do everything on a modern operating system, like Windows 10 rather than XP.

Upgrading therefore makes sense to small businesses, organizations, and your mom and dad's home computer. They probably have basic computing needs centered on a few software tools, QuickBooks, email, and other relatively common applications. These are likely to also be compatible with more modern operating systems like Windows 10, since they're generally consumer-grade applications that see active development and relatively widespread use.

This is not necessarily the case for large organizations, particularly those that have niche software needs where custom tools meet demands and haven't seen active development in years. This problem gets even *worse* for systems and networks that have to function 24/7 or must deliver safety-critical functionality. If you take some time to think about it, systems that fit this description are integral to our modern existence: electric grids; power plants; public water and sanitation systems; transportation infrastructure; hospitals; the list goes on.

Imagine the world of hurt we'd be in if ransomware hit [Air Traffic Control networks](https://aviation.stackexchange.com/questions/22970/is-atc-connected-to-the-internet-why) as hard as WCry hit the NHS....

### Safety-critical systems often rely on tight coupling ###

Within systems engineering, there's something called the self-reinforcing complexity cycle. You can find a graphical depiction of it [here](https://www.researchgate.net/figure/265129686_fig2_Fig-2-Hollnagel-and-Woods-2005-self-reinforcing-complexity-cycle). The example included in that figure deals with control panels for oil rigs, but the same general notions hold for systems of all types: in order to address system deficiencies, we add components and capabilities to the system. This increases the complexity of the system. In order to monitor the system for faults, we ask human operators or autonomous monitors (often both) to supervise or take part in the monitoring task, increasing the complexity of that task. This increase in task complexity can lead to new problems, and therefore system deficiencies, which must be addressed.

You can see how this becomes a vicious cycle.

Within this cycle, the push to increase system reliability (addressing system deficiencies) can lead to _tight coupling_, or the integration of system components in such a way that their operation is affected by the operation of components around them. This has a number of benefits, like reducing latency when responding to adverse system events, helping to inform corrective actions, and reducing resource draw. These are desirable qualities of high performance systems that require safe operation, like nuclear reactors, rockets, and other complex systems.

However, this is a Catch-22: a single component failure in a tightly-coupled system can cascade through the system, leading to catastrophic system failure, destruction, and death. For notable examples of tight system coupling leading to catastrophic system failure, see Challenger, Three Mile Island, and Apollo 13. In each of these cases, unforeseen interactions between system components led to failures, which then cascaded through the systems to result in a catastrophic failure. On their own, these failures may not have been necessarily serious, but the tight coupling required in each of these systems to ensure safe performance meant that failures fell like dominoes through the system. For some extremely interesting reading on the matter, see Perrow's [Normal Accidents](https://www.amazon.com/Normal-Accidents-Living-High-Risk-Technologies/dp/0691004129).

### Cascading failures at the NHS ###

This relates directly to the UK's NHS and why they're still running Windows XP. This large, connected national hospital system needs to deliver critical patient care on demand at all hours of the day; if not, lives could be put in jeopardy. Digital systems are used to manage information for patient recordkeeping, procedure ordering, tracking, and service rendition, among other things; in fact, it was this information management system that the UK's Health Secretary [failed to update](http://www.businessinsider.com/jeremy-hunt-was-warned-of-urgent-need-to-update-nhs-cyber-security-2017-5), despite repeated warnings to do so.

I have a feeling that this large, interconnected system was not initially designed for modularity in the same way that modern server farms or well-planned IT infrastructure is today. It was probably designed to do its job, to deliver information as needed for patient care, rather than for the eventuality that large portions of it would need to basically be hot-swapped when major upgrades were needed. This requires a bit of forward thinking, some over-engineering, and some extra capital. These things are not politically expedient in any country, particularly for a national healthcare system [repeatedly](https://www.theguardian.com/society/2014/nov/05/nhs-wastes-over-2-bn-on-unnecessary-treatment) [accused](http://www.telegraph.co.uk/news/health/9150311/A-scandalous-waste-of-money-in-the-health-service.html) of wasteful spending practices and "bureaucratic incoherence."

So, as charges of financial waste and [unnecessary strain](http://www.bbc.com/news/uk-politics-eu-referendum-36058513) are leveled at the NHS by both the government and the public, [budgets are slashed](https://www.theguardian.com/society/2014/oct/05/nhs-finances-crisis-rising-demand-budget-cuts-30-billion-pound-deficit-2020). Compared to maintaining patient care, IT funding is put on the backburner. Upgrades are canceled, [components are abandoned](https://www.theguardian.com/society/2013/sep/18/nhs-records-system-10bn), and stopgap fixes are implemented to just keep the damn thing running. System complexity grows.

Then, in May 2017, a ransomware worm burrows its way through this massive, out-of-date IT system and begins encrypting the machines and infrastructure needed to deliver and maintain patient care. Failures at critical junctures begin to cascade through the system, and at the worm's height [nearly fifty hospitals](http://www.bbc.com/news/uk-39918426) are hit.

Things are getting back to normal, and maybe this event will serve as a wake-up call to those in positions of authority. Contingencies need to be made for future attacks, because we certainly haven't seen the last of events like this.

What's _not_ helpful is sarcastic outrage at the use of outdated operating systems, or a disbelief that anyone would let anything remain unpatched. Things are different with legacy systems in tightly-coupled, safety-critical systems. Even if the funding were available, risk-averse managers terrified of software incompatibility or an untested patch knocking out critical systems--even for a brief period of time--probably aren't enthusiastic about national-level software upgrades.

Rather than pointing out the series of blunders that led to this, we need to remind system designers to expect the best but plan for the worst. All of the hardware and software used in a system like this needs to have a legacy support plan in place, and upgrade capability needs to be baked into the design of the system. Yes, it's going to cost extra money up front and require some forethought, but we have to be ready to address foreseeable cyber threats. It's also probably less serious than a loss of life or vast sums of money when critical systems like these go down.

It's an uncomfortable truth, but it's probably going to get worse before it gets better.
