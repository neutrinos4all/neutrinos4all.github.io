---
title: "Making a CV with LaTeX"
date: 2016-04-05
slug: 'latexvita'
tag: 'tutorials'
---

For those of you who would like to create a better-looking curriculum vitae for prospective employers, consider using moderncv. This tutorial will help you get started.
<!--more-->

I have to admit, one of the principal reasons for building the site is to have a personalized space on the [internet](http://www.theverge.com/2016/4/2/11352744/ap-style-guide-will-no-longer-capitalize-internet) that people (and potential employers) can use to get a sense of me as a potential employee. I can display some of my projects, talk about the work I do, and get some info about my interests. I used LinkedIn for a long time, but sometime ~2015 I realized it was becoming less useful for actual employer networking or job-searching. I recently decided I didn't want to use their service any more, made this site, and took down my LinkedIn profile. Maybe sometime I'll write more in-depth about it, perhaps once I've been out of the service for a while.

Anyway, that meant I'd need a proper [curriculum vitae](https://www.ncfr.org/professional-resources/career-resources/general-career-resources/your-cv-it-vita-or-vitae), or at least some written and preferably downloadable method for communicating details about my education, work experience, and qualifications. I had made a vita before, but I used Word and it showed. I knew a bit of LaTeX and I wanted to demonstrate what skills I had, plus maybe continue to work on them a bit, so I decided to make the entire CV with it.

I knew that I wanted to use a pre-canned template, vowing that some day I would write my own, eventually landing on [moderncv](https://www.ctan.org/pkg/moderncv?lang=en). It looked relatively good for being a standardized template and seemed easy to use. Once you download it (manually, through either your TeX editor or, if you're using Linux, through Synaptic), call the template at the very top of your .tex document:

    \documentclass[11pt,a4paper]{moderncv}

...and you're off and running.

This tutorial won't focus on using the package, because you can currently find some [helpful](https://walrustech.wordpress.com/tag/template/) [content](https://fredrikloch.me/post/using-moderncv/) out there on its use--not to mention the actual [template,](https://bazaar.launchpad.net/~xdanaux/moderncv/trunk/view/head:/examples/template.tex) which includes an extensive system of very straightforward comments. I don't want to re-hash their work. What I will spend a bit on, though, are the little improvements I made on it.

### Typesetting ###

I didn't like how the standard moderncv package rendered text, particularly because I didn't like how it handled kerning on larger-point fonts. I opted to use [EB Garamond](https://www.google.com/fonts/specimen/EB+Garamond#charset), an open-source incarnation of the Garamond typeface family. Installed through the `texlive-fonts-extra` Linux package, I enabled it with:

    \usepackage{ebgaramond}

I also made use of small capital letters throughout my vita, particularly for section and subsection headings, becuase I thought it added a nice touch and slightly improved readability for text-heavy sections. It can be safely used throughout the template by calling `\textsc{your text here}` anywhere you'd like to use it.

### Relevant coursework bulleted items ###

After I listed out all of my coursework with `\cvline` entries and rendered it, I decided it left too much negative space on the page. Putting my courses in a bulleted format would be a more efficient approach. The code I used for that section can be seen here:

    \section{\textsc{Relevant Coursework}}
    \hspace{6.4em}
    \begin{tabular}{p{.4\textwidth}p{.5\textwidth}}
	    $\cdot$ Advanced Formal Methods Techniques & $\cdot$ Human Factors Research Methodology\\
	    $\cdot$ Cognitive Engineering & $\cdot$ Human Information Processing\\
	    $\cdot$ Cognitive Processes & $\cdot$ Industrial Hygiene\\
	    $\cdot$ Design and Analysis of Experiments & $\cdot$ Java Programming I\\
	    $\cdot$ Formal Methods in Human Factors & $\cdot$ Ontology Engineering\\
	    $\cdot$ Home Health Innovations & $\cdot$ Sensing Realities\\
	    $\cdot$ Human-Computer Interaction & $\cdot$ Safety in Human Factors\\
    \end{tabular}

It may take some custom tinkering on your part, but I'll provide a bit of explanation for the included commands:

* `\hspace{}` adds horizontal white space, depending on the argument you give it inside the curly braces. The 'em' unit label signifies an amount of horizontal space equivalent to an uppercase letter "M" in the font being currently used. As someone else told me, it's sometimes easier to visualize how large those differences are as opposed to points or millimeters.
* `{tabular}{p{.4\textwidth}p{.5\textwidth}}` sets the two columns in my table, as well as the portion of the page that each column will occupy given in a decimal fraction of the total "text-occupiable space" width. Here, the first column will take 40% of the width and the second will take 50%. If you left it at .5 and .5, you'd notice that the righthand column was pushed a little too far over, a residual effect from modifying the `\hspace{}` parameter earlier. Reducing the size of the lefthand column will pull the righthand column over, the amount of which can be finely controlled by modifying the decimal value passed in.
* `$\cdot$` is just the LaTeX math symbol code used to render the bullets in my list. There are many ways to do it, but if you wanted to stick with a mathematical symbol check [this](http://web.ift.uib.no/Teori/KURS/WRK/TeX/symALL.html) page for a quick reference.

### Tasteful colors and formatting ###

The rest is up to you, your particular tastes, and what you think would make for a professional-looking vita. Using a bit of color in sparing fashion can add a bit of style to your document, so long as it isn't too loud. LaTeX allows for an endless variety of customization and tweaking, so don't shy away from using its capabilities to your advantage.

As always, if you find mistakes or have questions, please send an email.
