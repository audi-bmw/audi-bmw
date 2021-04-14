# Prerequisites

:warning: Isaac currently only supports **Ubuntu 18.04 LTS** for development and simulation 

## ISAAC & ISAAC SIM Installation

### 1. Upgrade available packages
```bash
sudo apt-get update
sudo apt-get upgrade
sudo apt update
sudo apt upgrade
sudo apt install linux-oem-osp1 linux-firmware

sudo reboot
```

### 2. Install build essentials 
```bash
sudo apt-get install build-essential gcc make cmake
sudo apt-get install linux-headers-$(uname -r)


sudo reboot
```

### 3. Choose Nvidia drivers
* Reboot the system enter bios -> security and disable secure boot 
* Open 'software & update' on your PC
* Go to additional drivers
* Select nvidia 455 (or newer)
* Click apply
* Reboot

### 4. Install CUDA
Follow the very well written tutorial by Jean-Marc from [here.](https://blog.jeremarc.com/setup/nvidia/cuda/linux/2020/09/19/nvidia-cuda-setup.html)
You'll only need CUDA, no need to install all the dependencies after that.

### 6. Setup Isaac SDK
Download the SDK from [here.](https://developer.nvidia.com/isaac/downloads)
Save it to a fixed directory.
* Create a symbollic link for isaac using
    ```bash
    sudo ln -s <isaac_location_on_your_pc> /isaac
    ```
* Install isaac dependencies using
    ```bash
    cd /isaac
    chmod +x ./engine/engine/build/scripts/install_dependencies.sh
    ./engine/engine/build/scripts/install_dependencies.sh
    ```
* add ```source /usr/local/lib/bazel/bin/bazel-complete.bash``` to the end of ~/.bashrc script

### 7. Test Bazel
Alles gut? Let's check it out!

  ```bash
  cd /isaac/sdk/
  bazel build //apps/samples/stereo_dummy
  bazel run //apps/samples/stereo_dummy
  ```
Head over to your web browser and navigate to [http://localhost:3000](http://localhost:3000)
If everything went well, it should open the Isaac UI.

### 8. Installing Unity3D
* Download the Unity Hub image from [here.](https://unity3d.com/get-unity/download)
* Give permissions and download Unity 2019.3.6f1 
  ```
  chmod +x UnityHub.AppImage
  ./UnityHub.AppImage unityhub://2019.3.6f1/5c3fb0a11183
  ```
### 9. Fixing Communications Protocol
* Edit the python code in '/isaac/sdk/packages/pyalice/CapnpMessages.py': 
    1. at line 15
        ```
        PATH = "/**/*.capnp"       # the path to find all ".capnp" files
        ```
    2. at line 27 **AND** line 45 (subject to change depending to your isaac version)
        ```
        capnp_files = glob.glob(os.getcwd() + PATH, recursive=True)
        ```

### 10. Starting the Simulation
* Copy the ideal_app and ideal_app_sim-pkg folders to your home directory.
* Copy the unity simulation folder to any adequate directory.
* In a terminal, open Unity if it isn't already
  ```
  ./UnityHub.AppImage
  ```
* In Unity, Open the project found in the isaac sim folder/projects/sample
* To run ideal_app, do the following in another terminal:
  ```
  cd ~/ideal_app_sim-pkg/
  ./apps/ideal_app/ideal_app_sim/ideal_app_sim
  ```
* If the Factory01 scene is not selected, you can double click it on the lower panel folder by going into Assets/Robotics-Simulation/Scenes
* On the left pane, select the ScenarioLauncher gameobject.
* On the right pane, select your scenario location, e.g.: /Scenarios/TestScenarios/AWARE-Scenario1.xml
* Press Play on the Top, head over to [http://localhost:3000](http://localhost:3000) and enjoy the show!
