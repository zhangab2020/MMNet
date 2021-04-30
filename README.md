## Morphology-Molecule Network(MMNet)

### Introduction

Our main contribution is to propose a novel algorithm to improve the accuracy of species identification by combining molecular and morphological information.


### Installation (You can install it through requirements.txt):
* Python 3.6.6 ([Anaconda](https://www.anaconda.com/products/individual) is recommended)
* torch (1.2.0)
* torchvision (0.3.0)
* visdom (0.1.8.8)

### Data preprocess:
- Prepare an aligned FASTA file like:
  ```console
  >BCIBT1645-13,Urbanus esmeraldus
  aacttt-atattttat-ttttggaat-ttgagcag...
  ```
  Note that the separator is a comma.
  
  `>ID,species name`
  

- Change the file name in `DataPreprocess/preprocessFASTA2csv.py` according to the FASTA file and run the script

- And the output file looks like:


| **idx**                            | **seqs**    | **id**       | **name**             | **label** |
|:----------------------------------:|:-----------:|:------------:|:--------------------:|:---------:|
| >FBCOB308-10,Adalia bipunctata     | aacattatat… | FBCOB308-10  | Adalia bipunctata    | 0         |
| >COLFE1380-13,Adalia bipunctata    | aacattatat… | COLFE1380-13 | Adalia bipunctata    | 0         |
| >COLFF981-13,Adalia bipunctata     | aacattatat… | COLFF981-13  | Adalia bipunctata    | 0         |
| >COLFE416-12,Adalia bipunctata     | aacattatat… | COLFE416-12  | Adalia bipunctata    | 0         |
| >FBCOB309-10,Adalia bipunctata     | aacattatat… | FBCOB309-10  | Adalia bipunctata    | 0         |
| >FBCOF1053-12,Adalia decempunctata | aacattatat… | FBCOF1053-12 | Adalia decempunctata | 1         |
| >COLFF267-13,Adalia decempunctata  | aacattatat… | COLFF267-13  | Adalia decempunctata | 1         |
| >COLFF266-13,Adalia decempunctata  | aacattatat… | COLFF266-13  | Adalia decempunctata | 1         |
| >COLFE085-12,Adalia decempunctata  | aacattatat… | COLFE085-12  | Adalia decempunctata | 1         |
| >FBCOA277-10,Adalia decempunctata  | aacattatat… | FBCOA277-10  | Adalia decempunctata | 1         |
| >COLFA615-12,Anatis ocellata       | aacattatat… | COLFA615-12  | Anatis ocellata      | 2         |
| >COLFD685-12,Anatis ocellata       | aacattatat… | COLFD685-12  | Anatis ocellata      | 2         |
| >COLFD686-12,Anatis ocellata       | aacattatat… | COLFD686-12  | Anatis ocellata      | 2         |
| >COLFE438-12,Anatis ocellata       | aacattatat… | COLFE438-12  | Anatis ocellata      | 2         |
| >COLFB032-12,Anatis ocellata       | aacattatat… | COLFB032-12  | Anatis ocellata      | 2         |


- Add the path of pictures in the rightmost column, then it looks like:


| **idx**                            | **seqs**    | **id**       | **name**             | **label** | **img**                 |
|:----------------------------------:|:-----------:|:------------:|:--------------------:|:---------:|:-----------------------:|
| >FBCOB308-10,Adalia bipunctata     | aacattatat… | FBCOB308-10  | Adalia bipunctata    | 0         | /Path/To/Images/xxx.jpg |
| >COLFE1380-13,Adalia bipunctata    | aacattatat… | COLFE1380-13 | Adalia bipunctata    | 0         | /Path/To/Images/xxx.jpg |
| >COLFF981-13,Adalia bipunctata     | aacattatat… | COLFF981-13  | Adalia bipunctata    | 0         | /Path/To/Images/xxx.jpg |
| >COLFE416-12,Adalia bipunctata     | aacattatat… | COLFE416-12  | Adalia bipunctata    | 0         | /Path/To/Images/xxx.jpg |
| >FBCOB309-10,Adalia bipunctata     | aacattatat… | FBCOB309-10  | Adalia bipunctata    | 0         | /Path/To/Images/xxx.jpg |
| >FBCOF1053-12,Adalia decempunctata | aacattatat… | FBCOF1053-12 | Adalia decempunctata | 1         | /Path/To/Images/xxx.jpg |
| >COLFF267-13,Adalia decempunctata  | aacattatat… | COLFF267-13  | Adalia decempunctata | 1         | /Path/To/Images/xxx.jpg |
| >COLFF266-13,Adalia decempunctata  | aacattatat… | COLFF266-13  | Adalia decempunctata | 1         | /Path/To/Images/xxx.jpg |
| >COLFE085-12,Adalia decempunctata  | aacattatat… | COLFE085-12  | Adalia decempunctata | 1         | /Path/To/Images/xxx.jpg |
| >FBCOA277-10,Adalia decempunctata  | aacattatat… | FBCOA277-10  | Adalia decempunctata | 1         | /Path/To/Images/xxx.jpg |
| >COLFA615-12,Anatis ocellata       | aacattatat… | COLFA615-12  | Anatis ocellata      | 2         | /Path/To/Images/xxx.jpg |
| >COLFD685-12,Anatis ocellata       | aacattatat… | COLFD685-12  | Anatis ocellata      | 2         | /Path/To/Images/xxx.jpg |
| >COLFD686-12,Anatis ocellata       | aacattatat… | COLFD686-12  | Anatis ocellata      | 2         | /Path/To/Images/xxx.jpg |
| >COLFE438-12,Anatis ocellata       | aacattatat… | COLFE438-12  | Anatis ocellata      | 2         | /Path/To/Images/xxx.jpg |
| >COLFB032-12,Anatis ocellata       | aacattatat… | COLFB032-12  | Anatis ocellata      | 2         | /Path/To/Images/xxx.jpg |


- Change the csv name in `DataPreprocess/seq-img2database.py` and run the script. The database of output could be found in the directory of `data`.

### Usage:
To test our algorithm, please follow these steps (tested on Ubuntu 16.04):

1 . Install the dependent packages
```console
pip install -r requirements.txt
```

2 . Modify the `main.py` file in the directory (Set the path of training database and adjust hyperparameters according to your server). After you have modified the `main.py` file, please run the code as follows.

(Note: 0, 1, 2 use graphics cards No. 0, No. 1 and No. 2.
If you run **python train.sh** directly, you use all graphics cards by default.)
```console
CUDA_VISIBLE_DEVICES=0,1,2 python main.py
```

3 . The final result will be saved in the `output` folder.



