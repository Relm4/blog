---
title: "Relm4 is back! Announcing version 0.7 and 0.8"
date: 2024-03-12
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

I'm happy to announce Relm4 0.7 and 0.8!
It has been some time since I last posted here and I actually even skipped the announcement of Relm4 0.6 due to lack of time.
Fortunately, I found some time this week and am back with not one, but two releases!

Because Relm4 follows the version numbers of the underlying gtk-rs crate, we decided to catch up to their recent 0.8 release and postpone any feature development to version 0.9.
This means that version 0.7 and 0.8 of Relm4 are practically identical except for their dependencies.

Check out the [full changelog](https://github.com/Relm4/Relm4/blob/main/CHANGES.md) for more information.

> ## About Relm4
> 
> Relm4 is an idiomatic GUI library inspired by [Elm](https://elm-lang.org/) and based on [gtk4-rs](https://crates.io/crates/gtk4).
> 
> We believe that GUI development should be easy, productive and delightful.  
> The [gtk4-rs](https://crates.io/crates/gtk4) crate already provides everything you need to write modern, beautiful and cross-platform applications.
> Built on top of this foundation, Relm4 makes developing much more idiomatic, simpler and faster and enables you to become productive in just a few hours.

## Factory changes

Starting with Relm4 0.7, factories and components share the same builder pattern for initialization.
This change should especially benefit beginners as it allows reusing the patterns learned while working with components.
It also makes it easier to create reusable factories because the containing component can decide what happens with output messages of the factory and whether they need to be forwarded.

<table>
    <tr>
        <th> Component </th> <th> Factory </th>
    </tr>
    <tr>
        <td>

```rust
let component = Header::builder()
    .launch(init_data)
    .forward(sender, |msg| { ... });
```

</td>
<td>

```rust
let factory = FactoryVecDeque::builder()
    .launch(root_widget)
    .forward(sender, |msg| { ... });
```
</td>
</tr>
</table>

## More typed abstractions

While the gtk4 crate provides outstanding Rust abstractions overall, sometimes it doesn't fully embrace the potential of the Rust language.
Fortunately, to make your code more idiomatic, Relm4 adds several new typed abstractions on top of the existing ones in this release.
After `TypedListView` was added in version 0.6, members of the community have since extended the abstractions with the new `TypedColumnView` and `TypedGridView` types.
All three types allow you to use `gtk::ListView`, `gtk::ColumnView` and `gtk::TypedGridView` respectively without the otherwise necessary boilerplate code and also add compile-time guarantees regarding type-safety.

## Relm4 icons

Technically, the [relm4-icons crate](https://crates.io/crates/relm4-icons) has been out for quite some time, but it was never properly announced.
With this crate, it becomes incredibly easy to add icons to your Relm4 and gtk-rs applications.
You only need to add the crate, a small config file and call `initialize_icons()` in your setup code.
While the crate ships with over 3000 ready-to-use icons and also allows you to add your own SVG icons, 
only the configured icons will actually be bundled with your app to keep the resulting binaries small.

If you're already using Relm4 icons, please be aware that starting with this release, you need to use a config file instead of feature flags. 
This is because crates.io made the crate un-publishable when [they added new limits for the amount of feature flags](https://blog.rust-lang.org/2023/10/26/broken-badges-and-23k-keywords.html).
While I don't think it was a nice move to ban new releases of existing crates over night, it eventually led to a new, more flexible solution.
This new solution uses a `icons.toml` configuration file to specify which icons you need in your app.

While this works great, this is still a somewhat hacked together solution because, after disallowing the use of many feature flags, Rust has no proper solution for communicating a lot of configuration options to build scripts.
Hopefully, this gap will be filled with a new mechanism in the future.

## More unification

Besides factories, components also received a small, but quite noticeable change that brings more consistency.
Previously, regular components received a reference to the root widget during initialization, but async components used an owned root widget instead to avoid lifetime problems.
When refactoring a regular component to an async component, this caused complex error messages unless the function signatures were updated to use owned root widgets.
By always using owned root widgets, it now takes even fewer steps to refactor between regular and async components.

## Other improvements

- Use native async traits instead of async-trait in v0.8
- Add `AsyncComponentStream` to supports streams for async components
- Add `Toaster` as an abstraction over `adw::ToastOverlay` for usage in the model of a component
- Implement more traits for recent libadwaita widgets
- Port the book to 0.7 and 0.8
- Lots of other improvements and fixes

## Where to get started

+ ⬆️ **[Migration guide](https://relm4.org/book/stable/0_6_to_0_7.html)**
+ 🏠 **[Website](https://relm4.org)**
+ ⭐ **[Repository](https://github.com/Relm4/Relm4)**
+ 📖 **[Book](https://relm4.org/book/stable)**
+ 📜 **[Rust documentation](https://docs.rs/relm4)**
+ 📨 **[Chat room](https://matrix.to/#/#relm4:matrix.org)**