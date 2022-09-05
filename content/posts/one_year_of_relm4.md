---
title: "One year of Relm4"
date: 2022-09-05
# weight: 1
tags: ["relm4"]
author: "Aaron Erhardt"
showToc: true
TocOpen: false
draft: false
hidemeta: false
comments: false
disableShare: false
disableHLJS: false
hideSummary: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
editPost:
    URL: "https://github.com/Relm4/blog/blob/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

One year ago the first stable version of Relm4 was released.
Since then, it has been an incredible journey.
From a small proof-of-concept library, Relm4 has grown to be a community-driven project with many thousands of downloads.
Now it's time to celebrate, look back and see what will come in the future.

## The history of Relm4

As part of a job at my university, I was tasked with rewriting an old Qt4 based C++ application.
The choice of language quickly fell on Rust, but choosing a GUI library turned out a bit more difficult.
After some back and forth, I settled on relm, which was easy to use and backed by the full-featured GTK3 toolkit.

Yet at about the same time, gtk4-rs 0.1 was released as the first stable version of Rust bindings for GTK4.
Naturally, I wanted to switch to GTK4 for my application as soon as possible to avoid porting the application later, but relm kept me back at GTK3.

After experimenting with relm itself, I quickly realized that porting it to GTK4 would require a major redesign.
With seemingly nobody else willing to put in the effort, I started with a small prototype of a new relm version from scratch.
With just above 100 lines of code, I had a proof-of-concept implementation and started moving forward.
Some months and 9 beta releases later, I was finally able to announce the first stable version of Relm4.

# Relm4 today

TODO

## Our goals

Anniversaries are always a good opportunity to look back and see how much progress has been made.
To measure our progress I'll have a look at Relm4's goals: Productivity, simplicity, outstanding documentation and maintainability.

TODO

## Quotes

> No other library I could find can give me both the tremendous power of GTK4 as well as a simple and ergonomic way of writing my applications, even when I look outside of the Rust ecosystem.
> 0.5 is a huge improvement over 0.4 both in terms of functionality as well as the quality and usefulness of the macros, which are a core part of the system.
>
> Another great part about Relm4 is the fact that I can easily ask questions here in this chat and I get great help.
> Most of the times it's simple things, like pointing me to a part of the documentation or a single paragraph, but such small things can often help avoid hours of debugging and googling.
> This friendliness makes me feel happy to use it too.
>
> *dexterdy*

---

> I was using relm before and Relm4 has taken the ease of use and expressiveness from there, while fixing most of the annoyances.
> I think the biggest improvement in 0.5 is async, it's become much easier to use now.
> Also, factories have gotten nicer to work with.
> Thanks for all the work, and the great support from everyone here.
>
> *Eric Trombly*

---

> The official book helped a lot, definitely delivering on the outstanding documentation goal.
> I liked the development experience.
> My biggest surprise was how well it blends with GTK ecosystem: it eliminates the burden of managing GTK objects manually, but it doesn't try to hide GTK API from you.
> You still use GTK and Libadwaita types directly, GTK documentation applies directly, you still learn GTK by using Relm4 if that's your goal.
> I think this explicitly defined limited scope is a great design decision.
>
> *azymohliad*

# Credit

With a doubt, GTK and Rust are a very successful combination.
The power of GTK and the robustness and guarantees of the Rust programming fit together increadibly well.
It's no surprise that the gtk-rs crates are still the most most popular UI crates (according to stats from [lib.rs](lib.rs)).
In fact, quite a few apps I use daily are already written with GTK and Rust.

Relm4 would not have been possible without TODO