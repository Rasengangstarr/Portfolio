---
title: "Hugo Static Web Site"
date: 2022-02-18T17:30:32Z
draft: false
summary: "How I made this site"
---

I've wanted to make some kind of portfolio website / blog for a few years now.

During some downtime at work (caused by a storm taking out the power where our Gitlab instance is stored), I got to googling around new javascript frameworks, looking for something other than the Vue.JS I already used at work to play around with this weekend.

Somewhere down that rabbit hole, I started looking into static website frameworks. Remembering that long time portfolio project idea, I decided to have a play with [Hugo](https://gohugo.io/).

I skim watched [This series](https://www.youtube.com/watch?v=qtIqKaDlqXo&list=PLLAZ4kZ9dFpOnyRlyS-liKL5ReHDcj4G3) on Youtube, then got to work.

# Installation

The official website told me i needed to use [Homebrew](https://brew.sh/) to install Hugo on my Linux machine. I think brew is the Mac-OS package manager, like apt or pacman, but anyway I haven't used it before.

The installation process was... icky:

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
test -d ~/.linuxbrew && eval "$(~/.linuxbrew/bin/brew shellenv)"
test -d /home/linuxbrew/.linuxbrew && eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
test -r ~/.bash_profile && echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.bash_profile
echo "eval \"\$($(brew --prefix)/bin/brew shellenv)\"" >> ~/.profile
```

Didn't read it or check it. I'm sure its fine.

Anyway, once that was done I could run

```
brew install hugo
```

And I was away.

# Creating the site

Hugo provides a bunch of commands for configuration etc.

I created a new directory for Hugo projects:

```
mkdir HUGO
```

Then jumped in there and created a new site:

```
cd HUGO
hugo new site portfolio
cd portfolio
```

And tested it worked by running:

```
hugo server -D
```

# Adding a theme

Next I added the [terminal](https://themes.gohugo.io/themes/hugo-theme-terminal/) theme to my site by cloning it's github project into the themes directory of the project.

Following the instructions provided by the theme's developer, i ran up the server again with:

```
hugo server -t terminal
```

No idea what that -t option does, but it worked.

# Messing with the theme

Top level configuration in Hugo is done in the config.toml file at the root directory of the project. Again, the theme developer provided some code to copy paste into there, with a bunch of parameters you can mess with.

For example, by default the theme renders the accents of the page in orange, so I messed with some different options they provided:

```
# ["orange", "blue", "red", "green", "pink"]
themeColor = "blue"
```

I also didn't like how everything was smushed to the left on the site, with an ugly bar in the middle, which didn't look anything like the clean examples they'd provided:

![Ugly offcenter default behaviour](/images/uglytheme.png)

Again, looking in that __config.toml__ file I found:

```
# set theme to full screen width
  fullWidthTheme = false 

# center theme with default width
  centerTheme = true 
```

Flipping that __centerTheme__ flag did the trick

# Making this page

To create this page, I used Hugo's command line tool:

```
hugo new projects/hugo-static-web-site.md
```

This creates the page in a subdirectory called projects. I also wanted projects to behave as a list page, so scanning the config file, I found and modified this:

```
[[languages.en.menu.main]]
identifier = "about"
name = "About"
url = "/about"
```

So that *Projects* appears in the top bar.

Finally, I used this [Cheatsheet](https://www.nextpertise.net/posts/210424_hugo_markdown_cheatsheet/) to make the page.

I also wanted to see what it would look like on my phone, so opened a port in the Pop OS (Ubuntu) firewall:

```
sudo ufw allow 1313
```

then ran the hugo server command with additional --bind and --baseURL parameters.

![It looks cool on mobile too](/images/mobilehugo.png)

I really like how the menu looks on the mobile version.

# Next steps and conclusion

Next I'll probably figure out the best way to upload this to github pages and host it there.

I was shocked at how easy this was to set up. I set everything up and created this page in less than a couple of hours. I'll probably go back and write up some recent projects, sort out the About Page, and write up any future projects on here.
