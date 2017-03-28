# TensorBoard for MXNet

[![Build Status](https://travis-ci.org/zihaolucky/tensorboard-distro.svg?branch=v1.0.0)](https://travis-ci.org/zihaolucky/tensorboard-distro)
[![PyPI version](https://badge.fury.io/py/tensorboard.svg)](https://badge.fury.io/py/tensorboard)

Bring [TensorBoard](https://github.com/tensorflow/tensorflow/tree/master/tensorflow/tensorboard) to MXNet.

> TensorBoard is a suite of web applications for inspecting and understanding your TensorFlow runs and graphs. TensorBoard currently supports five visualizations: scalars, images, audio, histograms, and the graph.  

> This README gives an overview of key concepts in TensorBoard, as well as how to interpret the visualizations TensorBoard provides. For an in-depth example of using TensorBoard, see the tutorial: [TensorBoard: Visualizing Learning](https://www.tensorflow.org/versions/master/how_tos/summaries_and_tensorboard/index.html). For in-depth information on the Graph Visualizer, see this tutorial: [TensorBoard: Graph Visualization](https://www.tensorflow.org/versions/master/how_tos/graph_viz/index.html).  

## Installing via PyPI(recommended)
It's easy to install TensorBoard via PyPI:

```
pip install tensorboard
```

Please submit your feedback at https://github.com/dmlc/tensorboard/issues/19 if you have any troubles in using TensorBoard PyPI.

We maintain our PyPI at [tensorboard-distro)](https://github.com/zihaolucky/tensorboard-distro)

## Installing from source
When installing from source you will build a pip wheel that you then install using pip. We provide a `installer.sh` and `build_pip_package.sh` for you to get that pip wheel.

We’re also working on providing a pre-built pip wheel for you, so you can install TensorBoard package more easily. We would let you know once we finish this feature but currently it has to be installed from source.

### Clone the TensorBoard repository

```bash
$ git clone https://github.com/dmlc/tensorboard.git
```

### Prepare environment for Linux

#### Install Protocol Compiler
Note that this requires [Protocol Buffers 3](https://developers.google.com/protocol-buffers/?hl=en) compiler, so please install it.

#### Install Bazel

Follow instructions [here](http://bazel.build/docs/install.html) to install the dependencies for bazel.

#### Install other dependencies

```bash
# For Python 2.7:
$ sudo apt-get install python-numpy python-dev python-wheel python-mock python-protobuf
# For Python 3.x:
$ sudo apt-get install python3-numpy python3-dev python3-wheel python3-mock
```

### Prepare environment for Mac OS X

#### Install Protocol Compiler

Note that this requires [Protocol Buffers 3](https://developers.google.com/protocol-buffers/?hl=en) compiler, so please install it.

#### Install Bazel

Follow instructions [here](http://bazel.build/docs/install.html) to install the
dependencies for bazel. You can then use homebrew to install bazel:

```bash
$ brew install bazel
```

#### Dependencies

You can install the python dependencies using easy_install or pip, or conda if you use Anaconda for virtual-env management. Using
conda, run

```bash
$ conda install six, numpy, wheel, protobuf
```


### Prepare envronment for Windows

#### Install Protocol Compiler

Note that this requires [Protocol Buffers 3](https://developers.google.com/protocol-buffers/?hl=en) compiler, so please install it as follows:
1. Download the windows version (protoc-xxxx-win32.zip) and extract (do not use spaces in the folder name e.g. C:/protoc-buffer)
2. Add the protocol buffer bin folder to the windows PATH (e.g. C:/protoc-buffer/bin).

#### Install MS Visual Studio
bazel requires visual studio to be installed. The [Visual Studio Community](https://www.visualstudio.com/vs/community/) free version can be used.
After installation, make sure the installation is OK by running a Hello World C example.
You need to install Windows SDK for Windows 8 (available as an option within the installation of Visual Studio).

#### Install Anaconda 
Install [Anaconda](https://www.continuum.io/downloads). Use installer for python 2.7.
- Install for current user only to avoid permission issues.
- Select the installation root directory to be C:/Anaconda2 (to avoid spaces).
- Make it the default system python2.7 (option avilable through the installation).

#### Install bazel for windows
This is the [bazel-windows](https://bazel.build/versions/master/docs/install-windows.html) installation instructions.
The simplest method is to use [chocolatey](https://chocolatey.org/) windows package manager. To install chocolatey follow this [link](https://chocolatey.org/install)).
- You may need to execute "Set-ExecutionPolicy Unrestricted" before installation.
- After chocolatey is installed , run "choco install bazel" command. It will install bazel dependencies (python27, JDK and MSYS) and then installs bazel. The python27 is not required since Anaconda will be used.

After bazel is installed, apply the following system configurations:
1. Create a Windows Environment Variable BAZEL_SH=path/to/MSYS/bash(typically it will be C:\tools\msys64\usr\bin\bash.exe).
2. Add the MSYS bin (e.g. c:\tools\msys64\usr\bin) to the windows PATH at its start (before references to windows shell paths).
3. Create an environment variable JAVA_HOME=path/to/jdk (e.g. C:\Program Files\Java\jdk1.8.0_121).
4. bazel will automatically configure BAZEL_VS and BAZEL_PYTHON after it starts.
5. When bazel is running, it will search for python27.lib (actually the visual studio linker does). It is located under (C:/Anaconda2/libs). The Visual Studio must be able to see python27.lib in any of its search paths. This is can be done by copying python27.lib under one of the default search paths for visual studio built-in libs e.g. C:\Program Files (x86)\Windows Kits\8.1\Lib\winv6.3\um\x64). If there is a way to let VS see C:/Anaconda2/libs, it would be better.


### Build (Non-Windows)

After that, to build the first part, simply:

```bash
$ cd tensorboard
$ sh installer.sh
# In this process, it might need configuration or failed in bazel build, just retry the specific step.
```
#### Configure the installation

For example(just type ’N’ for all case as we don’t need them):

```bash
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:
Do you wish to build TensorFlow with Google Cloud Platform support? [y/N] N
No Google Cloud Platform support will be enabled for TensorFlow
Do you wish to build TensorFlow with GPU support? [y/N] N
Do you wish to build TensorFlow with OpenCL support? [y/N] N
```

### Build on Windows
For Windows run "bash installer_windows.sh" from cmd.exe

```bash
C:\>cd tensorboard
C:\tensorboard>bash installer.sh
# In this process, it might need configuration or failed in bazel build, just retry the specific step.
```

#### Configure the installation
After running the installer_windows.sh, the configuration options will appear. Type 'N' for all options since they are not needed.
Just use "C:/Anaconda2/python.exe" as the location of python.
```bash
$ ./configure
Please specify the location of python. [Default is /usr/bin/python]:C:/Anaconda2/python.exe
```


## Usage
`dmlc/tensorboard` contains two parts in general, currently we have [Python interface](https://github.com/dmlc/tensorboard/tree/master/python) 
for writing/logging `scalar`, `histogram` and `image` data to `EventFile`, which the front-end load data from this event file for visualization.

Technically, we reuse the rendering part of original TensorBoard of TensorFlow, but rewrite the logging part in pure Python without touching the 
TensorFlow code. We've try to keep the concepts consistent but the logging API might has some slightly difference.

### Logging

See [README](python/README.md) in Python package.

### Rendering 

```bash
$ tensorboard --logdir=path/to/logs
``` 


## Contribute

You might want to see the development note of this project at our DMLC blog: [Bring TensorBoard to MXNet](http://dmlc.ml/2017/01/07/bring-TensorBoard-to-MXNet.html)

Feel free to contribute your work and don't hesitate to discuss in issue with your ideas.
