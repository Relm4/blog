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

One year ago, the first stable version of Relm4 was released.
Since then, it has been an incredible journey.
From a small proof-of-concept library, Relm4 has grown to be a community-driven project with many thousands of downloads.
Now it's time to celebrate, look back and see what will come in the future.

> ## About Relm4
>
> Relm4 is an idiomatic GUI library inspired by [Elm](https://elm-lang.org/) and based on [gtk4-rs](https://crates.io/crates/gtk4).
>
> We believe that GUI development should be easy, productive and delightful.
> The [gtk4-rs](https://crates.io/crates/gtk4) crate already provides everything you need to write modern, beautiful and cross-platform applications.
> Built on top of this foundation, Relm4 makes developing more idiomatic, simpler and faster and enables you to become productive in just a few hours.

## The history of Relm4

As part of a job at my university, I was tasked with rewriting an old Qt4 based C++ application.
The choice of language quickly fell on Rust, but choosing a GUI library turned out to be more difficult.
After some back and forth, I settled on relm, which was easy to use and backed by the full-featured GTK3 toolkit.

Yet at about the same time, gtk4-rs 0.1 was released as the first stable version of Rust bindings for GTK4.
Naturally, I wanted to switch to GTK4 for my application as soon as possible to avoid porting the application later, but relm kept me back at GTK3.

After experimenting with relm itself, I quickly realized that porting it to GTK4 would require a major redesign.
With seemingly nobody else willing to put in the effort, I started with a small prototype from scratch.
With just above 100 lines of code, I had a proof-of-concept implementation and started moving forward.
Some months and 9 beta releases later, I was finally able to announce the first stable version of Relm4.

## Relm4 today

Since its first stable release, Relm4 has grown significantly.
We rewrote and redesigned large parts multiple times to better fulfill our goals: productivity, simplicity and maintainability.
Version 0.5 which had its first beta release a few months ago is another big step forward.
It was in development for almost half a year, but makes Relm4 as simple, flexible and productive as never before.

Our last goal, outstanding documentation, suffered a bit from the long experimental development of 0.5.
Especially the book still isn't 100% up to date, but we're working on closing this gap.
And thanks to active feedback from the community, we are still polishing the API until 0.5 is released as stable.

Overall, I'm proud to say that Relm4 is now one of the best choices for native application development in Rust.
The solid application structure and its declarative UI definitions that seamlessly integrate with regular Rust code make Relm4 unique and delightful to work with.

## Quotes

> No other library I could find can give me both the tremendous power of GTK4 and a simple and ergonomic way of writing my applications, even when I look outside of the Rust ecosystem.
> 0.5 is a huge improvement over 0.4 both in terms of functionality and the quality and usefulness of the macros, which are a core part of the system.
>
> Another great part about Relm4 is the fact that I can easily ask questions in the official chat and I get great help.
> Most of the times it's simple things, like pointing me to a part of the documentation or a single paragraph, but such small things can often help avoid hours of debugging and googling.
> This friendliness makes me feel happy to use it too.
>
> *dexterdy*

---

> I was using relm before and Relm4 has taken the ease of use and expressiveness from there, while fixing most of the annoyances.
> I think the biggest improvement in 0.5 is async, it's become much easier to use now.
> Also, factories have gotten nicer to work with.
> Thanks for all the work and the great support from everyone!
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

## Credit

Without a doubt, GTK and Rust are a very successful combination.
The power of GTK and the robustness and guarantees of the Rust programming fit together incredibly well.
It's no surprise that the gtk-rs crates are still the most popular UI crates (according to stats from [lib.rs](lib.rs)).
Without the awesome work of the GTK and gtk-rs developers, Relm4 and many other projects would not have been possible.

Also, I want to thank all of our [contributors](https://github.com/Relm4/Relm4/graphs/contributors) and everyone else in our community.
Without contributions and feedback from the Relm4 community and our [team](https://github.com/orgs/Relm4/people), Relm4 would not be nearly as great as it is today.