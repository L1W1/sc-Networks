# sc-Networks
My internship working on single cell RNA data and creating networks to find genes that are expressed differently in e.g. healthy and diseased cells.

### Goal:
Create a workflow to go from scRNA data to gene interaction networks that represent the interactions that are highly different in e.g. healthy and diseased cells (two different trajectories as a result of pseudotime analysis). After that verify the results by computing the correlation between the gene expression. Perfom a test for permutation p-value.

### Files
##### 01_trajectories_processing.ipynb
Basic preprocessing of the sc-RNA data. Visualized the data and doing pseudotime analysis.
Marked the two different trajectories and sorted the cells either to trajectory A or trajectory B.
To try to understand what happens along those trajectories they are splitted into windows so that a cell belongs to one trajectory and one window. 
Then infer the adjacencies of the genes to each other for each window and create regulons that represent the interactions. Therefore grnboost from pyScenic (https://pyscenic.readthedocs.io/en/latest/) is used. 
Most of the steps here are taken from the scanpy tutorial "Preprocessing and clustering 3k PBMCs" (https://scanpy-tutorials.readthedocs.io/en/latest/pbmc3k.html).

##### 02_abs_matrix.ipynb
From the weights of each transcription factor gene interaction the absolute value of the difference between trajectory A and trajectory B is taken. So if a tf-gene interaction has a high difference between A and B this results in an high absolute value. Those values are stored in a matrix for each window.

##### 03_window_networks.ipynb
This script takes the matrices with the absolute values and creates a network for each window where the tf-gene interactions with the highes values are shown. So these networks show those interactions that vary the most between the two trajectories.

##### 04_combined_network.ipynb
Here all the window networks are combined so that it results in one large network where the interactions with the largest differences between the two trajectories for each window are combined.
Some of the genes are significant in more than one window.


##### 05_correlation_and_testing
Compute the correlation between two vectors of the expression matrix and compute the absolute value between them. Then do this for each pair in a window and after that for each pair in each window.
To make sure to use a useful correlation method I tested two different methods to decide which one to use in the end. I decided to go with pearsons correlation coefficient.

After that I tried to evaluate significance of correlation results with permutation test for p-value. Therefore the expression matrices were randomized and the correlation is for each tf-gene pair computed. Then it can be evaluated how significant the computed correlation the real expression of tf and gene is. For example if the correlation is very high for the real expression the random matrices would not reach a similar correlation value so no value is higher as the real correlation which means it is significant.

I had and still have a lot of problems with efficiency because computing the correlation for each tf-gene pair for a lot of randomized matrices takes a lot. With multiprocessing the needed time got less but still it is not satisfying.

### Resume
I learned a lot especially how to deal with single cell RNA data but also get to know a lot tools during this internship. There is much to improve on this project especially with the confimation of correctness of my workflow and the speed for the correlation computation. Still I had fun trying out different methods and experimenting with various packages and tools. Thanks to my supervisor for helping me through this and beeing so kind and patient with me!
