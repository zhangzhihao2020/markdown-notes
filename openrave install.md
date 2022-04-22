# openrave install
## Basic tools
###  python3
####  You can install python from the Ubuntu package repositories. Run the following commands in a terminal: 
`sudo apt-get update`
`sudo apt-get install ipython3 python3-dev python3-numpy python3-pip python3-scipy`

#### To test your installation, run the following commands: 
`python3 -c "import IPython; print('IPython v{}'.format(IPython.__version__))"`
​	*IPython v7.13.0*
`python3 -c "import numpy; print('numpy v{}'.format(numpy.__version__))"`
​	*numpy v1.17.4*
`python3 -c "import scipy; print('scipy v{}'.format(scipy.__version__))"`
​	*scipy v1.3.3*
`python3 -c "import sympy; print('sympy v{}'.format(sympy.__version__))"`
​	*sympy v1.6.2*

### Git 
#### Git is sophisticated version control software. First, you should create an account in GitHub.
#### Next, install Git from the Ubuntu package repositories 
`sudo apt-get update`
`sudo apt-get install git`
#### Set-up your Github details 
`git config --global user.name "your-github-username"`
`git config --global user.email "your-email@address.com"`
#### You can now clone the course repository 
`mkdir -p ~/catkin_ws/src`
`cd ~/catkin_ws/src`
`git clone https://github.com/crigroup/osr_course_pkgs.git`(替換網址  https://gitee.com/zhangzhihao2020/osr_course_pkgs.git)


# Installing OpenRAVE on Ubuntu 20.04
## Dependencies
### First, make sure the following programs are installed on your system:
`sudo apt-get install cmake g++ git ipython3 minizip python3-dev python3-h5py python3-numpy python3-scipy python3-sympy pyqt5-dev-tools`
CMake Error at python/bindings/CMakeLists.txt:50 (message):
  Should have installed pybind11; CMake will exit.

### Next, you will need to install the following libraries from the official Ubuntu repository:




# issue
## issue1
```CMake Error at python/bindings/CMakeLists.txt:50 (message):
 Should have installed pybind11; CMake will exit.
-- Configuring incomplete, errors occurred!
See also "/home/zzh/git/openrave/build/CMakeFiles/CMakeOutput.log".
See also "/home/zzh/git/openrave/build/CMakeFiles/CMakeError.log".
make: *** No targets specified and no makefile found.  Stop.
[sudo] password for zzh: 
Sorry, try again.
[sudo] password for zzh: 
make: *** No rule to make target 'install'.  Stop.```
```