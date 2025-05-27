---
title: "Ruby on Rails 7: Windows Subsystem for Linux (WSL)"
date: 2024-07-22T15:34:30-04:00
toc: true
toc_label: "Contents"
toc_sticky: true
categories:
  - blog
tags:
  - Ruby
  - Rails
  - WSL
---

For me, using Windows as a development environment only made sense for dotnet languages, such as Visual Basic and C#. The idea of writing Ruby on Windows never felt natural.

However, with the advent of the Windows Subsystem for Linux (WSL), Microsoft promises to provide the best of both worlds. As I recently switched my machine from Fedora to Windows 10, I thought I’d experiment to see whether I could be pursuaded.

First, I downloaded Windows Terminal from the [Microsoft Store](https://learn.microsoft.com/en-us/windows/wsl/install). This neat application will allow us to manage WSL.

## Windows Subsystem for Linux (WSL)

WSL leverages virtualisation technology to allow Linux distributions on Windows. With each Linux distribution running inside its own isolated container.

The installation for WSL is a quick oneliner:
```bash
wsl --install
```

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-1.png" alt="this is a placeholder image" caption="Installation of the Windows Subsystem for Linux (WSL)." %}

This command will enable WSL v2 and download a Ubuntu instance. Once complete, set up your user account and give your machine a quick reboot. Further information can be found [here](https://learn.microsoft.com/en-us/windows/wsl/install).

That’s it, just one command was needed. To launch the Ubuntu instance, simply right-click the Windows Terminal application and select Ubuntu.

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-2.png" alt="this is a placeholder image" caption="Selecting the Ubuntu instance." %}

You can also set Ubuntu to be the default profile in the settings:

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-3.png" alt="this is a placeholder image" caption="Setting default profile to Ubuntu in the Terminal configuration." %}

Now that we have WSL setup with ubuntu, lets configure the shell.


## ZSH

Install ZSH and set it as the default shell:

```shell
sudo apt install zsh
```

Run `zsh` and select option 2.

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-4.png" alt="this is a placeholder image" caption="Selecting option 2 produces a standard zsh configuration file." %}

Change the default terminal shell from `BASH` to `ZSH`:

```shell
sudo chsh -s $(which zsh)
```

Restart the terminal and confirm you’re using ZSH:

```shell
echo $SHELL
```

This should output `/usr/bin/zsh` or similar.

## Making ZSH beautiful

Install `ohmyzsh`:

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-5.png" alt="this is a placeholder image" caption="Installing ohmyzsh." %}


Select a [theme](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes) and set it inside the `ZSH_THEME` attribute of your `.zshrc` file. I like half-life.

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-6.png" alt="this is a placeholder image" caption="The ZSH_THEME field is set to half-life." %}

## ASDF

Time to install a Ruby version manager. Most Ruby developers I’ve met use rbenv or Ruby Version Manager (RVM) but `asdf` is an all in one version manager for pretty much every programming language out there.

```shell
sudo apt install curl git; git clone https://github.com/asdf-vm/asdf.git ~/.asdf --branch v0.13.1
```

Once `asdf` has been downloaded, add the following to the bottom of your `~/.zshrc` file:

```shell
. "$HOME/.asdf/asdf.sh"
```

Restart your terminal and confirm `asdf` is installed:

```shell
asdf --version
```

Further information for installing and configuring ASDF can be found [here](https://asdf-vm.com/guide/getting-started.html).

ASDF works a little different to other ruby version managers. As it provides version management for a [variety of languages](https://github.com/asdf-vm/asdf-plugins#plugin-list), you have to select a relevant plugin to add. In our case we will add ruby:

```shell
asdf plugin add ruby
```

That was simple enough, now lets install Ubuntu’s dependencies for Ruby:

```shell
sudo apt-get install autoconf patch build-essential rustc libssl-dev libyaml-dev libreadline6-dev zlib1g-dev libgmp-dev libncurses5-dev libffi-dev libgdbm6 libgdbm-dev libdb-dev uuid-dev
```

If that was all successful, you can install the latest version of ruby with:

```shell
asdf install ruby latest
```

At the time of this article, the latest version was `3.3.0`.

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-7.png" alt="this is a placeholder image" caption="asdf installing the latest version of Ruby." %}

If this compiled correctly, you can then set the global system version of ruby and check its working:

```shell
asdf global ruby latest
ruby -v
```

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-8.png" alt="this is a placeholder image" caption="The latest version of Ruby is set for global access." %}

## Rails

Now that ruby is working, let’s install Rails:

```shell
gem install rails
```

You can then check it is installed:

```shell
rails -v
```

Lastly, you can make a new Rails project and spin it up:

```shell
rails new example_project
cd example_project
bundle exec rails s
```

You should then be able to access your Rails application [here](http://127.0.0.1:3000/).

{% include figure image_path="/assets/images/ruby-on-rails-7-windows-system-for-linux-wsl/image-9.png" alt="this is a placeholder image" caption="Rails web application is successfully running." %}