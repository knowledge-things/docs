## 使用AppOFCuda转换光流图时异常

> https://developer.nvidia.com/opticalflow-sdk 
>
> https://docs.nvidia.com/video-technologies/optical-flow-sdk/nvofa-application-note/index.html

```
NvOFCudaAPI : m_ofAPI->nvCreateOpticalFlowCuda(m_cuContext, &m_hOF)returned error 2 at /opt/tylerz/tlt_dev/action_recognition/convert_dataset/Optical_Flow_SDK_2.0.23/Common/NvOFBase/NvOFCuda.cpp;35
```

错误代码2在NVIDIA的Optical Flow SDK中通常表示显印卡不支持该功能。NVIDIA的Optical Flow SDK在硬件级别支持Turing（如RTX 2000系列，GTX 1660、1650）和Ampere（如RTX 3000系列）架构的GPU。这意味着如果你正在使用的显卡不是基于这些架构，那么Optical Flow SDK就无法正确运行。经过确认NVIDIA Tesla V100: 这款GPU基于Volta架构。

如果你的硬件确实支持该SDK，那么这个错误可能是由于驱动程序的问题引起的。请确保你已经安装了最新的NVIDIA驱动程序，支持你的GPU和CUDA版本。

如果你的GPU不支持Optical Flow SDK，那么你可能需要寻找一个替代方案来计算光流，或者使用一个支持该功能的GPU。

**NVOFA硬件功能**

| Hardware Features                                 |  Turing   |  Ampere   |    Ada    |
| :------------------------------------------------ | :-------: | :-------: | :-------: |
| Optical flow mode                                 |   **Y**   |   **Y**   |   **Y**   |
| Support for external hints                        |   **Y**   |   **Y**   |   **Y**   |
| 4x4 grid size                                     |   **Y**   |   **Y**   |   **Y**   |
| 2x2 and 1x1 grid size                             |   **N**   |   **Y**   |   **Y**   |
| Hardware cost                                     |   **N**   |   **Y**   |   **Y**   |
| Region of interest (ROI) optical flow calculation |   **N**   |   **Y**   |   **Y**   |
| Maximum supported resolution                      | 4096x4096 | 8192x8192 | 8192x8192 |

通过添加参数指定运行的显卡`--gpuIndex=1`

```bash
./AppOFCuda -h
Usage: AppOFCuda [params] 
--input=<string>                Input filename [ e.g. inputDir/input*.png, inputDir/input%d.png, inputDir/input_wxh.yuv ]
                                Default value: None
--output=<string>               Output file base name [ e.g. outputDir/outFilename ]
                                Default value: None
--RoiConfig=<string>            Region of Interest filename 
                                Default value: None
--gpuIndex=<int32>              cuda device index
                                Default value: 0
--preset=<string>               perf preset for OF algo [ options : slow, medium, fast ]
                                Default value: medium
--visualFlow=<bool>             save flow vectors as RGB image
                                Default value: false
--inputBufferType=<string>      input cuda buffer type [options : cudaArray cudaDevicePtr]
                                Default value: cudaArray
--outputBufferType=<string>     output cuda buffer type [options : cudaArray cudaDevicePtr]
                                Default value: cudaArray
--useCudaStream=<bool>          Use cuda stream for input and output processing
                                Default value: false
--gridSize=<int32>              Block size per motion vector
                                Default value: 1
--measureFPS=<bool>             Measure performance(frames per second). When this option is set it is not mandatory to specify --output option, output is generated only if --output option is specified
                                Default value: false
```

