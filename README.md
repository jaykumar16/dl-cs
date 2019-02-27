# Compressed Sensing: From Research to Clinical Practice with Data-Driven Learning

## Setup

This project uses the open source python toolbox `sigpy` for generating sampling masks, estimating sensitivity maps, and other MRI specific operations. The functionality of `sigpy` can be replaced with the `bart` toolbox.

Install the required python packages (tested with python 3.6 on Ubuntu 16.04LTS):

```bash
pip install -r requirements.txt
```

Fully sampled datasets can be downloaded from <http://mridata.org> using the python script and text file.

```bash
python3 data_prep.py --verbose mridata_org.txt
```

The download and pre-processing will take some time! This script will create a `data` folder with the following sub-folders:

* `raw/ismrmrd`: Contains files with ISMRMRD files directly from <http://mridata.org>
* `raw/npy`: Contains the data converted to numpy format, [npy](https://www.numpy.org/devdocs/reference/generated/numpy.lib.format.html)
* `tfrecord/train`: Training examples converted to TFRecords
* `tfrecord/validate`: Validation examples converted to TFRecords
* `tfrecord/test`: Test examples converted to TFRecords
* `test_npy`: Sub-sampled volumetric test examples in `npy` format
* `masks`: Sampling masks generated using `sigpy` in `npy` format

All TFRecords contain fully sampled raw k-space data and sensitivity maps. The sensitivity maps were estimated using Ying and Sheng's [JSENSE](https://onlinelibrary.wiley.com/doi/full/10.1002/mrm.21245) implemented in `sigpy`.

## Training

The training can be performed using the following command.

```bash
python3 --model_dir summary/model recon_train.py
```

All the parameters (dimensions and etc) assume that the training is performed with the knee datasets from mridata.org. See the `--help` flag for more information on how to adjust the training for new datasets.

For convenience, the training can be performed using the bash scripts.

```bash
bash train_l1.sh # train with l1 loss
bash train_l2.sh # train with l2 loss
bash train_adv.sh # train with adversarial loss by warm starting with l1 loss
```

## Inference

For file input `kspace_input.npy`, the data can be reconstructed with the following command. The test data in `data/test_npy` can be used to test the inference script.

```bash
python3 recon_run.py summary/model kspace_input.npy kspace_output.npy
```

## References

1. `sigpy`: https://github.com/mikgroup/sigpy
1. `bart`: https://github.com/mrirecon/bart