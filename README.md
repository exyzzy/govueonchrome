# govueonchrome
Note: Go see the original [Medium Article](https://medium.com/@exyzzy/how-to-set-up-the-go-vue-web-stack-to-develop-on-a-chromebook-e609b192b17b)
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

## Install Go
Go to https://golang.org/dl/ and find the latest linux-amd64 version (1.13.5 in my case)

Type in your terminal window:
```
wget https://dl.google.com/go/go1.13.5.linux-amd64.tar.gz 
# or latest version
sudo tar -C /usr/local -xzf go1.13.5.linux-amd64.tar.gz
code ~/.profile
```

Add to bottom of the file after all other text:

```
GOROOT=/usr/local/go
GOPATH=$HOME/go
PATH=$GOPATH/bin:$GOROOT/bin:$PATH
export CGO_ENABLED=0
```

Save and close the file, in the terminal window type exit to close down your current terminal, then go to your linux folder and start a new terminal to load your new profile settings.

In the terminal, type `go version` to make sure go installed properly.

## Set up SSH keys to access Github and/or Bitbucket:
Type in the terminal window:
```
sudo apt-get install xclip 
# xclip is a clipboard util, it will be helpful
ssh-keygen 
# accept the default filename/location
# type in passphrase (remember it)
sudo apt-get install mercurial 
# needed by bitbucket
ls ~/.ssh
# to verify keys generated
eval $(ssh-agent) 
ssh-add ~/.ssh/id_rsa 
# type in the passphrase you just remembered 
cat ~/.ssh/id_rsa.pub | xclip -sel clip 
# to copy your public key into your paste buffer
```

Now go to bitbucket and github settings, SSH keys sections and upload your new SSH public key to your account. It’s in the paste buffer, so just ctrl-v it.

## Set up git
1. Type `git version`, to check your git version
1. Go to: https://git-scm.com/downloads, your version should be fine, but if not: `sudo apt-get install git`
1. Now config git:

```
git config --list 
# nothing there, right? So obviously, use your own settings below:
git config --global user.email “eric@foo.com”
git config --global user.name “Eric Lang”
git config --global url.”git@bitbucket.org:”.insteadof https://bitbucket.org
git config --global url.”git@github.com:”.insteadof https://github.com
git config --global core.editor “code --wait”
# ok, let's try it all..
```
## Clone a sample Go-Vue Project, Go Style
Use Go’s Get function to get a project from github
```
go get github.com/exyzzy/govueintro
cd ~/go/src/github.com/exyzzy/govueintro
# there it is, let’s load it up and take a peek, 
# but don't change anything yet
code .
```
Install Latest PostgreSQL
Note: you really do need to get the latest Postgres (12.1 at this writing) to be compatible with Heroku. Details here.
```
sudo apt update
sudo apt -y upgrade
sudo shutdown -r now
# close all terminals, and open a new one
sudo apt update
sudo apt-get install lsb-release
sudo apt -y install gnupg2
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt -y install postgresql-12 postgresql-client-12
```

You can now start the database server using:
```
pg_ctlcluster 12 main start 
psql -V 
# to confirm version of postgres (12.1 or greater)
```

Now edit the postgres config for local mode:

```
sudo su - postgres
cd /etc/postgresql/12/main/
edit pg_hba.conf
```

Change the last column of ALL rows to be trust, eg:
```
local all postgres peer
```
to:
```
local all postgres trust
```
`exit` to exit postgres su mode.

Note that your postgres account may not have access to VS code. So use edit (vi) the default system editor. A few commands that may help if you are unfamiliar:

* arrow keys to navigate
* dw to delete word under the cursor
* dd to delete line under the cursor
* i to go into “insert mode” to type new stuff at the cursor
* esc to exit insert mode
* w to write
* q to quit
* wq to write and quit
Now create yourself as admin:
```
sudo su - postgres
createuser --interactive --pwprompt
# use your name here
ericlang ericlang y 
# role password superuser?
exit
```
## Install Heroku CLI
```
curl https://cli-assets.heroku.com/install.sh | sh
#make a heroku account first, if you have not yet
heroku login -i
```
Note: Contrary to Heroku docs, Snapd did not work for me.
## Make sure it all works

Reboot the Chromebook first.
```
# create the user/pass for local db that govueintro assumes
cd ~/go/src/github.com/exyzzy/govueintro
createuser -P -d govueintro
# use password: govueintro
createdb govueintro
go install
# now run the app:
govueintro
```

This part is a little confusing. Normally to see your app running you’d point your browser at localhost. However, the linux terminal on a chromebox is a full fledged VM running in a different space from the chrome browser. The crostini devs solved this by creating an external interface for the container called penguin.linux.test, as described here and here. So to see govueintro with the main chromebox browser you can get to it with http://penguin.linux.test:8000 It is still called localhost for any commands issued in the linux terminal (like curl, below). Another solution is to install a browser in the linux VM itself (like sudo apt-get install firefox-esr , and localhost will work just fine, but then you have two browsers installed).

```
# open another terminal (ctrl-shift-n)
# to create the database tables, type: 
curl -X DELETE http://localhost:8000/api/clean
# can alternatively run: psql -U govueintro -f data/setup.sql -d govueintro
```

Now open: http://penguin.linux.test:8000 and you can play with the govueintro application — see the todos menu.
Everything should be building and running locally now. Let’s deploy it to Heroku.

```
cd ~/go/src/github.com/exyzzy/govueintro
heroku create
# note the name of the app heroku gives you, and use it below 
# lit-wildwood-48301 is my app name
heroku addons:create heroku-postgresql:hobby-dev
heroku git:remote -a lit-wildwood-48301
git push heroku master
curl -X DELETE http://lit-wildwood-48301.herokuapp.com/api/clean
```
All done, now go to the heroku app you just made. Mine is https://lit-wildwood-48301.herokuapp.com (yours will be different). Have fun.
