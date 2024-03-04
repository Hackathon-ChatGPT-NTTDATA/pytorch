FROM python:3.9-slim

RUN pip install --upgrade pip --root-user-action=ignore requests \
    jupyter \
    pytorch-lightning \
    timm \
    segmentation_models_pytorch


COPY ./main.py /workspace/main.py    

WORKDIR /workspace