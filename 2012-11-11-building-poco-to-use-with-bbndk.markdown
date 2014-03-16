Title: Building POCO to use with BBNDK
Date: 2012-11-11 19:38
Tags: BB10, BBNDK, BB10, Blackberry, Cascades, C++, Native, Playbook, POCO, QNX

On the GitHub page of [OpenFramework project](https://github.com/falcon4ever/openFrameworks/tree/developPlayBook/addons/ofxQNX/libs/poco/Sources) there is a guide that shows how to build [POCO C++ libraries](http://pocoproject.org) for Blackberry 10 and Playbook on your Linux or Mac OS box.

These steps are:

1. Download and install the BlackBerry Native SDK for your platform from the [Cascade](https://developer.blackberry.com/cascades/download/) or [Native](http://developer.blackberry.com/native/download/) downloads page.

2. Download a stable release from the project [downloads page](http://pocoproject.org/download/index.html). Basic Edition is enough because crypto, mysql, odbc, openssl support will be ommited during the build process. For now, the latest release is [1.4.4](https://sourceforge.net/projects/poco/files/sources/poco-1.4.4/poco-1.4.4.tar.gz/download).

3. Extract the downloaded archive file.

4. Create directories for one or several of the available building targets for which you need to compile the library: BB10 simulator, BB10 target, Playbook simulator, Playbook target.

5. Download the appropriate configuration profile(s) [BB10 simulator](https://github.com/falcon4ever/openFrameworks/blob/developPlayBook/addons/ofxQNX/libs/poco/Sources/QNX-bb10-sim), [BB10 target](https://github.com/falcon4ever/openFrameworks/blob/developPlayBook/addons/ofxQNX/libs/poco/Sources/QNX-bb10-device), [Playbook simulator](https://github.com/falcon4ever/openFrameworks/blob/developPlayBook/addons/ofxQNX/libs/poco/Sources/QNX-playbook-sim), [Playbook target](https://github.com/falcon4ever/openFrameworks/blob/developPlayBook/addons/ofxQNX/libs/poco/Sources/QNX-playbook-device) and store it (them) to the "Unpacked POCO root"\build\config directory.

6. Download [syslog.h](https://github.com/falcon4ever/openFrameworks/blob/developPlayBook/addons/ofxQNX/libs/poco/Sources/syslog.h) and copy it to "SDK root"\target\qnx6\usr\include directory.

7. Build library for the chosen configuration profile and then add it to your project.

Load the environment variables for the console session:
```
source "SDK root"/bbndk-env.sh
```
Configuring build:
```
cd "Unpacked POCO root"
./configure --config="Profile file name" --omit=NetSSL_OpenSSL,Crypto,Data/ODBC,Data/MySQL --no-tests --no-samples --static --prefix="Profile result directory"
```
Compiling and installing result of the build in the "Profile result directory":
```
make -s
make install
```