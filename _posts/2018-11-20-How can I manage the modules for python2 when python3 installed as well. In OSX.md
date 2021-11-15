---
layout: post
title: 'How can I manage the modules for python2 when python3 installed as well. In OSX'
date: 2018-11-20 08:04:00 +0800
category: from_cnblogs
slug: p20181120080400
---
ref: https://stackoverflow.com/questions/53385448/how-can-i-manage-the-modules-for-python2-when-python3-installed-as-well-in-osx

I'm using OSX and I installed python3 with anaconda installed. In my OSX, there exists two versions of python, i.e. python2 and python3.

I managed the modules in anaconda which only affect modules in python3. But how can I manage(install, delete, update) the modules for python2?

I've checked some posts about 'python2 is at /usr/bin/python' . So it's ok to use python2 by '/usr/bin/python' without configuring alias. But, how can I manage(install, delete, update) the modules for python2 when python3 installed as well. In OSX.

Anaconda comes with a package and environment manager called conda. This is what you need to do:

Create a separate Python 2.7 environment, let's call it old and busted.

`conda create --name old_and_busted python=2.7`

Now switch to this environment:

`conda activate old_and_busted`

Verify it worked if you want:

`python --version`

Install some modules cool:

`conda install flask`

Bonus, use pip to install something cool in the same environment:

`pip install flask`

What environment are we in again?

`conda env list`

Let's check for that package:

`conda list`

Now this part is very important, make sure to do it often - go back to your Python 3 environment:

`conda activate base`

pipenv manages environments in a similar way. Anaconda specializes in packaging for scientific computing handling packaging non-python extensions (e. g. C, C++) dependencies well.

** Note on `conda` vs `source` for environment activation **

If conda activate does not work use `source activate`. This was changed in Anaconda 4.4.0.
If you have this in your .bash_profile (or .profile or other magical dotfile) you use source activate:

`export PATH="$HOME/anaconda3/bin:$PATH"`

If you have this updated code in your shell startup then you can use conda activate:

`. $HOME/anaconda3/etc/profile.d/conda.sh`
`conda activate`