## References

- `remote-opencv.dockerfile` is based on the following 2 dockerfile repositories.

https://github.com/janza/docker-python3-opencv/tree/master

https://github.com/JetBrains/clion-remote

- The Clion Remote Mode:

https://blog.jetbrains.com/clion/2020/01/using-docker-with-clion/

https://www.jetbrains.com/help/clion/remote-projects-support.html?_ga=2.228080923.707263221.1597415304-1774992309.1572786626#WorkWithRemote

## Build and Run

```bash
# build image
docker build -t clion/remote-cpp-env:0.5 -f remote-opencv.dockerfile .

# run container
docker run -d --cap-add sys_ptrace -p 2222:22 --name clion_remote_env -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix clion/remote-cpp-env:0.5

# make container be able to open display
xhost +

# (only for the first time) 
# Add Remote Host toolchain.
# Add CMake profile which uses Remote Host.
# Add Mappings in Deployment.
# Please refer to https://www.jetbrains.com/help/clion/remote-projects-support.html?_ga=2.228080923.707263221.1597415304-1774992309.1572786626#WorkWithRemote

# open clion (clion will transfer files for us)

# Enter into container and make and execute, and exit
docker exec -it clion_remote_env /bin/bash

# enable access control
xhost -
```

