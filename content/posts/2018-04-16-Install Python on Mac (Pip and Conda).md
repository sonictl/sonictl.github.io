---
layout: post
title: 'Install Python on Mac (Pip & Conda)'
date: 2018-04-16 10:35:00 +0800
category: from_cnblogs
slug: p20180416103500
---
# Install Python on Mac (Pip & Conda)
标签（空格分隔）： 运维


---

## PIP

### 1. **Update macOS**

Before you start, make sure your macOS is up-to-date:

- Click on the Apple menu in the top-left corner.
- Select "System Preferences" > "Software Update".
- Install any available updates.

### 2. **Install Homebrew**

Homebrew is a package manager for macOS that simplifies the installation of software.

1. Open Terminal.

2. Install Homebrew by running:

   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   ```

3. Follow the on-screen instructions to complete the installation.

### 3. **Install Python**

1. **Install Python**:

   - With Homebrew, you can install Python by running:

     ```bash
     brew install python
     ```

   - Homebrew installs Python 3.x by default. Verify the installation:

     ```bash
     python3 --version
     ```

2. **Set Up `pip`**:

   - ```
     pip
     ```

      is the package installer for Python. It should be installed along with Python. Verify it with:

     ```bash
     pip3 --version
     ```



### 4. **Set the Global Index URL**

Use the following command to configure `pip3` to use the SJTU mirror as the default index URL:

```bash
pip3 config set global.index-url https://mirror.sjtu.edu.cn/pypi/web/simple
```

This command updates the `pip3` configuration to use the SJTU mirror for all package installations.

### 5. **Verify the Configuration**

To ensure that the configuration has been set correctly, you can check the current index URL with:

```bash
pip3 config get global.index-url
```

The output should be:

```
https://mirror.sjtu.edu.cn/pypi/web/simple
```

### 4. Set Up a Virtual Environment

Now, when you install packages using `pip3`, it will use the SJTU mirror by default. 

**Use `pip` to install `virtualenv`:**

```bash
pip3 install virtualenv
```

**Create a Virtual Environment**:

- Navigate to your project directory:

  ```bash
  cd path/to/your/project
  ```

- Create a virtual environment and create a 'my_env' folder in proj. Directory:

  ```bash
  python3 -m venv my_venv
  ```

- Activate the virtual environment:

  ```bash
  source my_venv/bin/activate
  ```

- Your terminal prompt will change to indicate that the virtual environment is active.

**Deactivate the Virtual Environment**:

- To deactivate, simply run:

  ```bash
  deactivate
  ```

### 5. **Install an IDE or Code Editor**

Choose an Integrated Development Environment (IDE) or code editor to write and manage your Python code. Popular choices include:

- **Visual Studio Code**:

  ```bash
  brew install --cask visual-studio-code
  ```

  - Install the Python extension from the VS Code Marketplace.

- **PyCharm**:

  ```bash
  brew install --cask pycharm
  ```

  - PyCharm has both a Professional (paid) and Community (free) edition.

### 6. **Install Additional Tools (Optional)**

Depending on your needs, you might want to install additional tools:

- **Git** (version control):

  ```bash
  brew install git
  ```

- **Node.js** (if you're working with full-stack development):

  ```bash
  brew install node
  ```

- **Docker** (for containerization):

  ```bash
  brew install --cask docker
  ```

### 7. **Test Your Setup**

1. Create a simple Python script to ensure everything is working:

   ```python
   # hello.py
   print("Hello, world!")
   ```

2. Run the script using:

   ```bash
   python3 hello.py
   ```

You should see "Hello, world!" printed to the terminal.

This setup will get you started with Python development on your new Mac. If you encounter any issues, make sure to check the documentation for the tools you’re using or seek help from the community forums.

# Conda vs Pip

- **Conda**：
  - **整合性：** Conda 提供了环境管理和包管理的一体化解决方案，简化了工作流程。
  - **环境和包的统一管理：** 环境和包都由 Conda 管理，减少了管理上的复杂性和潜在的兼容性问题。
  - **支持多语言：** 除了 Python，Conda 还可以管理其他语言和工具的包。
- **Pip**：
  - **分离性：** 环境管理和包管理由不同的工具（`venv`/`virtualenv` 和 `pip`）分别处理。
  - **灵活性：** 通过组合使用 `venv`/`virtualenv` 和 `pip`，可以实现对 Python 包的精细管理，但需要额外的配置和操作。
  - **专注 Python：** 主要关注 Python 包的管理，不支持非 Python 的依赖项。

在实际使用中，选择 Conda 还是 pip 主要取决于你的需求和项目复杂性。如果你需要一个全面的环境和包管理解决方案，Conda 是一个好的选择。而如果你更倾向于使用 Python 官方工具和喜欢灵活配置，使用 `pip` 和虚拟环境工具的组合可能更适合。

# Conda on Mac

### **1. Install Conda**

**a. Download and Install Miniconda**

Miniconda is a minimal installer for Conda. It provides the Conda package manager and a basic Python installation without the additional packages that come with Anaconda. This is often preferred to avoid unnecessary bloat and to get a more customizable setup.

1. **Download Miniconda for macOS (M1 chip):**

   - Go to the Miniconda download page.
   - Download the macOS ARM64 (Apple Silicon) version of the Miniconda installer. The file should be named something like `Miniconda3-latest-MacOSX-arm64.sh`.

2. **Open Terminal** and navigate to the directory where the installer was downloaded.

3. **Run the installer:**

   ```bash
   bash Miniconda3-latest-MacOSX-arm64.sh
   ```

4. **Follow the installation prompts:**

   - Press `Enter` to review the license agreement, then type `yes` to accept it.
   - Choose the installation location (the default is usually fine).
   - Allow the installer to initialize Miniconda by adding it to your `PATH`.

5. **Close and reopen Terminal** to activate the changes, or run:

   ```bash
   source ~/.bash_profile
   ```

   or

   ```bash
   source ~/.zshrc
   ```

   depending on your shell.

**b. Verify the Installation**

Check if Conda is installed correctly by running:

```bash
conda --version
```

This should display the Conda version number.

### **2. Set Up Conda**

**a. Update Conda**

It's a good idea to update Conda to ensure you have the latest features and fixes:

```bash
conda update conda
```

**b. Create a New Conda Environment**

Creating isolated environments allows you to manage dependencies for different projects separately:

```bash
conda create --name myenv python=3.10
```

Replace `myenv` with your desired environment name and `3.10` with the desired Python version.

**c. Activate the Environment**

Activate the new environment:

```bash
conda activate myenv
```

**d. Install Packages**

With your environment activated, you can now install packages using Conda:

```bash
conda install numpy pandas matplotlib
```

You can also install packages from conda-forge or other channels if needed:

```bash
conda install -c conda-forge some-package
```

**e. Deactivate the Environment**

To deactivate the environment and return to the base environment:

```bash
conda deactivate
```

### **3. (Optional) Install Additional Tools**

- **Install `mamba`**: An alternative to Conda that can be faster for resolving dependencies.

  ```bash
  conda install mamba -c conda-forge
  ```

- **Set Up Jupyter Notebook**: If you work with notebooks, you can install Jupyter:

  ```bash
  conda install jupyter
  ```

### **4. Managing Environments**

- **List Environments**: To see a list of all Conda environments:

  ```bash
  conda env list
  ```

- **Remove an Environment**: To delete an environment:

  ```bash
  conda remove --name myenv --all
  ```

By following these steps, you’ll have Conda set up on your new Mac with an M1 chip, and you’ll be ready to manage your Python environments and packages efficiently.



## Anaconda

Refer the link: [https://medium.com/@GalarnykMichael/install-python-on-mac-anaconda-ccd9f2014072](https://medium.com/@GalarnykMichael/install-python-on-mac-anaconda-ccd9f2014072)


The tutorial for installing pycharm and anaconda is [here](https://medium.com/@GalarnykMichael/setting-up-pycharm-with-anaconda-plus-installing-packages-windows-mac-db2b158bd8c).

As I'm now transfering from PC to Mac, I'm getting used to work on Mac, which is a suffering process. Oah...