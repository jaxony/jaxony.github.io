---
layout: post
title: Pain Free Way To Install OpenCV3 For Python On Mac OS
---

The title says it all. What follows is a quick tutorial on how to get OpenCV3 working on your Mac, with Python bindings. I have to say it's not easy to install it.

# Yet Another OpenCV Installation Tutorial!?
Most of this tutorial is based on Satya Mallick's [fantastic tutorial](https://www.learnopencv.com/install-opencv3-on-macos/), which I used to get things working. However, I *skipped some steps* because the `virtualenvwrapper` that Satya (and Adrian Rosebrock from [PyImageSearch](http://www.pyimagesearch.com/2016/12/19/install-opencv-3-on-macos-with-homebrew-the-easy-way/)) suggests to use just did not want to work on my peculiar system (probably a fault of mine).

Anyway, I used a simpler way to install OpenCV, and I think you should install OpenCV this way, too. It's less annoying.

I'll also explain some Unix stuff that confused me when I was (more of) a beginner at all this software magic.

# Tutorial
## Step 0. Install XCode.
If you're reading this tutorial, then you probably have this already. If not, install it from the App Store. This will be a couple gigabytes and take a while to install.

## Step 1. Install Homebrew.
Homebrew is an unofficial package manager for MacOS. It is also the package manager that MacOS needs and deserves. Jump into your terminal and run this command (Don't copy the `$` sign, it's just a convention to add it at the front of a command):

```bash
$ /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Now add this line to your hidden `.bash_profile` file located at your home directory (`~`). The `.bash_profile` file is a text file that gets run every time you log-in to a shell (`bash` is the default shell on Mac, and a "shell" is just an interface where you control the computer with text commands instead of clicks and mouse movements). We want to put set-up code in `.bash_profile` so that our environment is all set up the way we want when we open our shell.

Side note: If you're using `zsh` or other exotic shells, then you probably know what you're doing and I don't need to explain.

```bash
# After downloading Homebrew, open your .bash_profile
$ vim ~/.bash_profile
```

Inside `vim`, a ubiquitous text editor, press `i` to get into insert mode. Paste the following code on a new line:

```bash
export PATH=/usr/local/bin:$PATH
```
Note: this is where the `$` comes in handy! I don't want you to execute the above statement, so I didn't add a `$` to signify that it's NOT a command to run.

Here we have something called an environment variable. Here we're modifying `PATH`. We are prepending a new file directory `/usr/local/bin` so that `bash` looks for stuff in this directory *first* before searching in other paths inside our current PATH variable, whose values can be accessed with `$PATH`. This path is `export`ed so that other programs can access it too.

To save and quit vim, press `Esc` and type `:wq` for write -> quit.

Side note: Why do we need Homebrew? Because we'll install Python and OpenCV through Homebrew instead of labouring over it ourselves.

## Step 2. Install a verion of Python with Homebrew.
I recommend installing both Python 2.7 and Python 3.6.

```bash
$ brew install python
$ brew install python3
```

There already exists a system version of Python 2.7 (located in `/System/Library/Frameworks/Python.framework/Versions`) that's shipped with MacOS. That's why you can type `python` in the terminal and a Python interpreter appears. The reason we want to double up and, say, python2.7 with Homebrew (stored in a different directory on the computer) is because it's bad practice to do software development on the system's install. If you screw up the system's python version, then the system is screwed as it depends on that python install to run itself. If you screw up a python version that's not involved in system routines, then you can delete it and restart--no problems.

## Step 3. Make and activate a `virtualenv`.
This is the part where other tutorials tell you to install `virtualenvwrapper`. You don't need it. It's an added bonus, but it's also an added level of complexity. We'll just use a `virtualenv` (without the wrapper).

Use `pip`, a python package manager.
```bash
$ pip install virtualenv
```

Now, what's this `virtualenv` business? Jackson, you've told me to install new python versions, and then on top of this install a virtual environment?

I never appreciated `virtualenv`s when I started off coding, but now--three years later--I strictly do not start a project without having made one. A `virtualenv` keeps duplicated your python install (yet again!) in its own little directory, usually named `venv` though you can name it `dinosaur` if you want. That way, you can use `pip` to install exactly what you need for that project: the right versions of the packages, etc. When you don't have a `virtualenv`, you're using python packages that are shared across multiple projects, and versions start to conflict, and you'll understand why `virtualenv`s are necessary. You don't have to understand it now... But you will some day :)

Now create a virtual environment somewhere. I put it in the `~`.

```bash
# syntax: virtualenv -p <the python binary> <name of virtual env>
$ virtualenv -p python3.6 venv
```

You've made your house. Now you need to live inside it. Do this by activating the virtual environment by running the built-in Unix `source` command executes the code in a supplied file.

```bash
$ source ~/venv/bin/activate
```

FYI, `activate` is the binary file (hence why it lives in the `bin`ary directory of the virtualenv) that activates the virtualenv. We are simply running it with `source`.

## Step 4. Install OpenCV

```bash
$ brew install opencv
```

That's it. You don't need anything else. At least I didn't. This might take an hour.

## Step 5. Link stuff together!
This is were Satya's tutorial comes to the rescue.
- `echo` is a built-in bash command that writes what you give it to standard output (stdout).
- `OUTPUT > FILENAME` redirects output from stdout to a file by creating a new file of that name, or overwriting an existing file with the same name.
- `OUTPUT >> FILENAME` redirects output from stdout to a file. Instead of overwriting an existing file, `>>` appends the output to the existing file.

Combining the above knowledge, the command below simply:
- writes or appends the string `/usr/local/opt/opencv/lib/python3.6/site-packages` to a new/existing file located at `/usr/local/lib/python3.6/site-packages/opencv3.pth`

```bash
$ echo /usr/local/opt/opencv/lib/python3.6/site-packages >> /usr/local/lib/python3.6/site-packages/opencv3.pth
```
Note: Satya's tutorial told me to use `opencv3` directory but I instead had `opencv`.

If you `ls /usr/local/lib/python3.6/site-packages/`, you'll see `opencv3.pth` somewhere in there. `opencv3.pth` doesn't actually exist, so you're really making a new file. `>>` is used just to be safe.

Now finally run this:
```bash
$ cd ~/venv/lib/python3.6/site-packages
$ ln -s /usr/local/opt/opencv/lib/python3.6/site-packages/cv2.cpython-36m-darwin.so cv2.so
```
This creates a symbolic link (like a C pointer) called `cv2.so` (a shared object file) which doesn't actually contain any of the code. It refers whoever's trying to use it to instead go to the real thing, located at `/usr/local/opt/opencv/lib/python3.6/site-packages/cv2.cpython-36m-darwin.so`.

Check if it all worked!
```bash
$ import cv2
$ print(cv2.__version__)
'3.3.0'
```

Good luck!




