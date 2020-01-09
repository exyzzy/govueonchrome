# govueonchrome
# How to set up the Go+Vue Web Stack to Develop on a Chromebook
For years I have done my development on mac laptops and desktops — they certainly work well for this, and I still have/use them. However, as part of some promotion a few months back to get our company to try Google cloud services, Google sent me a free Chromebook. I had purchased and messed around with a Chromebook when they first came out and was somewhat underwhelmed. This time looking at them though I was impressed that even the Celeron version they sent me was very usable, and what’s more it now has Linux built-in. So I set off on a small experiment to see if I could configure all I needed in Chromebook’s Linux to do real development for my current stack. Cut to the chase: yes! It’s not quite as easy to set up as a mac, but it is very possible with just a little more effort. You can end up with a super nice dev setup on Chromebook for this stack, with really nothing substantial lacking, and maybe even save a few dollars over a Mac or Windows machine. I was so happy with the result that I recently bought a Pixelbook Go and replicated the setup on it to get more storage, a faster processor, better display, and sleeker HW.

Go (GoLang) is just an amazingly well designed language for modern web application back ends, which makes sense as it traces its lineage back through the original C. Indeed, the clarity and conciseness of The Go Programming Language reminded me very much of my favorite programming book, The C Programming Language. Likewise Vue JS is a straightforward and efficient reactive front end. Between the two (Go+Vue) you just need to fill in the gaps. Vuetify works really well for me as a Vue replacement for the bootstrap framework, that exceeds it in capability. Axios is a great promises based HTTP client that will allow you to remove JQuery from your stack. PostgresSQL is my preferred relational storage platform, but I also use AWS S3 for images. It’s hard to beat Heroku (and Heroku Postgres) for prototype hosting — free for development and cheap for a light user base, with the ability to scale as needed. If you get to big user numbers its easy enough to switch over to some other host.
To summarize my current favorite dev stack for web apps:
* Go for the Back End
* Gorilla for Go muxing
* Vue JS for the Front End
* Vuetify for Vue UI Framework
* Axios for HTTP Promises
* PostGresSQL for DB
* Heroku for Hosting
* AWS for image storage
* Sparkpost for email

Given this stack, you specifically do not need such things as Go Templating, Bootstrap, or jQuery.
Additionally, it is hard to find a better editor than Microsoft VS Code, which also works native on linux.
Over the rest of this article we will set up all these pieces on your Chromebook and exercise them along the way to make sure everything works. At the end we’ll deploy a sample Go+Vue app to Heroku.

## Enable Linux

Follow the normal Chromebook instructions to set up your Chromebook under your Google account and make sure you have internet access

1. Go to Settings
1. On the left of the screen click on Linux (Beta)
1. Click “Turn On”
1. Click “Install”

Now you have Linux.

Tip: With a terminal window in focus, type ctrl-shift-n to make a new terminal window, so you can have more than one.
Tip: use ctrl-shift-v to paste into a terminal window.

## Install Visual Studio Code
1. Go to https://code.visualstudio.com/download and click on the .deb option
1. Code will begin to download.
1. When finished click “Show in Folder”
1. Double click on the filename and click “Install”

Visual Studio Code is now in your Linux apps folder

Tip: you can always get back here by opening the File Manager and then My Files > Downloads, don’t forget to delete the download files.

Click on the Launcher and scroll down to the Linux apps folder, click on Visual Studio Code. Note that you can also start the editor from a terminal by typing `code`

Tip: Here are some useful VS Code Extensions to install:
* ESLint
* File Utils
* Go
* Prettier
* SQLTools
* Bracket Pair Colorizer 2
* Vetur

Tip: Use Ctrl-Shift-I to reformat an editor file, Prettier style

(..to be continued)
