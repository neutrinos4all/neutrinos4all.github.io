---
title: "Computational exploration of mental models using model checking"
date: 2017-08-07
slug: 'dissertation-topic-mental-models'
tag: human factors, dissertation, cybersecurity
---

For my dissertation, I'm arguing that we should be able to formally model and analyze human mental models to find unanticipated problems in computer security. But what does that mean and why does it matter?
<!--more-->

Imagine that you're a new employee at some company and you get lots of emails a day. Some are important, but most are threads you've been CC'ed on to help you spin up on current projects, track outstanding issues, and see what others are doing. The IT department set up your email client with the read pane disabled; that is, there was no permanently present space that would show you an email as soon as you clicked on it. Rather, you had to double-click the email if you wanted to read it, and it would open the email in a new window.

_What a pain. It takes so long just to get through this daily barrage, and it's exhausting. If only I could do it faster..._

It would be super useful if you could just enable the read pane, then 'down arrow' quickly through your inbox. Select an email, open it, visually scan it, and move on. No more double-click, read, click to close, double-click, read ... what a timesaver! It's such a small change, so you just go ahead and enable the read pane in your email client settings. Instant productivity improvement.

_This is awesome._

After a few weeks, someone from the IT department brings you in for a sit-down. Several machines at the company had recently been hit by a malware attack, and its point of origin was your computer. As the IT person asks you about this incident, we can imagine that it might go like this:

IT: "Do you remember getting any suspicious emails within the past few days?"  
You: "Hmm, maybe. I mean I get a lot of emails, and I can't remember everything I click on..."  
IT: "You mean you click on things without reading what it is?"  
You: "Well sometimes a lot of them are crap, so I just blow through them using the down arrow. And I always make sure I don't click on anything that looks dangerous, like we were trained to do when I got here."  
IT: "Wait a minute, down arrow? What down arrow? How do you read your email?"  
You: "With the read pane. I set it up so that I can see all my emails without having to double-click on each one of them..."  
IT: "You _what?_ We set it up to be disabled for a reason! Why would you do that?"  
You: "I don't know, because I wanted to save time and be more productive! I thought I was doing a good thing! Nobody said anything about that in training, and if I didn't click on something dangerous I don't understand how I got hacked."  
IT: "Look, Ms. Smith, we do things the way we do them for a reason. Don't change them. And you can download malware just by opening a bad email, you don't have to click on anything. Unfortunately, I'm going to have to tell my boss about this."  
You: "Oh my God, am I in trouble? I'm so sorry about this. I just started here, I'm a new employee. I didn't know. Nobody would even want to hack me anyway, I don't know anything..."  

