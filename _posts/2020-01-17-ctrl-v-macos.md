---
layout: post
title: "Ctrl+v in MacOS"
date: "2020-01-17"
categories: macos
---

*TL;DR:* Use [Karabiner-Elements](https://pqrs.org/osx/karabiner/) if you want to reliably paste using `^v`

## Background
Recently, I mapped `^v` to paste on my MacBook. This worked well with one exception: In some cases, `^v` would act as Page Down. It took some time to isolate the pattern but what I noticed was that -- for example -- if while editing text and I placed my cursor someplace that wasn’t the end of the text field (e.g. while editing a long message on Slack), `^v` would page down. If, however, I placed my cursor at the end of the same text field, `^v` would paste as expected. Due to this minor annoyance disrupting my workflow, I searched for an explanation.

## A Solution
This unexpected `^v` behavior proved difficult to research. Numerous web searches turned up very little aside from [a StackExchange question](https://apple.stackexchange.com/questions/156211/using-ctrl-v-in-mail-app-on-mac-os-x/) from another user facing the same issue. Fortunately, [an answer posted there](https://apple.stackexchange.com/a/174491) led me to the excellent [Karabiner-Elements](https://pqrs.org/osx/karabiner/), which allows for all kinds of keyboard customizations. It was through this application that I was able to map `^v` to paste and disable the Page Down behavior: 
![]({{ site.url }}/assets/karabiner-prefs.png)

## Digging Deeper
This resolved the issue at hand, but I was left curious as to _why_ this behavior existed in the first place -- so I set about looking into that. Especially vexing was the fact I was able to find _almost nothing at all_ explaining why this behavior is what it is on MacOS. The most relevant thing I found was a [reference table on Wikipedia](https://en.wikipedia.org/wiki/Table_of_keyboard_shortcuts#Text_editing) associating `^v` with the action "Move the cursor down the length of the viewport" in emacs.

## Baked with Cocoa
This led me down the path of researching emacs keybindings on MacOS desktop applications. What I learned is that [MacOS uses some emacs keybindings throughout desktop applications via the Cocoa text system](https://www.hcs.harvard.edu/~jrus/site/cocoa-text.html). Not only that, but the bindings are user-customizeable (side note: the system default `StandardKeyBinding.dict` is in binary format, so you'll need to [convert to text and back again](https://discussions.apple.com/thread/1768480?answerId=8358753022#8358753022) if you intend to work with it). 

Additional information on user keybindings can be found in [Apple's Developer Documentation](https://developer.apple.com/library/archive/documentation/Cocoa/Conceptual/EventOverview/TextDefaultsBindings/TextDefaultsBindings.html)
