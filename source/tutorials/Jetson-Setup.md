# Jetson Setup

  This section explains how to prepare a Jetson device before installing the DeepStream SDK.

  ## Install Jetson SDK components

  Download NVIDIA SDK Manager from https://developer.nvidia.com/embedded/jetpack. You will use this to install JetPack 5.1 GA (corresponding to L4T 35.2.1 release)

  - NVIDIA SDK Manager is a graphical application which flashes and installs the JetPack packages.
  - The flashing procedure takes approximately 10-30 minutes, depending on the host system.

>  If you are using Jetson Xavier NX developer kit, you can download the SD card image from https://developer.nvidia.com/embedded/jetpack. This comes packaged with CUDA, TensorRT and cuDNN.

  ## Install Dependencies

  Enter the following commands to install the prerequisite packages:

  ```
$ sudo apt install \
libssl1.1 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstreamer-plugins-base1.0-dev \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev
  ```

  ### Install librdkafka (to enable Kafka protocol adaptor for message broker)

  1. Clone the librdkafka repository from GitHub:

     ```
     $ git clone https://github.com/edenhill/librdkafka.git
     ```

  2. Configure and build the library:

     ```
     $ cd librdkafka
     $ git reset --hard 7101c2310341ab3f4675fc565f64f0967e135a6a
     ./configure
     $ make
     $ sudo make install
     ```

  3. Copy the generated libraries to the deepstream directory:

     ```
     $ sudo mkdir -p /opt/nvidia/deepstream/deepstream-6.2/lib
     $ sudo cp /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream-6.2/lib
     ```

  ## Install latest NVIDIA BSP packages

  Installation of JetPack 5.1 GA will ensure that latest NVIDIA BSP packages are installed.

  ## Install the DeepStream SDK

  - **Method 1**: Using SDK Manager

    Select `DeepStreamSDK` from the `Additional SDKs` section along with JP 5.1 GA software components for installation.

  - **Method 2**: Using the DeepStream tar package: https://developer.nvidia.com/downloads/deepstream-sdk-v620-jetson-tbz2

    > 1. Download the DeepStream 6.2 Jetson tar package `deepstream_sdk_v6.2.0_jetson.tbz2` to the Jetson device.
    >
    > 2. Enter the following commands to extract and install the DeepStream SDK:
    >
    >    ```
    >    $  sudo tar -xvf deepstream_sdk_v6.2.0_jetson.tbz2 -C /
    >    $ cd /opt/nvidia/deepstream/deepstream-6.2
    >    $ sudo ./install.sh
    >    $ sudo ldconfig
    >    ```

  - **Method 3**: Using the DeepStream Debian package: https://developer.nvidia.com/downloads/deepstream-62-620-1-arm64-deb

    Download the DeepStream 6.2 Jetson Debian package `deepstream-6.2_6.2.0-1_arm64.deb` to the Jetson device. Enter the following command:

    ```
    $ sudo apt-get install ./deepstream-6.2_6.2.0-1_arm64.deb
    ```

  - **Method 4**: Use Docker containers

    DeepStream docker containers are available on NGC. See the [Docker Containers](https://docs.nvidia.com/metropolis/deepstream/dev-guide/text/DS_docker_containers.html) section to learn about developing and deploying DeepStream using docker containers.

  ### Run deepstream-app (the reference application)

  1. Navigate to the samples directory on the development kit.

  2. Enter the following command to run the reference application:

     ```
     $ deepstream-app -c <path_to_config_file>
     ```

     Where `<path_to_config_file>` is the pathname of one of the reference application’s configuration files, found in `configs/deepstream-app/`. See Package Contents for a list of the available files.

     Config files that can be run with deepstream-app:

     1. `source30_1080p_dec_infer-resnet_tiled_display_int8.txt`

     2. `source30_1080p_dec_preprocess_infer-resnet_tiled_display_int8.txt`

     3. `source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt`

     4. `source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8_gpu1.txt` (dGPU only)

     5. `source1_usb_dec_infer_resnet_int8.txt`

     6. `source1_csi_dec_infer_resnet_int8.txt` (Jetson only)

     7. `source2_csi_usb_dec_infer_resnet_int8.txt` (Jetson only)

     8. `source6_csi_dec_infer_resnet_int8.txt` (Jetson only)

     9. `source2_1080p_dec_infer-resnet_demux_int8.txt`

     10. `source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.yml`

     11. `source30_1080p_dec_infer-resnet_tiled_display_int8.yml`

     12. `source4_1080p_dec_preprocess_infer-resnet_preprocess_sgie_tiled_display_int8.txt`

         

     - You can find sample configuration files under `/opt/nvidia/deepstream/deepstream-6.2/samples` directory. Enter this command to see application usage:

       ```
       $ deepstream-app --help
       ```

     - To save TensorRT Engine/Plan file, run the following command:

       ```
       $ sudo deepstream-app -c <path_to_config_file>
       ```

  > 1. To show labels in 2D Tiled display view, expand the source of interest with mouse left-click on the source. To return to the tiled display, right-click anywhere in the window.
  > 2. Keyboard selection of source is also supported. On the console where application is running, press the `z` key followed by the desired row index (0 to 9), then the column index (0 to 9) to expand the source. To restore 2D Tiled display view, press `z` again.

  ### Boost the clocks

  After you have installed DeepStream SDK, run these commands on the Jetson device to boost the clocks:

  ```
$ sudo nvpmodel -m 0
$ sudo jetson_clocks
  ```

>  For Jetson Xavier NX, run sudo nvpmodel -m 8 instead of 0.

  ## Run precompiled sample applications

  1. Navigate to the chosen application directory inside `sources/apps/sample_apps`.

  2. Follow the directory’s README file to run the application.

     > If the application encounters errors and cannot create Gst elements, remove the GStreamer cache, then try again. To remove the GStreamer cache, enter this command: `$ rm ${HOME}/.cache/gstreamer-1.0/registry.aarch64.bin`
     >
     > When the application is run for a model which does not have an existing engine file, it may take up to a few minutes (depending on the platform and the model) for the file generation and the application launch. For later runs, these generated engine files can be reused for faster loading.

## Installing the reference graphs

Download reference graphs:

```
https://developer.nvidia.com/deepstream-getting-started
```

Install reference graphs:

```bash
sudo dpkg -i deepstream-reference-graphs-6.2.deb
```

Graphs are installed to:

```
/opt/nvidia/deepstream/deepstream/reference_graphs
```

### deepstream-test1

Simplest example of using DeepStream for object detection. Demonstrates decoding video from a file, performing object detection and overlaying bounding boxes on the frames.

Graph Files

deepstream-test1.yaml – The main graph file
parameters.yaml – File containing parameters for the various components in the graph
README - Contains detailed graph description and execution instructions
ds_test1_container_builder_dgpu.yaml - Configuration file for building application specific container for dGPU platform
ds_test1_container_builder_jetson.yaml - Configuration file for building application specific container for Jetson platform

Path - /opt/nvidia/deepstream/deepstream/reference_graphs/deepstream-test1

Sample Commands:

On x86:

```
$ /opt/nvidia/graph-composer/execute_graph.sh deepstream-test1.yaml \
      parameters.yaml -d ../common/target_x86_64.yaml
```

On Jetson:

```
$ /opt/nvidia/graph-composer/execute_graph.sh deepstream-test1.yaml \
      parameters.yaml -d ../common/target_aarch64.yaml
```

