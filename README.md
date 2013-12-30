## Dynamic compilation of Zonation

These instruction describe the dynamic compilation of Zonation 3.1.11 on Ubuntu 13.04 (Raring Ringtail). Instructions have not been tested on other distributions/versions. You can copy the commands from this file or run the individual steps with the accompanied bash scripts. 

You execute all steps by running:

```
./00-run-all
```

### 1. Install Zonation dependencies

```
sudo apt-get update
sudo apt-get install cmake libqt4-dev libfftw3-dev libqwt-dev libboost-all-dev libgdal-dev
``` 

Alternatively, run:

```
./01-deps-zig3
```

### 2. Get Zonation sources

Create a suitable download directory, fetch Zonation source code (version 3.1.11) from CBIG server, and extract the sources to the directory created:

```
mkdir zonation3
cd zonation3
wget http://cbig.it.helsinki.fi/files/zonation/zig3-src/zig3-3.1.11-src.tar.bz2
tar -jxvf zig3-3.1.11-src.tar.bz2
```

Alternatively, run:

```
./02-wget-zig3
```

### 3. Build Zonation

To build Zonation library (`zig3lib`) and Zonation CLI utility (`zig3`), do the following:

```
mkdir zonation3/build
cd zonation3/build
cmake ../src-3
make
```

If you have several cores available for compilation, you can pass switch `-j X` to `make` where `X` is the number of designated cores (e.g. `make -j 4`).

Alternatively, run:

```
./03-build-zig3
```

### 4. Make Zonation available system wide (optional)

In order to call `zig3` anywhere on the system (instead of just the build-location), create a symbolic link:

```
sudo ln -s FULL_PATH/zonation3/build/zig3/zig3 /usr/local/bin/zig3
```

Replace `FULL_PATH` with the full path to the directory containing directory `zonation3` created in step 2.

Alternatively, run:

```
./04-postinstall-zig3
```

## Testing Zonation using the tutorial data

This stage is completely optional.

You can test that Zonation works by running some of the runs used in the [Hunter Valley tutorial](https://github.com/cbig/zonation-tutorial). This is not a thorough test, but it will at least give you an overview on whether Zonationis working as intended. Tutorial data and setup files are fetched using [git](http://git-scm.com/) and run using [zrunner](https://github.com/cbig/zrunner).

### 5. Install zrunner dependencies

First, install git:

```
sudo apt-get -y install git
```

Then, install Python packages needed by zrunner:

```
sudo apt-get -y install python-yaml python-pip 
```

Alternatively, run:

```
./05-deps-zrunner
```

### 6. Install zrunner

zrunner is installed directly from GitHub using [pip](http://www.pip-installer.org/en/latest/).

```
sudo pip install https://github.com/cbig/zrunner/archive/master.zip
```

Alternatively, run:

```
./06-install-zrunner
```

### 7. Clone Zonation tutorial using git

With git installed, clone the tutorial repository with the following command:

```
git clone https://github.com/cbig/zonation-tutorial.git
``` 

Alternatively, run:

```
./07-git-tutorial
```

### 8. Run the tutorial runs

You can run [5 basic tutorial variants](https://github.com/cbig/zonation-tutorial/tree/master/basic) defined in the configuration file `tutorial_runs.yaml` by using zrunner:

```
zrunner -l tutorial_runs.yaml
```

zrunner will produce an output file `results_XXX.yaml` in the same folder. `XXX` will correspond to information about your system. If everything went fine, you should see no critical errors on the screen and the yaml-file should report execution times for successful runs.