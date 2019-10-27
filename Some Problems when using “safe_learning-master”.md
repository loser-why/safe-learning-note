## Some Problems when using “safe_learning-master”（In Windows）

There are several problem I met during testing your code provided in  https://github.com/befelix/safe_learning.  The aim of this markdown is just to share my experiences about running the code and solving the problems that I met. (My test platform: Windows 10 for 64 bit, Python 3.6). This noted is edited by Hongyuan Wang.



**ERROR#1**： Occurs during the installation of cvxpy package.

error: Failed building wheel for scs

error: Microsoft Visual C++14.0 is required. Get it with “Microsoft Visual C++ Build Tools”: http://landinghub.visualstudio.com/visual-cpp-build-tools

error: Failed building wheel for cvxpy

error: Microsoft Visual C++14.0 is required. Get it with “Microsoft Visual C++ Build Tools”: http://landinghub.visualstudio.com/visual-cpp-build-tools

 

**Solution:** This error can be settled by installing Microsoft Visual C++14.0 (for python 3), or Microsoft Visual C++9.0 (for python 2, but I don’t test it cause I do not use python 2). But it is actually not necessary to download and install Visual Studio. The another solution is to install the package by using install the .whl file of corresponding package. The specific installation step is shown as follows.

 step1: Download “scs‑1.2.7‑cp36‑cp36m‑win_amd64.whl” and “cvxpy‑1.0.25‑cp36‑cp36m‑win_amd64.whl” from <https://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml>.

step2:  Install  “scs‑1.2.7‑cp36‑cp36m‑win_amd64.whl” and “cvxpy‑1.0.25‑cp36‑cp36m‑win_amd64.whl” in command prompt (for example, Anaconda prompt) in order.

​            `pip install  scs‑1.2.7‑cp36‑cp36m‑win_amd64.whl`

​            `pip install cvxpy‑1.0.25‑cp36‑cp36m‑win_amd64.whl`

Bingo

**Note:** The website <https://www.lfd.uci.edu/~gohlke/pythonlibs/#lxml> do not provide the camplete historical version of cvxpy.whl. (it would be better if someone has cvxpy-1.0.15,whl). Actually, cvxpy-1.0.25 version and scs-1.2.7 version are also suitable for safe_learning-master package, although cvxpy>=1,<=1.0.15 and scs==2.0.2 are said to be required in “requirements.txt”.  Users can also try other version to see if they are suitable for running code successfully, as long as those fit your system configuration and python version.





**ERROR#2**： Occurs during the running examples in the file "../safe_learning-master/examples"

Someone may meet the following error when execute the example script:

error: No modular named "safe_learning"



**Solution**:  It is easy to be solved by adding following instructions at the beginning of each example script (but after "`from\__future__...`")

​         `import sys`

​         `sys.path.append("../")`



**ERROR#3**:  It should be noted that it may occurs an unexpected situation after you running one of examples if someone uses Spyder as an interpreter in Anaconda (.ipynb can be converted to .py directly in Spyder).  This unexpected situation shows an error in Console after running scripts:

`"An error ocurred while starting the kernel"` 

`...Check failed: PyBfloat16_Type.tp_base  !=nillptr`

which means the kernel in console is died at this case. As far as I know, it seems to ascribe this error to the software Spyder itself. Something unexpected or wrongs may happen in the internal of Spyder.  



**Solution**: First, it should be emphasized again that it just may happen occasionally, that is, the example scripts can be implemented successfully most of the times. This error seems to disappear when you restart the Spyder with a certain intervals between the new start and the last start (not sure the specific time intervals). It is possible to try those scripts in other interpreter rather than Spyder (but I don't try). The another choice is that someone can run the .ipynb file provided by author in Jupyter Notebook, and those examples will be implemented successfully.





**PROBLEM#1**:  Installation of gpflow==0.4.0

It seems that gpflow==0.4.0 cannot be installed successfully by using:

​                      `pip install gpflow==0.4.0`

**Solution**:  

1. Download source code of gpflow 0.4.0 from website: https://github.com/GPflow/GPflow/releases/tag/0.4.0

2. Extracting the .zip or tar.gz file

3. Locating the above extracted folder which contains the file "setup.py" in command prompt: 

   ​          `cd C:\Users\computer_name\Downloads\GPflow-0.4.0\GPflow-0.4.0`

4. Executing:

   ​          `pip install .`

   