### System failure: Cascades of smaller, seemingly innocuous events ###
In my [last post](https://appliedcaffeine.org/wcry-and-legacy-software.html), I argued that most complex systems failures can often be attributed to a cascade of system events that, on their own, probably seem innocuous or minor. Charles Perrow's [Normal Accidents](https://www.amazon.com/Normal-Accidents-Living-High-Risk-Technologies/dp/0691004129) and James Reason's [swiss cheese model](https://en.wikipedia.org/wiki/Swiss_cheese_model) of accident causation are pretty helpful here. This same thinking can be applied here to our toy scenario. Looking through it again, consider the potential problems that aligned and resulted in malware infection:

* New employee cybersecurity training only addressed phishing and attacks dependent on user clicks, rather than more passive attack strategies;  
* New employee was inundated with emails, leading to overload and mental fatigue conditions;  
* Email client default settings were not explained to the new employee;  
* IT team allowed a potentially dangerous setting to remain user-configurable.  

There are probably others, but the general takeaway is that a normally-skilled user (a lifelong, prudent computer user who is neither cybersecurity expert nor spring chicken) was facing pressure to get up to speed on work practices through a large volume of email. In the interest of making a good impression in her new job, the user realized a workflow bottleneck and implemented a workaround to reduce her fatigue. However, unbeknownst to her (consequential from her incomplete training), the workaround actually made the system less secure by making it easy for automatically-executing malware to run on the system. The system was initially configured in a safer mode, but she had no idea that was a deliberate decision because nobody told her so; furthermore, she could change the setting herself, so she thought it wasn't that important to begin with.

Each of those problems--security training that focuses only on "click to pwn" phishing attacks and malware, invisible defaults, insufficient granularity on user-configurable software controls--might be easily-explainable annoyances on their own. "Yeah, we've been meaning to rework the security training, we just haven't gotten around to it yet," or "We're focused on the big cybersecurity risks, we don't have time to micromanage user settings!" Problems also arise when we consider her mental model of how the email client worked to keep her safe from malware. Because she wasn't trained and wasn't thinking about passive malware attacks, she didn't recognize the vulnerability introduced by enabling the read pane and blasting through a bunch of emails without being careful about what she was opening. In a way, it was improperly calibrated to focus specifically on things like malicious Word documents and fake Gmail login pages.

When these problems align in a sort of perfect storm of failure, the consequences can be catastrophic. It's just so hard for humans to predict, because it's really hard to see how these different pieces can fit together and potentially lead to a massive hack before it happens.

That's what this dissertation is about. I hunt [black swans](https://en.wikipedia.org/wiki/Black_swan_theory), exploring how user mental models interacting with the world can lead to computer security problems. I do it by leveraging techniques from formal methods to find and describe those one-in-a-million events.

### Using formal methods to explore human mental models ###
Formal methods are well-defined mathematical languages and techniques for the modeling, specification, and verification of systems. System models are descriptions that leverage well-supported theoretical formalisms to represent a system’s behavior. Specifications are desirable system behaviors captured as propositions expressed in a mathematically precise, unambiguous manner. Upon describing the system model and specifications, verification mathematically proves (often using exhaustive searches) whether specifications do or do not hold in the system model. A specification is proved if it is satisfied by the model, while specification violations can return a counterexample. This is an execution trace capturing the model state in which the violation occurred and a list of incremental model states leading to the violation.

_Note: That paragraph was shamelessly ripped from the paper I wrote for [SOUPS 2017](https://www.usenix.org/conference/soups2017). The rest of the paper can be found [here](https://appliedcaffeine.org/docs/soups_paper.pdf)._

We can use formal methods to analyze mental models by formally representing both the mental models and basic aspects of the target computer program with mathematical logic, create specifications (in my case, using linear temporal logic) that describe desirable properties of the interaction between the mental models and the program, then use an automated model checker to explore the entire statespace of the model/program interaction and search for specification violations. This statespace is combinatoric, which means that _every possible interaction_ is enumerated, and that will include those catastrophic one-in-a-million combinations of events. The tricky part is finding them in the millions of interactions expressed in the statespace. Exhaustively searching for counterexamples is one way to do it.

As mentioned before, counterexamples are produced when we get specification violations, and certain programs can include an execution trace with the counterexample. Sort of like [stack traces](https://en.wikipedia.org/wiki/Stack_trace), execution traces give us an exact account of the steps that occurred leading to the violation all the way back to Step0. This can help us find, diagnose, and fix the problems that led to the violation. Because this dissertation focuses on mental models, this could suggest improvements to training regimens to help users more fully develop their mental models. It's also conceivable that we could deduce software changes (such as denying users the ability to turn on their read pane) that could help as well.

That's the coolest thing about this project: we could find all sorts of lurking problems that we wouldn't have known about until they caused an error or problem, as well as potential remedies for these problems tailored right to the issue itself. What's more, we're extending a pretty rigorous analytic method to the traditionally "squishy" realm of human psychology, performance, and systems interaction. There aren't many that do this sort of work, so I'm extremely excited to be one of them.

### What comes next? ###
This entry is just the rough outline of my topic. It doesn't really describe the methods in detail at all, and no room is devoted to past literature and successful application of these methods to squishy humans. We also fail to discuss the limitations and challenges inherent to the use of formal methods for any analytic purpose. Future blog entries will address these issues, as well as a bunch of stuff I baked into the user/IT employee exchange: user shaming, the 'big fish' folk model of hacking, and some other items.

There's so much neat stuff coming up, so stay tuned!
