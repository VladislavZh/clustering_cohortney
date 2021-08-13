# Neural Clustering for Marked Temporal Point Processes
Here we provide the implementation of COHORTNEY.
The publication is currently under review.

## Data
We provide 17 datasets: 12 synthetic and 5 real-world. All datasets are
in the GDrive: https://drive.google.com/drive/folders/1xSjHx7SQDEefgCuAeP21NLOabIpL13XH?usp=sharing
- data/[sin_,trunc_]Kx_C5 - synthetic datasets
- data/[Age,Linkedin,IPTV,ATM,Booking] - real world datasets
All the datasets should be saved to the data/ folder.

##  Method
We use an LSTM-based model to estimate the intensity as
a piecewise constant function. The model is in 'models/LSTM.py'.

### Highlights

The ```get_partition``` function in 'utils/data_preprocessor.py' preprocesses
point processes to a format that is suitable for the LSTM

The file 'data\trainers.py' consists of the Trainer class. It conducts the model training

## Starting the experiments
To start the experiments, one needs to run the following command (e.g. for K5_C5
dataset):

```
python run.py --path_to_files data/K5_C5 --n_steps 128 --n_clusters 1
--true_clusters 5 --upper_bound_clusters 10 --random_walking_max_epoch 40
--n_classes 5 --lr 0.1 --lr_update_param 0.5 --lr_update_tol 25 --n_runs 5
--save_dir K5_C5 --max_epoch 50 --max_m_step_epoch 10 --min_lr 0.001
--updated_lr 0.001 --max_computing_size 800 --device cuda:0
```

To run all the experiments run 'start.sh' script:
```
./start.sh
```

All the results and the parameters are stored in 'experiments/[save_dir]' folder:
- 'experiments/[save_dir]/args.json' has the parameters.
- 'experiments/[save_dir]/last_model.pt' has the model.

## The results

With purity metric:

| **Category**      | **COHORTNEY**             | **DHMP**                     | **K-shape**      | **K-means**         | **K-means**      | **GMM**          |
|-------------------|---------------------------|------------------------------|------------------|---------------------|------------------|------------------|
|                   |                           |                              |                  | **partitions**      | **tsfresh**      | **tsfresh**      |
| Exp               | **0.89 &pm; $0.09**       | 0.86 &pm; 0.15               | 0.32             | 0.32                | 0.71             | 0.80             |
| Sin               | 0.85 &pm; 0.18            | 0.84 &pm; 0.13               | 0.42             | 0.42                | **0.89**         | 0.80             |
| Trunc             | **0.97 &pm; 0.04**        | 0.96 &pm; 0.06               | 0.32             | 0.32                | 0.44             | 0.45             |
| Real              | **0.44 &pm; 0.12**        | 0.00 &pm; 0.00               | 0.32             | 0.32                | 0.35             | 0.35             |
| Nr. of wins       | **3**                     | 0                            | 0                | 0                   | 1                | 0                |

Average running time (seconds):

| **Dataset**       | **COHORTNEY**      | **DHMP**                     | **Dataset**      | **COHORTNEY**      | **DHMP**                     |
|-------------------|--------------------|------------------------------|------------------|--------------------|------------------------------|
| Exp               | **4149**           | 98626                        | Sin              | **2432**           | 175721                       |
| Trunc             | **2076**           | 65277                        | Real             | **3677**           | 62288                        |
| Nr. of wins       | 2                  | 0                            |                  | 2                  | 0                            |
