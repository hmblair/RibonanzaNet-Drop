A repository for replicating the results and performing inference on the Ribonanza
model fine-tuned for sequence dropout prediction during chemical mapping experiments,
as outlined in the paper _Ribonanza: deep learning of RNA structure through dual crowdsourcing_. 

To install, clone the repository and install the requirements via
```
git clone https://github.com/hmblair/DropoutPrediction
cd DropoutPrediction
pip3 install -r requirements.txt
```

To use the RNA Foundation Model, it must be installed separately, via
```
pip3 install -q --user -v --no-dependencies rna-fm 
pip3 install -q --user ptflops
```

In order to replicate the results from the paper, you will need the train data 
and pre-trained models. These can be found at https://drive.google.com/drive/folders/14bBVz0CLkzt4AG9w5YPr_pWVsWT53HDl?usp=share_link;
the data should be placed in ./data, and the pre-trained models in ./pretrained/checkpoints.
The utils.py file contains functions for loading the predictions made by the
models, as well as computing the metrics provided in the supplemental table. 

The training is handled by the PyTorch Lightning CLI, which is configured using
YAML files. Example files for performing training and inference can be found
under ./examples/phase, and files for specifying the hyperparameters of the
pre-trained models can be found under ./examples/model. To run training, you
should pass one of each file to \_\_main__.py, such as by calling 
```
python3 __main__.py fit \
    --config examples/phase/fit.yaml \
    --config examples/model/lstm.yaml
```
Running inference is similar, except you should pass the model checkpoint as well. 
```
python3 __main__.py predict \
    --config examples/phase/predict.yaml \
    --config examples/model/lstm.yaml \
    --ckpt_path checkpoints/lstm.ckpt
```

This repo works with .nc files, which are like .csv files but can store multidimensional
data. Interfacing with them can be done with xarray, which behaves similarly
to pandas. If you want to run inference on your own data, you should use the function 
fasta_to_nc in utils.py to convert your data from .fasta to .nc, and then create 
a new YAML file like those under examples/phase/predict.yaml, with predict_paths
modified to include the path of the .nc file generated by the former process.