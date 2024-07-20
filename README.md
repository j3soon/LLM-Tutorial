# LLM Tutorial

LLM tutorial materials include but not limited to NVIDIA NeMo, TensorRT-LLM, Triton Inference Server, and NeMo Guardrails.

This material is used in the [NCHC LLM Bootcamp](https://www.openhackathons.org/s/siteevent/a0CUP00000L45Bx2AJ/se000345).

## Running on TWCC

### Create Container

1. Visit TWCC (<https://www.twcc.ai>) and click `Sign In`.
   ![](docs/images/01-home.png)
2. Enter your email and password, and then login through iService.
   ![](docs/images/02-login.png)
3. You should be re-directed to the user dashboard (<https://www.twcc.ai/user/dashboard>) after sign in.
   ![](docs/images/03-dashboard.png)
4. Click the top-left dropdown and select the 「國網教育訓練用計畫」 instead of other wallets.  
   **WARNING**: This is an important step to avoid charges on your other wallets.
   ![](docs/images/04-project.png)
5. Click the second dropdown: `Services > Interactive Container`.
   ![](docs/images/05-services.png)
6. And click `Create`.
   ![](docs/images/06-interactive-container.png)
7. Search for `nemo` or `triton` and click it, depending on which tutorial you want to run.
   ![](docs/images/07_1-nemo-container.png)
   ![](docs/images/07_2-triton-container.png)
8. Select container image `nemo-24.05:latest` or `tritonserver-24.05-trtllm-python-py3:latest`, depending on which tutorial you want to run, and then scroll down.
   ![](docs/images/08-container-image.png)
9. Select `c.super` (V100 GPU x1) for the configuration type, and click `REVIEW & CREATE`
   ![](docs/images/09-container-gpu.png)
10. Confirm again that you are using the correct wallet 「國網教育訓練用計畫」, and click `CREATE`.
    ![](docs/images/10-container-warning.png)
11. Wait for the container to be initialized and ready. You can click `REFRESH` to check the status after a few minutes.  
    **WARNING**: After finishing the tutorial, make sure to check the container and click `DELETE` to avoid using up all your credits.
    ![](docs/images/11-container-initializing.png)
12. When the container shows `Ready`, click the container name to enter the details page.
    ![](docs/images/12-container-ready.png)
13. Scroll down the container details page.
    ![](docs/images/13-container-details.png)
14. Click the `LAUNCH` button in the `Jupyter` row to open the Jupyter Notebook.
    ![](docs/images/14-container-jupyter.png)
15. Click `New` and then `Terminal` to open a terminal.
    ![](docs/images/15-jupyter.png)
    ![](docs/images/16-jupyter-new.png)
16. You can now run commands in the terminal.
    ![](docs/images/17-jupyter-terminal.png)

Related video: [TWCC 開發型容器-基本操作](https://youtu.be/LhqBdJK0PrY)

### Environment Setup

In the terminal, run:

```sh
# Clone the repository and link the workspace
cd ~
git clone https://github.com/j3soon/LLM-Tutorial
sudo rm -rf /workspace
sudo ln -s $PWD/LLM-Tutorial/workspace /.
# Change ownership for NeMo 24.05, which is required when patching it later in the notebook
sudo chown -R $(id -u):$(id -g) /opt/NeMo
# All done! Go back to Jupyter Notebook / Jupyter Lab
```

**Note**: To paste text in the jupyter terminal webpage, press `Ctrl+Shift+V`. To copy text, select the text, right-click, and choose `Copy`.

You should now see the `LLM-Tutorial` folder in the Jupyter file browser.

![](docs/images/18-jupyter-files.png)

Navigate to `LLM-Tutorial/workspace` and open the notebook you want to run.

### Running the Notebook

Since these notebooks require a lot of time to run, we recommend you to first click `Cell > Run All` to run all cells in the notebook. Then, you can start going through the notebook.

### Deleting the Container

After you have finished the tutorial, make sure to delete the container to avoid using up all your credits.

The container list should be empty:

![](docs/images/06-interactive-container.png)

## Frequently Asked Questions (FAQ)

### Unexpected Errors During pip Install

If you have been using TWCC in the past, you may encounter unexpected errors during `pip install`. This is because TWCC mounts the user home directory automatically for ease of development. This will cause the package installed by `pip` to be stored under the `~/.local` directory. You can back up the `.local` directory and remove it:

```sh
mv ~/.local ~/.local.bak
```

### Disk Quota Exceeded

This may happen if you are using TWCC in the past and have somehow end up with zero disk quota.

Example error message:

```
~$ git clone https://github.com/j3soon/LLM-Tutorial
Cloning into 'LLM-Tutorial'...
error: copy-fd: write returned: Disk quota exceeded
fatal: cannot copy '/usr/share/git-core/templates/hooks/fsmonitor-watchman.sample' to '/home/uXXXXXXX/LLM-Tutorial/.git/hooks/fsmonitor-watchman.sample': Disk quota exceeded
```

Click `VIEW DETAILS` in the user dashboard and check if the `Total Storage` quota is below `100GiB`. If so, you indeed stumbled upon the disk quota issue. The `HFS Portal` should show similar results.

![](docs/images/faq-disk-quota-issue-1.png)
![](docs/images/faq-disk-quota-issue-2.png)

This issue happens rarely and we're currently working with TWCC to resolve it.

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
```

## Contributors

The code was primarily written by [Cliff](https://github.com/wcks13589), tested by [Yang-Hsien](https://github.com/y-h-Lin), and documented by [Johnson](https://github.com/j3soon), with assistance from others listed in the [contributor list](https://github.com/j3soon/LLM-Tutorial/graphs/contributors).

## Acknowledgements

We would like to thank [NVIDIA](http://www.nvidia.com/), [OpenACC](http://www.openacc.org/), and [NCHC](https://www.nchc.org.tw/?langid=2) (National Center for High-performance Computing) for making this bootcamp happen.

## References

- [openhackathons-org/End-to-End-LLM](https://github.com/openhackathons-org/End-to-End-LLM)