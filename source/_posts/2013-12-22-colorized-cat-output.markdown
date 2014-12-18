---
layout: post
title: "Colorize Your Cat Output"
date: 2013-12-22 13:37
comments: true
categories: ['progamming', 'shell', 'command line', 'color', 'colorize', 'tips']
---

If you're like me and constantly displaying source files via cat, here's a tip for making that output colorized.

First, make sure you have ```easy_install``` installed:

```bash
curl -O http://python-distribute.org/distribute_setup.py
sudo python distribute_setup.py
sudo rm distribute_setup.py
```

Next, install Pygments:

```bash
sudo easy_install Pygments
```

Finally, create an alias ```ccat``` (colorized cat) or something similar in your ```.profile``` or ```.bash.rc``` or ```.bash_profile```:

```bash
alias ccat='pygmentize -O console256 -g'
```

Open a new terminal window or source the file containing your alias, and now you have colorized cat output!

<img src="/images/misc/colorized_cat.png" />
