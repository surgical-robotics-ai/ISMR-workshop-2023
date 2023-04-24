# ISMR hands-on session installation instructions

In the following workshop, we will going through the process of training deep learning models for [robotic instrument segmentation](https://endovissub2017-roboticinstrumentsegmentation.grand-challenge.org/) using data generated from the AMBF simulator. After following this workshop, you will have trained a deep learning model who will received images from your simulated scene (left image) and generates masks on top instruments (right image).


|             Video input to Network              |       Video output (segmentations) from Network       |
| :---------------------------------------------: | :---------------------------------------------------: |
| <img src='./images/raw_video.gif' width="400"/> | <img src='./images/inferred_video.gif' width="400" /> |

<hr>

## Prerequisites and software installation overview

This tutorial assumes you have access to a Ubuntu 20.04 machine with ROS noetic installed. To train the segmentation model you will need GPU with at least 12GB of memory and NVIDIA drivers installed on your pc. In case, of not having a big enough GPU for training we will be providing instructions and scripts to train the network on the cloud service [Google colab](https://colab.research.google.com/). 

To run the necessary code we provide two different options
  1. Python system wide setup: A minimal installation that allows the people to run the code without installing heavy dependencies such as pytorch or cuda. This instructions are only for people planning to use colab for the neural network training.  
  2. Anaconda virtual environment setup (advanced): Recomended for people with access to a GPU in their laptop and who would like to run the neural network in real-time.

**Note:** A third but not recommended option would be to install all the code dependencies in your system-wide python intepreter. Installing packages on your [system-wide interpreter](www.activestate.com/blog/how-to-manage-python-environments-global-vs-virtual/) is not usually recommended as it can lead to dependencies conflicts. If installing packages in your global interpreter is not a concern you, e.g., if you are working from a docker container, you can skip the instructions regarding Anaconda installations and install everything in your global interpreter.

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

# Install ambf dependencies
sudo apt-get install libasound2-dev libgl1-mesa-dev xorg-dev
sudo apt-get install ros-noetic-cv-bridge ros-noetic-image-transport

source /opt/ros/noetic/setup.bash #source ros libraries
git clone https://github.com/WPI-AIM/ambf
cd ambf
mkdir build 
cd build
cmake ..
make -j7
```

**Note**: Do not continue unless compilation was succesful.

After compilation, it will be recommended to add the AMBF executable to path and automatically source ROS and ambf in the your terminal session. You can a complish the following by the adding the following block of code to your `.bashrc`.

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
ros ##Assuming you setup the alias in the previous step 
git clone https://github.com/jabarragann/surgical_robotics_challenge.git
cd surgical_robotics_challenge/scripts
pip install -e .

python -c "import surgical_robotics_challenge; print(surgical_robotics_challenge.__file__)"
```

**Note**: If you see an import error ask for help.

<font size="+1"><b>Checkpoint cmd</b></font>

```bash
cd <path-surgical-robotics-challenge>
./run_environment_3d_med.sh
```

This will open the simulated suturing environment seen in the images above.


<hr>

### <font size="+3"> <b>dVRK segmentation scripts installation</b></font>

All information about the dVRK segmentation scripts can be seen [here](https://github.com/Accelnet-project-repositories/dVRK-segmentation-models.git). To download the code please follow below:

```bash
git clone https://github.com/Accelnet-project-repositories/dVRK-segmentation-models.git
cd dVRK-segmentation-models
```

To run and install the necessary code we provide two different options
  1. Python system wide setup: A minimal installation that allows the people to run the code without installing heavy dependencies such as pytorch or cuda. This instructions are only for people planning to use colab for the neural network training.  
  2. Anaconda virtual environment setup (advanced): Recomended for people with access to a GPU in their laptop and who would like to run the neural network in real-time.

**Note:** A third but not recommended option would be to install all the code dependencies in your system-wide python intepreter. Installing packages on your [system-wide interpreter](www.activestate.com/blog/how-to-manage-python-environments-global-vs-virtual/) is not usually recommended as it can lead to dependencies conflicts. If installing packages in your global interpreter is not a concern you, e.g., if you are working from a docker container, you can skip the instructions regarding Anaconda installations and install everything in your global interpreter.

<hr>

<font size="+1"><b>Basic installation</b></font>

```bash
pip install -e . -r requirements_basic.txt --user
echo 'export PATH=$PATH:$HOME/.local/bin' >> ~/.bashrc # Add your local bin to path
```

<font size="+1"><b>Checkpoint cmd</b></font>

```bash
surg_seg_ros_video_record --help
```

<hr>

<details>
<summary><font size="+1"><b>Advanced installation (Anaconda setup)</b></font></summary>


Download anaconda install with 
```bash
  wget -O ~/Downloads/Anaconda3-2023.03-Linux-x86_64.sh  https://repo.anaconda.com/archive/Anaconda3-2023.03-Linux-x86_64.sh
  bash  ~/Downloads/Anaconda3-2023.03-Linux-x86_64.sh 
```

During installation, you will be prompt to choose whether to initialize Anaconda Distribution by running conda init. Enter “yes”. After installation, excute the next instruction to prevent base environment automatic activation

```bash
conda config --set auto_activate_base False
```

Close your terminal sessions to have the changes take effect.

**Note:** Keep in mind that having a Anaconda virtual env and ros env in the same terminal session can lead to problems. Do no install ROS or AMBF in a terminal session that has Anaconda activated.

```bash
cd <path-dVRK-segmentation-repo>
sudo apt install ffmpeg # Required for data recording script
conda create -n surg_seg python=3.9 numpy ipython  -y && conda activate surg_seg # Create and activate anacond virtual env
pip install -e . -r requirements.txt --user #install all required packages on environment
```

The installation of the requirements will take about 2min-5min due to pytorch and cuda installations.
</details>

<hr>


### <font size="+3"> <b>Workshop assets</b></font>

To download pre-trained models and other assets please clone the following repository:

```bash
git clone https://github.com/dayon95/AMBFSegmentation.git

```

<hr>

Congratulations for making it to the end!
