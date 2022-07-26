---
title: "Off to new adventures - Announcing Relm4 v0.5 beta!"
date: 2022-07-26
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

A new realm appears on the horizon, waiting for you with plenty of new features to discover!
I'm exited to announce Relm4 v0.5, by far our biggest release so far.

> ## About Relm4
> 
> Relm4 is an idiomatic GUI library inspired by [Elm](https://elm-lang.org/) and based on [gtk4-rs](https://crates.io/crates/gtk4).
> 
> We believe that GUI development should be easy, productive and delightful.  
> The [gtk4-rs](https://crates.io/crates/gtk4) crate already provides everything you need to write modern, beautiful and cross-platform applications.
> Built on top of this foundation, Relm4 makes developing more idiomatic, simpler and faster and enables you to become productive in just a few hours.

# What's new?

Relm4 v0.5 features some fundamental changes that greatly simplify the API and solve some longstanding issues.
More than half a year ago, [Michael Murphy](https://github.com/mmstick) from [System76](https://system76.com) started off the development of 0.5 with some great ideas to improve the structure of Relm4 applications.
Over time, more developers joined the effort to redesign and refine Relm4's API.
Now - almost 300 commits later - I'm proud to present the results of our hard work.

## Fewer traits, more fun!

So far, Relm4 had several traits for different use-cases.
In total, 7 traits were unified into one interface, the `Component` trait.
Furthermore, the new trait covers not only all previous use-cases, it even adds some features on top.

### Maximum flexibility

In version 0.4, Relm4's macros had become more attractive for gtk4-rs based applications.
I'm happy this trend continues in version 0.5.
You can now mix Relm4 into any gtk-rs application and the other way around with hardly any limitations.

### Commands

Commands are a concept in [Elm](https://elm-lang.org) that is often used to run web-requests in the background without blocking other UI updates.
With Relm4, you now can take full advantage of Rust's async ecosystem to run asynchronous tasks in the background without blocking your application.

```rust
// Add a new command future to be executed in the backgroud
sender.command(|out, shutdown| {
    // Cancel the future if the component is shut down in the meantime
    shutdown.register(async move {
        // Wait for one second
        tokio::time::sleep(Duration::from_secs(1)).await;
        // Emit a delayed increment event
        out.send(AppMsg::Increment);
    }).drop_on_shutdown()
})
```

### Seamless communication

Many applications written in Relm4 rely upon communication between components.
Often one message needs to be forwarded to another component, e.g. to show a dialog.
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
Additionally, the macro gained a lot of exiting features and can continue the code generation even if it encounters errors.

### Rusty blueprints

With 0.5, the macro syntax was cleaned up and became even easier.
By pure coincidence, it looks very similar to the [Blueprint language](https://jwestman.pages.gitlab.gnome.org/blueprint-compiler/) that's specifically designed for GTK4 UIs.

Yet, Relm4 has more to offer than just creating UIs.
It integrates well into the surrounding Rust code, allows you to update values automatically and to use local variables and connect message handlers directly with the UI declaration.
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

    // Use different widgets depending on a pattern
    // and show a transition in between
    #[transition(SlideLeft)]
    match counter.value {
        (0..=2) => {
            // First match arm: show a button
            gtk::Button {
                set_label: "Value is smaller than 3, click to increment it!",
                connect_clicked[sender] => move |_| {
                    sender.input(appmsg::increment);
                }
            }
        },
        _ => {
            // Second match arm: show a label
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

## Fabulous factories

Along with components, factories saw a big update.
The new `FactoryComponent` trait is very similar to `Component` and allows every entry of a factory to manage its own state.

The updated `FactoryVecDeque` type comes extremely close to the goal of making collections of UI elements just as accessible as regular data collections.
With implementations for `Index` and `IndexMut` as well as most methods from `VecDeque`, you'll hardly notice you're actually working with UI elements.
Even external updates, e.g. when a user swaps two tabs, are now automatically transferred to the order of elements in the `FactoryVecDeque`.
A new RAII-guard completes the list of improvements and makes it impossible to forget to render the changes after modifying the data.

```rust
let mut guard = factory.guard();
guard.push_back(1);
guard.swap(1, 2);
let last_value = guard.pop_back().unwrap();
println!("Value of the first element {}", guard[0]);
// Changes are rendered as soon as the scope ends
```

Let's see the new factories in action!
The new [`tab_game`](https://github.com/Relm4/Relm4/blob/next/examples/libadwaita/tab_game.rs) example takes advantage of the new features to build a small game.

<video controls>
    <source src="./tab_game.webm" type="video/webm">
    Your browser does not support the video tag.
</video> 

## Try it out!

Try out the new beta by adding the following lines to your project's Cargo.toml file:

```toml
relm4 = { git = "https://github.com/Relm4/Relm4", tag = "0.5.0-beta1" }
```

Yet, keep in mind that this is still a beta, so some things might be slightly adjusted before the stable release.
Also, the book isn't fully ported to v0.5, but the examples in the repository are always up to date.

## Remaining tasks

We're looking forward to release 0.5 as stable soon.
However, there are still a few things left to do.
**Feedback and contributions are highly appreciated!**

+ Polish some rough edges in the API
+ Finish updating the book
+ Port more examples to 0.5
+ Port the rest of relm4-components to 0.5

# Where to get started

+ ⬆️ **[Migration guide](https://relm4.org/book/next/0_4_to_0_5.html)**
+ 🏠 **[Website](https://relm4.org)**
+ ⭐ **[Repository](https://github.com/Relm4/Relm4)**
+ 📖 **[Book](https://relm4.org/book/next)**
+ 📜 **[Rust documentation](https://relm4.org/docs/relm4/relm4/)**


# Special thanks

We highly appreciate feedback and contributions to Relm4.
In the name of all members of the Relm4 organization I thank those who helped with this release:

+ [Michael Murphy](https://github.com/mmstick) for coming up with several brilliant ideas and contributing most of the component rework.
+ [Maksym Shcherbak](https://github.com/cofee-on-the-desk) for his ongoing and outstanding contributions around factories and other parts of Relm4.
+ [Andy Russell](https://github.com/euclio) for contributing improvements all across Relm4.
+ [Eduardo Flores](https://github.com/edfloreshz) for joining the discussions and porting the book to 0.5.
+ Everyone else who contributed or gave feedback.
+ The whole gtk-rs team for providing awesome Rust bindings for GTK and always being helpful.

