# megaDNA: A long-context language model for the generation of bacteriophage genomes
![Online_figure](https://github.com/lingxusb/megaDNA/assets/12596418/ef85a641-0a79-4232-9d09-4abf498f04be)

Generative pre-trained transformers (GPTs) have revolutionized the field of natural language processing. Inspired by the success of large language models, we develop a long-context generative model for genomes. Our multiscale transformer model was pre-trained on unannotated bacteriophage genomes with byte-level tokenization. We demonstrate the foundational capabilities of our model including the prediction of essential genes, genetic variant effects, regulatory element activity and taxonomy of unannotated sequences. Furthermore, it generates de novo sequences up to 96K base pairs, which contain functional regulatory elements and novel proteins with phage-related functions.

### Install
To install `megaDNA`, run the following bash script:
 ```bash
 git clone https://github.com/lingxusb/megaDNA.git
 cd megaDNA
 pip install .
 ```

### Trained model
The trained 145M model is availale at [huggingface](https://huggingface.co/lingxusb/megaDNA_updated/tree/main)

### Sequence generation
```python
 import torch
 
 # load the model

 device = 'cpu' # use 'cuda' for GPU
 model = torch.load(model_path, map_location=torch.device(device))

 # new sequences can be generated by a primer sequence:

 nucleotides = ['**', 'A', 'T', 'C', 'G', '#'] # vocabulary

 seq_tokenized = model.generate(primer_sequence,
                                seq_len=context_length,
                                temperature=0.95, 
                                filter_thres=0.0)

 # To transform tokens back to DNA ucleotide sequence:
 def token2nucleotide(s):
     return nucleotides[s]
 generated_sequence = ''.join(map(token2nucleotide, seq_tokenized.squeeze().cpu().int()))
 ```

Please check our jupyter notebook: [megaDNA_generate.ipynb](https://github.com/lingxusb/megaDNA/blob/main/notebook/megaDNA_generate.ipynb). GPU recommended.

Or you can easily run the [Colab Notebook](https://colab.research.google.com/drive/1T7pDY-pL2aJk8mogUKhDu5DpG9r7bjv4?usp=sharing) in the browser. Please make sure to connect to a GPU instance (e.g. T4 GPU).

### Model embeddings and loss
```python
# a random input sequence
encoded_sequence = np.random.choice(np.arange(1,5), 100)
input_seq = torch.tensor(encoded_sequence).unsqueeze(0).to(device) 

# get embeddings
output = model(input_seq, return_value = 'embedding')

# output[0:3] stores embeddings from three transformer layers.

# get model loss
output = model(input_seq, return_value = 'loss')

print(output)
```

### Reference
- [A long-context language model for deciphering and generating bacteriophage genomes](https://www.biorxiv.org/content/10.1101/2023.12.18.572218v3)
- [MEGABYTE: Predicting Million-byte Sequences with Multiscale Transformers](https://arxiv.org/abs/2305.07185)
- [MEGABYTE-pytorch by Phil Wang](https://github.com/lucidrains/MEGABYTE-pytorch)
- Please contact shaobinlx@gmail.com or raise an issue in the github repo with any questions.
