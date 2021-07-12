---
layout: post
title: 'How to add conda env into jupyter notebook installed by pip'
date: 2021-06-15 13:03:00 +0800
category: from_cnblogs
slug: p20210615130300
---
## How to add conda env into jupyter notebook installed by pip
ref: https://medium.com/@nrk25693/how-to-add-your-conda-environment-to-your-jupyter-notebook-in-just-4-steps-abeab8b8d084

If your linux server has no Anaconda installed but conda installed. You don't want to install Anaconda for just installing jupyter notebook. You just installed jupyter notebook with pip3 with this [link](https://test-jupyter.readthedocs.io/en/latest/install.html#id4)

### 1. check the env on which jupyter notebook is runing:
   which pip3
   where pip3 #for windows cmd

### 2. add your Conda environment to your jupyter notebook:

**Step 1: Create a Conda environment.**

`conda create --name firstEnv`

once you have created the environment you will see some output after you create your environment.

**Step 2: Activate the environment**
 After you activate it, you can install any package you need in this environment.

For example, I am going to install Tensorflow in this environment. The command to do so,

  `conda install -c conda-forge tensorflow`

**Step 3: install ipykernel**

Now comes the step to set this conda environment on your jupyter notebook, to do so please install ipykernel.

   `conda install -c anaconda ipykernel`

After installing this,

just type,

 `python -m ipykernel install --user --name=firstEnv`

Using the above command, I will now have this conda environment in my Jupyter notebook.

Just check your Jupyter Notebook, to see the shining `firstEnv` under "new" list of jupyter first page.


### the newer comer: jupyterLab

https://jupyter.org/install
