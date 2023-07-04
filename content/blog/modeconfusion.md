---
title: "Mode confusion and the recent Boeing 737 MAX-8 crashes"
date: 2019-03-15
slug: 'mode-confusion-boeing-crashes'
tag: 'human factors'
---

A few quick notes on the role that mode confusion may have played in the recent crashes of two Boeing 737 MAX-8 aircraft.
<!--more-->

### Recent crashes and software changes

Recently two Boeing 737 MAX-8 airliners [crashed](https://www.nytimes.com/interactive/2019/03/13/world/boeing-737-crash-investigation.html) within a few months of each other, killing 357 people (a total loss of life aboard both craft). Preliminary results from their respective investigations revealed strong similarities between the two, and an early target for explanation is a newly-installed flight control system called the Maneuvering Characteristics Augmentation System, or [MCAS](https://theaircurrent.com/aviation-safety/what-is-the-boeing-737-max-maneuvering-characteristics-augmentation-system-mcas-jt610/). Because the MAX design required shifting engine placement and center of gravity around, the MCAS software change was made to help pilots maintain control during steep maneuvers and airspeeds approaching stall conditions by automatically adjusting the plane's trim settings. 

While well intentioned, Boeing [decided](https://www.seattletimes.com/business/boeing-aerospace/u-s-pilots-flying-737-max-werent-told-about-new-automatic-systems-change-linked-to-lion-air-crash/) that these software changes didn't represent a sufficiently significant change to system performance; therefore, Boeing didn't have to train pilots on this change. This lack of awareness of the change, coupled with the [counterintuitive way](https://leehamnews.com/2018/11/14/boeings-automatic-trim-for-the-737-max-was-not-disclosed-to-the-pilots/) in which this stall protection system can be disabled, has led to a situation ripe for human error through mode confusion.

### What is mode confusion? ###

Put simply, mode confusion occurs when a human operator fails to keep track of the system's current mode of operation. Human error can result when an operator attempts control actions that, while correct in a certain mode of system operation, are incorrect in the system's present operational mode. These incorrect actions can at best fail to work, but at worst can put the system in an unsafe operational state, potentially damaging the system or killing people. 

### Potential sources of mode confusion described by a 737 MAX pilot ###

In a recent article from AOPA, a pilot trade magazine, a pilot discussed MCAS operation and how it may have contributed to the Lion Air and Ethiopia Air crashes. You can find that article [here](https://www.aopa.org/news-and-media/all-news/2019/march/14/faa-grounds-boeing-737-max-fleet). I have extracted some particularly noteworthy passages, and highlighted some sections that caught my eye:

- "An airline pilot with 737 MAX-8 flying experience who wished to remain anonymous explained to AOPA that the new [MCAS] augmentation system affected the stabilizer trim but **noted several ways to defeat it."**

- "A system malfunction **'should appear to a pilot the same way a runaway trim wheel appears,'** the pilot continued. 'The result is that we have a runaway trim checklist—and a procedure” to work around it. 'You turn off the electric trim and go to a manual reversion. It’s something we train for. **It is true that Boeing didn’t tell anyone about it [MCAS]—so that is problematic.'"**

- "'In order to compensate for what the engineers perceived to be an issue with respect to pitch, they added this MCAS system that operates when the autopilot is off and the angle of attack exceeds certain limitations and when the airplane is banked pretty steeply.” **He said the technology runs the stabilizer pitch down for several seconds and it "reassesses and will start again until it believes the airplane has reached a safe angle of attack, and it operates without the pilots knowing [about it]."**

- "Tecce [an independent COMAIR expert, not the anonymous pilot] noted that in the case of the Lion Air Boeing 737 MAX crash, “now the airplane is pitching down and actually moving the control wheel will not stop that system. **If the pilot uses the trim system on the yoke, the [MCAS] system will stop" but "if the airplane isn’t in the proper attitude it will reactivate,” Tecce said, further forcing the aircraft downward if pilots fail to recognize the situation and take proper corrective action.'"**

So, to recap, Boeing installed a software patch to change how the airplane handles itself, with pilots neither trained nor even told about it before being installed. This system operates without their knowledge, can be disabled in several (unintuitive and unexplained, due to lack of training) ways, and will reactivate itself without input from the pilot. Furthermore, corrective action must be taken once the pilot recognizes the situation---a near impossibility, again due to the lack of training or awareness from the pilots flying these airplanes. The MCAS failure mode also looks just like a related, but totally different, type of system malfunction that is remediated through a completely different checklist-driven procedure.

While we should wait for the FAA's final determination for a definitive answer, this already looks quite a bit like poor system design and change communication practices are contributing factors to these crashes. Remember that [blaming human error is never a responsible solution](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC3115647/).