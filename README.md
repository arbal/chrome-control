# About This Fork

Forked from [d4rkb1ue/chrome-control](https://github.com/d4rkb1ue/chrome-control) to make an equivalent command line utility for [Brave Browser](https://github.com/brave/brave-browser). Predicably, it is named [brave-control](https://github.com/arbal/brave-control) and the script is named [brave.js](https://github.com/arbal/brave-control/blob/master/brave.js).

### Commands used for curious readers:

1. Modify code & comments. Commit.

```sh
perl -pi -e 's/chrome([^\w-])/brave$1/g' ./chrome.js
perl -pi -e 's/Google Chrome/Brave Browser/g' ./chrome.js
perl -pi -e 's/Chrome/Brave/g' ./chrome.js

git add ./chrome.js && git commit -m 'Modified chrome.js to use Brave Browser' && git push
```

#### **NOTE: In order to preserve history, it is important to commit any file rename operations separately from any file content changes.**

2. Use [git-mv](https://git-scm.com/docs/git-mv) to rename script. Commit.

```sh
git mv -v ./chrome.js ./brave.js
git commit -m 'Renamed chrome.js to brave.js' && git push
```

3. Use [hub](https://hub.github.com/) to rename this repo. Described in [github/hub#390](https://github.com/github/hub/issues/390#issuecomment-457858483).

```sh
hub api -XPATCH repos/arbal/chrome-control -f name=brave-control
git remote set-url origin https://github.com/arbal/brave-control.git
```

#### Notes from earlier [d4rkb1ue/archrome-control](https://github.com/d4rkb1ue/chrome-control) fork:

> Forked from [bit2pixel/chrome-control](https://github.com/bit2pixel/chrome-control) because the original repo isn't seem to be active any more, with these changes,

> 1. Apply performace patch from [bit2pixel/chrome-control/pull/7](https://github.com/bit2pixel/chrome-control/pull/7/)
> 2. Disable prompt for "Are you sure" for duplication removal
> 3. Search keyword in both title and url
>     - for example `[Hello World](https://exampledomain.com/hw)` can he searched by either `hello`, `world`, `hw` or `exampledomain`.
> 4. New feature to search for bookmarks. Command is `b`
> 5. Change command for Tabs searching to `t`


## How To Install

```sh
git clone https://github.com/arbal/brave-control.git
./install.sh
```

Or just download and double click,
- [Brave Control.alfredworkflow](https://github.com/arbal/brave-control/raw/master/integrations/Alfred/Brave%20Control.alfredworkflow)



Brave Control
==============

![Brave Control](img/banner.png)

A JXA script and an Alfred Workflow for controlling Brave Browser
(Javascript for Automation). Also see my [How I Navigate Hundreds of Tabs on Chrome with JXA and Alfred](https://medium.com/@bit2pixel/how-i-navigate-hundreds-of-tabs-on-chrome-with-jxa-and-alfred-9bbf971af02b) article if you're interested in learning how I created the workflow.

Usage
-----

Make this file an executable
```sh
chmod +x ./brave.js
```

Then run:
```sh
./brave.js
```


> MacOS will ask you to allow permissions for this tool to control Brave.
> Feel free to inspect the code before accepting.

Integration
-----------

You can use `brave-control` to create fun integrations with your favorite tools (Alfred, vim, vscode, iterm2, ...).

### Alfred

I've created an Alfred Workflow to use Brave Control.
You can find it under the intergrations directory.

Alfred Brave Control commands:
  - `tabs`: Lists all tabs
  - `close url <keywords>`: Close tabs with URLs matching these keywords
  - `close title <keywords>`: Close tabs with titles matching these keywords
  - `dedup`: Close duplicate tabs

![Alfred](img/tabs.gif)


Commands
--------

### Close duplicate tabs

```sh
./brave.js dedup
```


### Close all tabs by `titles` containing strings
> Strings are separated by spaces and `case insensitive`.

```sh
./brave.js close --title "inbox" "iphone - apple"
```

```sh
./brave.js close --title inbox iphone
```

### Close all tabs by `URLs` containing strings
> Strings are separated by spaces and `case insensitive`.

```sh
./brave.js close --url "mail.google" "apple"
```

```sh
./brave.js close --url google apple
```

### List all open tabs in all Brave windows

```sh
./brave.js list
```

The output is `JSON`, so you can pipe it to `jq`.
```sh
./brave.js list | jq .
```

Returns a struct like this:

```json
{
  "items": [
    {
      "title": "Inbox (1) - <hidden>@gmail.com - Gmail",
      "url": "https://mail.google.com/mail/u/0/#inbox",
      "winIdx": 0,
      "tabIdx": 0,
      "arg": "0,0",
      "subtitle": "https://mail.google.com/mail/u/0/#inbox"
    },
    {
      "title": "iPhone - Apple",
      "url": "https://www.apple.com/iphone/",
      "winIdx": 0,
      "tabIdx": 1,
      "arg": "0,1",
      "subtitle": "https://www.apple.com/iphone/"
    }
  ]
}
```

> `arg` and `subtitle` are used for [Alfred](https://www.alfredapp.com/) integration.

### Close a specific tab in a specific window

> `Window Index` and `Tab Index` is the `arg` returned by the `list` command.

```js
./brave.js close 0,13
```


### Focus on a specific tab in a specific window

```js
./brave.js focus 0,13
```

### Show help

```sh
./brave.js
```

### Don't prompt the user

`--yes` flag will cause no questions to be asked to the user. It'll close all tabs straight away.

> Attention: Use this with caution. Make sure you don't have any unsaved work, emails, ... etc.

### Prompt user in Brave

`--ui` flag will cause the questions to be asked using a Brave dialog instead of text in command line.

```sh
./brave.js close --url apple --ui
```

![Dialog](img/ui.png)

## How did you create the banner and icon?

It took around 4 hours using Photoshop. I wanted to use Illustrator but I realized that I completely forgot how to use it.

Drawing the shapes are pretty easy but I spent most of the time on the following:
- Picking harmonious colors.
- Figuring out how to make it less dull, ended up using a shadow under the window which made a significant difference.
- Figuring out how to represent the infrared waves.
- Finding a way to export the PNG to ICNS.

Planning to write an article on that soon.

## Bug Fixes & Pull Requests

If you find any bugs please feel to create an issue or create a pull request.

> I can only accept pull requests to the source code and not the binary files for ensuring the repo stays sanitized. If you have any feature requests for the workflow, I'd be happy to add them in after a discussion.


## License

Copyright (c) 2019 Renan Cakirerk

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

