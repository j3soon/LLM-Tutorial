# LLM Tutorial

LLM tutorial materials include but not limited to NVIDIA NeMo, TensorRT-LLM, Triton Inference Server, and NeMo Guardrails.

This material is used in the [NCHC LLM Bootcamp](https://www.openhackathons.org/s/siteevent/a0CUP00000L45Bx2AJ/se000345).

## Running on [TWCC](https://www.twcc.ai/)

Please follow this [TWCC README](./README_TWCC.md) to run the tutorials on TWCC.

## Running Locally

Install [docker](https://docs.docker.com/engine/install/ubuntu/) and [nvidia container toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html). Then add your user to the docker group and re-login/restart.

```sh
git clone https://github.com/j3soon/LLM-Tutorial.git
cd LLM-Tutorial

# (a) NeMo
docker run --rm -it --gpus=all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/workspace:/workspace --network=host nvcr.io/nvidia/nemo:24.05
# in the container
jupyter lab
# open the notebook URL in your browser

# (b) TensorRT-LLM
docker run --rm -it --gpus=all --ipc=host --ulimit memlock=-1 --ulimit stack=67108864 -v $(pwd)/workspace:/workspace --network=host nvcr.io/nvidia/tritonserver:24.05-trtllm-python-py3
# in the container
jupyter lab
# open the notebook URL in your browser
```

## Contributing

Make sure to run the following before committing:

```sh
pip install nb-clean
nb-clean clean workspace/NeMo_Training_TinyLlama.ipynb
nb-clean clean workspace/TensorRT-LLM.ipynb
nb-clean clean workspace/NeMo_Guardrails.ipynb
```

## Contributors

The code was primarily written by [Cliff](https://github.com/wcks13589), with assistance from others listed in the [contributor list](https://github.com/j3soon/LLM-Tutorial/graphs/contributors).

## Acknowledgements

We would like to thank [NVIDIA](http://www.nvidia.com/), [OpenACC](http://www.openacc.org/), and [NCHC](https://www.nchc.org.tw/?langid=2) (National Center for High-performance Computing) for making this bootcamp happen.

## References

- [openhackathons-org/End-to-End-LLM](https://github.com/openhackathons-org/End-to-End-LLM)
