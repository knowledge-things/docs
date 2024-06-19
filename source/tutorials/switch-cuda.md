switch-cuda
===========

Sometimes, it becomes necessary to switch to an earlier version of CUDA in order to run older code on a machine that is
actually set up to use the current version of the CUDA toolkit.
This is as simple as adjusting the values of a few environment variables, yet it is cumbersome to do manually.
Therefore, this repository provides the bash script [`switch-cuda.sh`](switch-cuda.sh), which adjusts the environment to
use a specific version of CUDA **for the current bash session**.

**Notice:** [`switch-cuda.sh`](switch-cuda.sh) has been written for and tested on Ubuntu 16.04, but is easily adapted
for other platforms.


Usage
-----

```
source switch-cuda.sh [VERSION]
```

Notice that the script has to be sourced rather than executed, as it performs changes of environment variables that are
supposed to persist after the script has finished.
If a version number is provided, than all relevant environment variables are adjusted to the required CUDA version
(including `PATH`, `LD_LIBRARY_PATH`, `CUDA_HOME`, and `CUDA_ROOT`).
If no version is provided, however, then the script simply prints all versions of CUDA that have been found on the used
machine (in `/usr/local`).


Examples
--------

```
$ source switch-cuda.sh 
The following CUDA installations have been found (in '/usr/local'):
* cuda-8.0
* cuda-9.0
* cuda-9.1
```

```
$ source switch-cuda.sh 9.0
Switched to CUDA 9.0.
```

switch-cuda.sh
---------

```shell
#!/usr/bin/env bash

# Copyright (c) 2018 Patrick Hohenecker
#
# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:
#
# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.
#
# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

# author:   Patrick Hohenecker <mail@paho.at>
# version:  2018.1
# date:     May 15, 2018


set -e


# ensure that the script has been sourced rather than just executed
if [[ "${BASH_SOURCE[0]}" = "${0}" ]]; then
    echo "Please use 'source' to execute switch-cuda.sh!"
    exit 1
fi

INSTALL_FOLDER="/usr/local"  # the location to look for CUDA installations at
TARGET_VERSION=${1}          # the target CUDA version to switch to (if provided)

# if no version to switch to has been provided, then just print all available CUDA installations
if [[ -z ${TARGET_VERSION} ]]; then
    echo "The following CUDA installations have been found (in '${INSTALL_FOLDER}'):"
    ls -l "${INSTALL_FOLDER}" | egrep -o "cuda-[0-9]+\\.[0-9]+$" | while read -r line; do
        echo "* ${line}"
    done
    set +e
    return
# otherwise, check whether there is an installation of the requested CUDA version
elif [[ ! -d "${INSTALL_FOLDER}/cuda-${TARGET_VERSION}" ]]; then
    echo "No installation of CUDA ${TARGET_VERSION} has been found!"
    set +e
    return
fi

# the path of the installation to use
cuda_path="${INSTALL_FOLDER}/cuda-${TARGET_VERSION}"

# filter out those CUDA entries from the PATH that are not needed anymore
path_elements=(${PATH//:/ })
new_path="${cuda_path}/bin"
for p in "${path_elements[@]}"; do
    if [[ ! ${p} =~ ^${INSTALL_FOLDER}/cuda ]]; then
        new_path="${new_path}:${p}"
    fi
done

# filter out those CUDA entries from the LD_LIBRARY_PATH that are not needed anymore
ld_path_elements=(${LD_LIBRARY_PATH//:/ })
new_ld_path="${cuda_path}/lib64:${cuda_path}/extras/CUPTI/lib64"
for p in "${ld_path_elements[@]}"; do
    if [[ ! ${p} =~ ^${INSTALL_FOLDER}/cuda ]]; then
        new_ld_path="${new_ld_path}:${p}"
    fi
done

# update environment variables
export CUDA_HOME="${cuda_path}"
export CUDA_ROOT="${cuda_path}"
export LD_LIBRARY_PATH="${new_ld_path}"
export PATH="${new_path}"

echo "Switched to CUDA ${TARGET_VERSION}."

set +e
return

```