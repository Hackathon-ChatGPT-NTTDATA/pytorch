## 10GB para dos GPUS 
##FROM nvcr.io/nvidia/pytorch:21.06-py3
## https://catalog.ngc.nvidia.com/orgs/nvidia/containers/pytorch/tags
##  nvcr.io/nvidia/pytorch:18.01-py3 
FROM nvcr.io/nvidia/pytorch:18.01-py3 

RUN pip install --upgrade pip && pip install \
    pytorch-lightning \
    timm \
    segmentation-models-pytorch

COPY ./main.py /workspace/main.py

WORKDIR /workspace
