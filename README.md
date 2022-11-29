# TensorRT Inference Example



1. 先从damo yolo官方导出trt model，官方只支持tensorrt7，可以修改为8的，这个非常简单，注意导出end2end
   
   ```bash
   python tools/converter.py -f configs/damoyolo_tinynasL25_S.py -c damoyolo_tinynasL25_S.pth --batch_size 1 --img_size 640 --trt --end2end --half
   ```
2. 在cmakelist中，正确填写你的cuda以及tensorrt路径,cuda路径我写在了cmake指令，tensorrt路径在cmakelist里面
   
   ```bash
   mkdir build
   cmake -DCMAKE_CUDA_COMPILER=/usr/local/cuda-11.1/bin/nvcc ..
   make -j8
   ./yolort_trt --image ../samples/0126.jpg --model_path ../damoyolo_tinynasL25_S_end2end_fp16_bs1.trt --class_names ../coco.names
   ```
3. 可能还有些小问题，预处理、后处理啥的还没验证对齐，先跑通再说，之前连batchedNMS都不会处理，想起走佬之前提起这个插件，借鉴一下~