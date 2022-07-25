---
title: "Off to new adventures - Announcing Relm4 v0.5 beta!"
date: 2022-07-25
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
    URL: "https://github.com/AaronErhardt/AaronErhardt.github.io/blob/master/blog-src/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

A new realm appears on the horizon, waiting for you with plenty of new features to discover!
I'm exited to announce Relm4 v0.5, by far our biggest release so far!

> ## About Relm4
> 
> Relm4 is an idiomatic GUI library inspired by [Elm](https://elm-lang.org/) and based on [gtk4-rs](https://crates.io/crates/gtk4).
> 
> We believe that GUI development should be easy, productive and delightful.  
> The [gtk4-rs](https://crates.io/crates/gtk4) crate already provides everything you need to write modern, beautiful and cross-platform applications.
> Built on top of this foundation, Relm4 makes developing more idiomatic, simpler and faster and enables you to become productive in just a few hours.

# What's new?

Relm4 v0.5 features some fundamental redesigns that simplify the API, solve longstanding issues and enable several new features.
The work was started more than half a year ago by [Michael Murphy](https://github.com/mmstick) from [System76](https://system76.com) who had some new ideas to improve the structure of Relm4 applications.
Over time, more contributors gathered together and almost every single part of Relm4 saw some adjustments while some parts were refactored several times.

## Fewer traits, more fun!

So far, Relm4 had many different traits for different use-cases.
In total 7 traits have now been unified in one simple interface, the `Component` trait.
Not only does this trait cover all different use-cases, it even adds some features on top.

### Maximum flexibility

In the last release announcement I mentioned that Relm4 had become more flexible when interacting with regular gtk-rs based code.
I'm happy this trend continues in 0.5.
You can now mix Relm4 into any gtk-rs application and the other way around with hardly any limitations.

### Commands

Commands are a concept in [Elm](https://elm-lang.org) that is often used to run web-requests in the background.
Now the same works in Relm4 too.
You can take full advantage of Rust's async ecosystem to run asynchronous tasks in the background without blocking your application.

```rust
// Send a new command with a delay of 1s
sender.command(|out, shutdown| {
    // Cancel the future if the component is dropped in the meantime
    shutdown.register(async move {
        tokio::time::sleep(Duration::from_secs(1)).await;
        out.send(AppMsg::Increment);
    }).drop_on_shutdown()
})
```

### Seamless communication

Many applications written in Relm4 rely upon communication between components.
Often one message needs to be forwarded to another component, for example to inform the main window that a dialog was closed.
Previously, this required forwarding the message from the application logic.
Now you can simply register a closure to modify and forward messages.

```rust
let component = MyComponent::builder()
    // Start the component service with an initial parameter
    .launch("Hello world")
    // Attach the returned receiver's messages to this closure.
    .connect_receiver(move |sender, message| match message {
        // Transform and forward the message
        Output::HelloThere => {
            sender.send(Input::Answer("General Kenobi!"))
        },
        _ => (),
    });
```

## Macroscopic macro improvements

The `view!` macro has been largely rewritten, making the codebase smaller, faster and more maintainable.
Additionally, the macro gained a lot of exiting features and is now able to recover from most invalid expressions so that the as much valid code as possible is generated.

### Rusty blueprints

With 0.5, the macro syntax has been cleaned up and become even easier.
By pure coincidence, it looks very similar to the [Blueprint language](https://jwestman.pages.gitlab.gnome.org/blueprint-compiler/) that's designed for GTK4 UIs specifically.

Yet, Relm4 has more to offer than just creating UIs.
It integrates well with the surrounding Rust code and allows you to update values automatically, use local variables and connect message handlers directly with the UI declaration.
You can even use `match` and `if` conditions to show widgets depending upon certain conditions.


```rust
// Create a new container
gtk::Box {
    // Arrange widgets vertically
    set_orientation: Vertical,

    // Watch `counter.value` to make the box invisible
    // if the value reaches 42
    #[watch]
    set_visible: counter.value != 42,

    // Append a new label
    gtk::Label {
        set_label: "Hello world!",
    },

    // Append a new label using a builder pattern
    gtk::Label::builder()
        .label("Builder pattern works!")
        .selectable(true)
        .build(),

    // Use different widgets depending on match statement
    // and show a transition in between
    #[transition(SlideLeft)]
    match counter.value {
        (0..=2) => {
            // First match arm: show button
            gtk::Button {
                set_label: "Value is smaller than 3, click to increment it!",
                connect_clicked[sender] => move |_| {
                    sender.input(appmsg::increment);
                }
            }
        },
        _ => {
            // Second match arm: show label
            gtk::Label {
                set_label: "Value is higher than 2",
            }
        }
    }
}
```

Further features added in this release include:

+ Support for multiple top-level widgets
+ Support adding widgets from local variables
+ Automatically block signals while updating values

## Factory improvements

Along with components, factories saw a big update.
The new `FactoryComponent` trait is very similar to `Component` and allows every entry of a factory to manage it's own state
At the same time, each element is still as easy to access as an element of an vector.
Also, a much more efficient and maintainable algorithm was added to calculate efficient updates.

## Try it out!

```toml
relm4 = { git = "https://github.com/Relm4/Relm4", tag = "0.5.0-beta1" }
```

## Remaining tasks

We hope that this release can be considered stable soon.
However, there are still a few things left to do.
Feedback and contributions are highly appreciated!

+ Polish some rough edges in the API
+ Finish updating the book
+ Port more examples to 0.5
+ Port the rest of relm4-components to 0.5

> Note: As of writing this, the book isn't fully ported to v0.5, but the examples in the repository are always up to date.

# Where to get started

+ ⬆️  **[Migration guide](https://relm4.org/book/next/0_4_to_0_5.html)**
+ ⭐ **[Repository](https://github.com/Relm4/Relm4)**
+ 📖 **[Book](https://relm4.org/book/stable)**
+ 📜 **[Rust documentation](https://relm4.org/docs/relm4/relm4/)**


# Special thanks

I highly appreciate feedback and contributions to Relm4 and thank those who helped me with this release:

+ [Michael Murphy](https://github.com/mmstick) for coming up with several brilliant ideas and contributing most of the component rework.
+ [Maksym Shcherbak](https://github.com/cofee-on-the-desk) for his ongoing and outstanding contributions around factories and other parts of Relm4.
+ [Andy Russell](https://github.com/euclio) for contributing improvements all across Relm4.
+ [Eduardo Flores](https://github.com/edfloreshz) for joining the discussions and porting the book to 0.5.
+ Everyone else who contributed, gave feedback in the Matrix room or on GitHub.
+ The whole gtk-rs team for providing awesome Rust bindings for GTK and always being helpful.
