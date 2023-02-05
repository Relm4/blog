---
title: "A long journey: Announcing Relm4 v0.5!"
date: 2023-02-05
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
ShowPostNavLinks: truehttp://localhost:1313/blog/posts/announcing_relm4_v0.5_beta/#commands
editPost:
    URL: "https://github.com/Relm4/blog/blob/main/content"
    Text: "Suggest Changes" # edit text
    appendFilePath: true # to append file path to Edit link
---

After more than one year of development, Relm4 0.5 has finally made it across the finish line with plenty of new features and improvements!
Without a doubt, version 0.5 is by far our largest release so far making Relm4 even easier, more stable and productive than before.

In this post, we'll have a look at the most important new features added since the [first beta release](http://localhost:1313/blog/posts/announcing_relm4_v0.5_beta).
Check out the [full changelog](https://github.com/Relm4/Relm4/blob/main/CHANGES.md) for more information.

> ## About Relm4
> 
> Relm4 is an idiomatic GUI library inspired by [Elm](https://elm-lang.org/) and based on [gtk4-rs](https://crates.io/crates/gtk4).
> 
> We believe that GUI development should be easy, productive and delightful.  
> The [gtk4-rs](https://crates.io/crates/gtk4) crate already provides everything you need to write modern, beautiful and cross-platform applications.
> Built on top of this foundation, Relm4 makes developing more idiomatic, simpler and faster and enables you to become productive in just a few hours.

## Async everything

Since the first beta release, Relm4 has supported [commands](/blog/posts/announcing_relm4_v0.5_beta/#commands) as a concept from Elm that made using asynchronous code easy to integrate.
In this release, we have added asynchronous components and asynchronous factories, which makes it possible to use async code almost everywhere in Relm4.
Especially, late initialization of components that have to wait on asynchronous tasks, e.g. to perform web request, has become very easy to implement.

```rust
// Placeholder until `init_model` completes
fn init_loading_widgets(root: &mut Self::Root) -> Option<LoadingWidgets> {
    view! {
        #[local_ref]
        root {
            set_orientation: gtk::Orientation::Horizontal,
            set_spacing: 10,

            #[name(spinner)]
            gtk::Spinner {
                set_spinning: true,
                set_hexpand: true,
                set_halign: gtk::Align::Center,
                // Reserve vertical space
                set_height_request: 34,
            }
        }
    }
    Some(LoadingWidgets::new(root, spinner))
}

// ...

// Async intializaion
async fn init_model(
    value: Self::Init,
    _index: &DynamicIndex,
    _sender: AsyncFactorySender<Self>,
) -> Self {
    tokio::time::sleep(Duration::from_secs(1)).await;
    Self { value }
}
```

<video controls style="width: 100%;">
    <source src="./async_factory.webm" type="video/webm">
    Your browser does not support the video tag.
</video> 

Additionally, components can be now converted into an asynchronous [`Stream`](https://docs.rs/futures/latest/futures/stream/trait.Stream.html) of messages.

## Widget templates

Often, similar widgets and properties are used multiple times in the same application.
With widget templates, you can define common UI elements as templates and reuse them across your code.

For example, defining a `gtk::Box` with a margin and custom CSS as a template looks like this:

```rust
#[relm4::widget_template]
impl WidgetTemplate for MyBox {
    view! {
        gtk::Box {
            set_margin_all: 10,
            inline_css: "border: 2px solid blue",
        }
    }
}
```

To use the template, you just need to use the `#[template]` attribute in the macro.

```rust
#[relm4::component]
impl SimpleComponent for AppModel {
    type Init = u8;
    type Input = AppMsg;
    type Output = ();

    view! {
        gtk::Window {
            set_title: Some("Widget template"),

            #[template]
            MyBox {
                gtk::Label {
                    #[watch]
                    set_label: &format!("Counter: {}", model.counter),
                },
            },
        }
    }
    
    // ...
}
```

More details can be found in [this PR](https://github.com/Relm4/Relm4/pull/310).

## Macro improvements

Sending messages for simple signal handlers has become easier.
Just like it has been possible in the [relm crate](https://github.com/antoyo/relm#widget-attribute) you can now send input messages with an arrow followed by the message.

```rust
gtk::Button {
    set_label: "Increment",
    connect_clicked => AppMsg::Increment,
}
```

## Less boilerplate

Recently, we published [Relm4 snippets](https://marketplace.visualstudio.com/items?itemName=Relm4.relm4-snippets) as an IDE extension that provides helpful snippets for implementing Relm4's traits.

## Other improvements

- Porting relm4-components to 0.5
- Porting the book to 0.5
- Moving from custom documentation builds to [docs.rs](https://docs.rs/relm4/)
- Adding `MessageBroker` to allow communication between components at different levels
- Add `Reducer` as message based alternative to `SharedState`
- Lots of other improvements and fixes


## On the horizon

Some tools haven't made it into this stable release but will follow soon.

+ [relm4-format](https://github.com/Relm4/Relm4/pull/385) is a tool that can format code inside the `view` macro to address the shortcomings of `rustfmt` regarding macros.
+ relm4-cli
