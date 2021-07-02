##Windows
The easiest way to setup a pseudo-linux environment to work with Project Codenet/ Wala is to setup a docker environment. This will require installing and configuring docker with graphics support if at all possible.
Windows should have built in graphics driver support, so this is as simple as making sure docker can identiy your graphics device

##Linux

As for linux, you need not install in docker at all. However, in terms of getting a basic install up and running as quickly as possible, using the existing dockerfile ```julianwindows/wala:walacodenet``` is the fastest way to get fully setup.

This requires a number of steps:

First and foremost, you must install docker

For Arch, this comes in the form of

```sudo pacman -S docker```

After installing docker, you can test if the default config works by running

```sudo dockerd```

This attempts to start the docker daemon if possible. There is a chance you will see a device pool error if you have not installed the appropriate drivers for your graphics device.

For me, this required install the mesa package for intel and then adding ```i915``` to my initramfs config. 
There is better documentation regarding the full installation (including kernel module configuration) on the arch website

After doing this, I recompiled my kernel modules using 
```
sudo mkinitcpio -p linux
```

After doing this, you can verify that the drivers have installed properly using
```aidl
sudo dmesg | grep "i915 firmware"
```
This should print two-three lines assuming you are configuring i915

After this is setup, using the docker image at https://hub.docker.com/layers/julianwindows/wala/walacodenetdev/images/sha256-e033202eee429b8e6aabe74176df16d3a3495545af19b5c446bb8110a9e03999?context=explore

You can use a vnc viewer to attach to this image

From there, you need only verify your java version and then you should be able to compile the java11 WALA branch using
```aidl
./gradlew compileJava
```

Likewise, you should be able to compile the wala based tools in the Project_CodeNet repo
```aidl
./gradlew clean build publishToMavenLocal
```

This might fail due to compatibility and this is being looked into
