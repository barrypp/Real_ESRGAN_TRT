# Real-ESRGAN
The Pytorch implementation is [real-esrgan](https://github.com/xinntao/Real-ESRGAN).

## Config
- Input shape(**INPUT_H**, **INPUT_W**, **INPUT_C**) defined in real-esrgan.cpp
- GPU id(**DEVICE**) can be selected by the macro in real-esrgan.cpp
- **BATCH_SIZE** can be selected by the macro in real-esrgan.cpp
- FP16/FP32 can be selected by **PRECISION_MODE** in real-esrgan.cpp
- The example result can be visualized by **VISUALIZATION**. 

## Windows

1. prepare
 - install cuda, https://developer.nvidia.com/cuda-downloads
 - install opencv, install path C:\ProgramFiles, may need to change opencv.props, https://sourceforge.net/projects/opencvlibrary/files/
 - install TensorRT, may not compatible with latest cuda version, may need to change TensorRT.props, https://developer.nvidia.com/tensorrt
 - copy nvinfer.dll, opencv_world460.dll, opencv_world460d.dll to Real_ESRGAN_TRT.exe path or add them to system path

   thanks https://github.com/yester31/Real_ESRGAN_TRT/issues/8#issuecomment-1340507366


## How to Run, real-esrgan as example (original)

1. generate .wts from pytorch with .pt, or download .wts from model zoo

```
git clone https://github.com/xinntao/Real-ESRGAN.git
cd Real-ESRGAN
pip install basicsr
pip install facexlib
pip install gfpgan
pip install -r requirements.txt
python setup.py develop

// download https://github.com/xinntao/Real-ESRGAN/releases/download/v0.1.0/RealESRGAN_x4plus.pth
cp ~/RealESRGAN_x4plus.pth {xinntao}/Real-ESRGAN/experiments/pretrained_models

cp {tensorrtx}/Real-ESRGAN/gen_wts.py {xinntao}/Real-ESRGAN
cd {xinntao}/Real-ESRGAN
python gen_wts.py
// a file 'real-esrgan.wts' will be generated.
```

2. build tensorrtx/real-esrgan and run

```
cd {tensorrtx}/real-esrgan/
mkdir build
cd build
cp {xinntao}/Real-ESRGAN/real-esrgan.wts {tensorrtx}/real-esrgan/build
cmake ..
make
sudo ./real-esrgan -s [.wts] [.engine]   // serialize model to plan file
sudo ./real-esrgan -d [.engine] [image folder]  // deserialize and run inference, the images in [image folder] will be processed.
// For example
// sudo ./real-esrgan -s ./real-esrgan.wts ./real-esrgan_f32.engine
// sudo ./real-esrgan -d ./real-esrgan_f32.engine ../samples

```

3. check the images generated, as follows. _OST_009.png
