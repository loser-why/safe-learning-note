



## Some Problems in “safe_explorations-master”（in Windows）

 This note is edited by Hongyuan Wang. Hope it be helpful for you.



Problem#1: Two mistakes

1. Some scripts in "../experiments/journal_experiment_configs" occurs the error (denoted by Error_A):

   `TabError: inconsistent use of tabs and spaces in indentation`

2. Some scripts occurs the error (denoted by Error_B):

   `ImportError: attempted relative import with no known parent package`

Due to the existence of this two mistakes, some scripts cannot be executed successfully. I picked up the file which have one or both of above error.

"cart_pole_rl_cautious_mpc_nperf=X.py",  X=8,12,20.      (Error_B)

"cart_pole_rl_nsafe=X_nperf=Y_r=1_beta_safe=Z.py"       (both Error_A and Error_B)

​       (X=1, Y=0, Z=2)         (X=1, Y=10, Z=2)        (X=2, Y=0, Z=2)          (X=2, Y=10, Z=1)

​       (X=2, Y=10, Z=3)       (X=3, Y=0, Z=2）     （X=3, Y=10, Z=2）  （X=3, Y=10, Z=3）

​    （X=3, Y=15, Z=1)        (X=4, Y=0, Z=1)          (X=4, Y=0, Z=2)         (X=4, Y=10, Z=2)         

​      (X=4, Y=15, Z=1)        (X=5, Y=10, Z=2)       

“dynamic_exploration_invertedpendulum.py”  （Error_B）

 **Solution**:  

For Error_A: 

```
def __init__(self):
    """ """
    #self.cost = super._generate_cost()
    super(Config,self).__init__(__file__)
self.cost = super(Config,self)._generate_cost()     # error occurs here
```

is changed to:

```
def __init__(self):
    """ """
    #self.cost = super._generate_cost()
    super(Config,self).__init__(__file__)
    self.cost = super(Config,self)._generate_cost()  # note the indent
```

For Error_B:

```
from defaultconfig_episode import DefaultConfigEpisode
```

is changed to 

```
from .defaultconfig_episode import DefaultConfigEpisode
```



### Problem#2: Some of scripts cannot be run successfully totally

The following scripts can be successfully executed:

"cart_pole_rl_nsafe=X_nperf=Y_r=1_beta_safe=Z.py", where X=1,2,3,4,5, Y=0,10,15, Z=1,2,3

"dynamic_expl_inv_pend_nsafe=X_nperf=Y_r=1_beta_safe=2.py",  where X=1,2,3,4,5，Y=0,5

"static_expl_inv_pend_nsafe=X_nperf=0_r=1_beta_safe=2.py ", where X=2,3,4,5

​        **Note**: It seems that the script  "static_expl_inv_pend_nsafe=0_nperf=0_r=1_beta_safe=2.py "  failed to run.  That is, n_safe cannot be set to 1. It will report an error:

```
RuntimeError: .../casadi/core/slice.cpp:70: Assertion "stop<=len" failed:
Slice (start=0, stop=1, step=1) out of bounds with supplied length of 0
```

I still can't figure out what's wrong here.



For the scripts "cart_pole_rl_cautious_mpc_nperf=X.py", where X=5,8,10,12,15,20, it seems something unexpected happens while running the code. These code can be started to run at the beginning, but fail at a certain instant. At this moment, running will be interrupted by an error shown as follows:

```
Traceback (most recent call last):

  File "<ipython-input-4-6928c002004d>", line 1, in <module>
    runfile('C:/Users/Wanghongyuan/Downloads/safe-exploration-master/safe-exploration-master/experiments/run.py', wdir='C:/Users/Wanghongyuan/Downloads/safe-exploration-master/safe-exploration-master/experiments')

  File "E:\Anaconda\lib\site-packages\spyder\utils\site\sitecustomize.py", line 705, in runfile
    execfile(filename, namespace)

  File "E:\Anaconda\lib\site-packages\spyder\utils\site\sitecustomize.py", line 102, in execfile
    exec(compile(f.read(), filename, 'exec'), namespace)

  File "C:/Users/Wanghongyuan/Downloads/safe-exploration-master/safe-exploration-master/experiments/run.py", line 97, in <module>
    run_scenario(args)

  File "C:/Users/Wanghongyuan/Downloads/safe-exploration-master/safe-exploration-master/experiments/run.py", line 88, in run_scenario
    run_episodic(conf)

  File "c:\users\wanghongyuan\downloads\safe-exploration-master\safe-exploration-master\safe_exploration\episode_runner.py", line 80, in run_episodic
    solver.update_model(X, y, opt_hyp=conf.train_gp, reinitialize_solver=True)

  File "c:\users\wanghongyuan\downloads\safe-exploration-master\safe-exploration-master\safe_exploration\cautious_mpc.py", line 475, in update_model
    self.init_solver(self.cost_func)

  File "c:\users\wanghongyuan\downloads\safe-exploration-master\safe-exploration-master\safe_exploration\cautious_mpc.py", line 192, in init_solver
    solver = cas.nlpsol('solver', 'ipopt', prob, opt)

  File "E:\Anaconda\lib\site-packages\casadi\casadi.py", line 16278, in nlpsol
    return _casadi.nlpsol(*args)

RuntimeError: .../casadi/core/function_internal.cpp:124: Error calling IpoptInterface::init for 'solver':
Error in Function::factory for 'nlp' [MXFunction] at .../casadi/core/function.cpp:1449:
Failed to create nlp_hess_l:[x, p, lam:f, lam:g]->[hess:gamma:x:x] with {"gamma": [f, g]}:
.../casadi/core/factory.hpp:359: Hessian generation failed:
Error in MX::hessian at .../casadi/core/mx.cpp:1618:
Error in MX::jacobian at .../casadi/core/mx.cpp:1604:
Error in XFunction::jac for 'helper_jacobian_MX' [MXFunction] at .../casadi/core/x_function.hpp:670:
Error in MXFunction::ad_forward at .../casadi/core/mx_function.cpp:809:
Error in MX::ad_forward for node of type N6casadi4CallE at .../casadi/core/mx.cpp:1851:
Error in Call::ad_forward for 'adj1_pred_func' [SXFunction] at .../casadi/core/casadi_call.cpp:122:
Error in Function::forward for 'adj1_pred_func' [SXFunction] at .../casadi/core/function.cpp:972:
Error in XFunction::get_forward for 'adj1_pred_func' [SXFunction] at .../casadi/core/x_function.hpp:710:
std::bad_alloc
```

![1567736588151](C:\Users\Wanghongyuan\AppData\Roaming\Typora\typora-user-images\1567736588151.png)



For this situation, I am still confused it and do not figure out what it happen. I am not sure if these several scripts are running well in your platform. 





## Some Problems in “safe_learning-master”

There are several problem I met during testing your code provided in  https://github.com/befelix/safe_learning.  The aim of this markdown is just to share my experiences about running the code and solving the problems that I met. (My test platform: Windows 10 for 64 bit, Python 3.6)



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

   



