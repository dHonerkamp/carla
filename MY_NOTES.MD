## Installation

- conda env with python 3.8

      conda create -n carla python=3.8
    
- install pygame: http://www.pygame.org/wiki/CompileUbuntu  
    NOTE: with ssh use `ssh -X`. Might not work from windows.
      
      pip install pygame

    OR https://stackoverflow.com/questions/19636480/installation-of-pygame-with-anaconda:
      
      conda install -c cogsci pygame
      
      
      
- install carla: pip only  has up to 0.9.5 (`pip install carla`). 
    For nightly, build from source, first do these steps 
    - Set the correct C path to use conda (o/w using wrong python version, 'pyconfig.h' file not found' error, https://github.com/carla-simulator/carla/issues/199):
      
           export CPLUS_INCLUDE_PATH="/home/honerkam/miniconda3/envs/carla/include/python3.5m" make setup
      
    - 'module distro not found': wants to install carla for both python2 and python3, so do:
    
          pip2 install distro && pip3 install distro

    - Then install system dependencies and build (only pythonAPI required): https://carla.readthedocs.io/en/latest/build_linux/

- add carla as a package:

      easy_install PythonAPI/carla/dist/carla-0.9.10-py3.5-linux-x86_64.egg

## Docker & local client setup

- carla docker: https://carla.readthedocs.io/en/latest/build_docker/

      docker run -p 2000-2002:2000-2002 --gpus all carlasim/carla:0.9.9 /bin/bash CarlaUE4.sh  < Your list of parameters >
   
   New command for latest version:
   
      sudo -E docker run --privileged --rm --gpus all -it --net=host -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix:rw carlasim/carla:latest
      
   - if `/tmp/.X11-unix/` doesn't exist: `xset q` seems to create it
   - `No protocol specified`: x11 permissions are incorrect. Try 
          
          xhost +local:
          
   carla master commit `c56a7738` seems compatible with current :latest image
- carla client: https://carla.readthedocs.io/en/latest/connecting_the_client/
- port forwarding:  
      
      alias ssh-carla='ssh -L 2000:aisgpu3:2000 -L 2001:aisgpu3:2001 -L 2002:aisgpu3:2002'

- oatomobile: set CARLA_ROOT (even if running it in Docker). Locally with pycharm I'm using this:

      CARLA_ROOT=/home/honerkam/repos/carla