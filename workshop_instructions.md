# ISMR hands-on session installation instructions

In the following workshop, we will going through the process of training deep learning models for [robotic instrument segmentation](https://endovissub2017-roboticinstrumentsegmentation.grand-challenge.org/) using data generated from the AMBF simulator. After following this workshop, you will have trained a deep learning model who will received images from your simulated scene (left image) and generates masks on top instruments (right image).


|        Video input to Network        | Video output (segmentations) from Network |
| :----------------------------------: | :---------------------------------------: |
| <img src='./images/raw_video.gif' width="500"/> | <img src='./images/inferred_video.gif' width="500" /> |

<hr>

## Prerequisites and software installation overview

This tutorial assumes you have access to a Ubuntu 20.04 machine with ROS noetic installed. To train the segmentation model you will need GPU with at least 12GB of memory and NVIDIA drivers installed on your pc. In case, of not having a big enough GPU for training we will be providing instructions and scripts to train the network on the cloud service [Google colab](https://colab.research.google.com/). 

To run the necessary code we provide two different options:
  1. Anaconda virtual environment setup: Recomended for people with access to a GPU in their laptop and who would like to run the neural network in real-time.
  2. Python system wide setup: A minimal installation that allows the people to run the code without installing heavy dependencies such as pytorch or cuda. This instructions are only for people planning to use colab for the neural network training.  

**Note:** A third but not recommended option would be to install all the code dependencies in your system-wide python intepreter. Installing packages on your [system-wide interpreter](www.activestate.com/blog/how-to-manage-python-environments-global-vs-virtual/) is not usually recommended as it can lead to dependencies conflicts. If installing packages in your global interpreter is not a concern you, e.g., if you are working from a docker container, you can skip instructions regarding Anaconda installations and install everything in your global interpreter.

<hr>

## Software installation

This guide will include instructions to install all the necesary assets for this workshop. At the end of each of the following subsections you will see a <b>Checkpoint cmd</b> that will assess whether you have that specific component correctly installed. If you feel you have a component already installed, feel free to jump to the checkpoint cmd. Below is a list of all the software components you need for the workshop:

1. AMBF simulator
2. Surgical robotic assets 
3. dVRK segmentation scripts

<hr>

### <font size="+3"> <b>AMBF installation </b></font> 

Below are all the commands required to install AMBF. For more information about it please refer to the repository [here](https://github.com/WPI-AIM/ambf). To compile AMBF first follow the instructions below.

```bash


apt-get install ros-noetic-cv-bridge ros-noetic-image-transport
#CMD missing for AMBF dependencies

source /opt/ros/noetic/setup.bash #source ros libraries
git clone https://github.com/WPI-AIM/ambf
cd ambf
mkdir build 
cd build
cmake ..
make -j7
```

If compilation was succesful it will be recommended to add both the AMBF executable to path and automatically source ROS and ambf in the your terminal session. You can a complish the following by the adding the following block of code to your `.bashrc`.

```bash
#open bashrc
gedit ~/.bashrc
```

Then past the following configuration block:

```bash
activate_ros_env(){
    source /opt/ros/noetic/setup.bash #ROS
    export PATH=$PATH:/<path-to-AMBF>/bin/lin-x86_64 #AMBF
    source /<path-to-AMBF>/build/devel/setup.bash #AMBF
}
alias ros="activate_ros_env"
```

**Note**: Notice that we don't automatically source the the ros and ambf libraries. The reason to do this is to avoid conflicts with the anaconda virtual environment that will be created in the following steps.


<font size="+1"><b>Checkpoint cmd</b></font>

1. Open to window terminals.
2. In the first terminal execute `ros && roscore`
3. In the second terminal execute `ros && ambf_simulator`

This commands should open a window with AMBF.

<hr>

### <font size="+3"> <b>Surgical robotic assets installation</b></font>



All information about the surgical Robotics Challenge can be seen ([here](https://github.com/jabarragann/surgical_robotics_challenge)). To download the assets follow below:

```bash
#Download packages to stream images
apt-get install ros-<version>-cv-bridge ros-<version>-image-transport
#Clone surgical robotics challenge assets
git clone https://github.com/jabarragann/surgical_robotics_challenge.git
```

<font size="+1"><b>Checkpoint cmd</b></font>
```bash
./run_environment_3d_med.sh
```


### dVRK segmentation scripts installation


# Test details

<details>
  <summary>Epcot Center</summary>
  <p>Epcot is a theme park at Walt Disney World Resort featuring exciting attractions, international pavilions, award-winning fireworks and seasonal special events.</p>
  <b>hello</b>
</details>





<code>
print(hello)
</code>


<blockquote><code>% Bin/*/MarchingCubes --in zeta.B.edt --out zeta.B.-2.ply --value -2</code></blockquote>
<blockquote><code>% Bin/*/MarchingCubes --in zeta.B.edt --out zeta.B.0.ply  --value  0</code></blockquote>
<blockquote><code>% Bin/*/MarchingCubes --in zeta.B.edt --out zeta.B.5.ply  --value  5</code></blockquote>



```bash
hello world
```


##Install anaconda
```
wget -O ~/Downloads/Anaconda3-2023.03-Linux-x86_64.sh  https://repo.anaconda.com/archive/Anaconda3-2023.03-Linux-x86_64.sh

bash  ~/Downloads/Anaconda3-2023.03-Linux-x86_64.sh 
```


close terminal to reload anaconda.

Checkpoint command
conda info

### ambf install
sudo apt install libasound2-dev libgl1-mesa-dev xorg-dev
git clone https://github.com/WPI-AIM/ambf.git
cd ambf && mkdir build
cd build
cmake ..
make

## Anaconda 
Change surg_env for surg_seg


python packages
https://www.activestate.com/resources/quick-reads/how-to-list-python-packages-globally-installed-vs-locally-installed/#:~:text=Locally%20installed%20Python%20and%20all,Local%5CPrograms%5C%20for%20Windows.

python packages installed in system wide interpreter
  WARNING: The script natsort is installed in '/home/juanbarragan/.local/bin' which is not on PATH.
  Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
