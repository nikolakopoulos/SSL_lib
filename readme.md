__GENERAL INFO__

If you are using our code, please cite our papers:

D. Berberidis, A. N. Nikolakopoulos and G. B. Giannakis, "Adaptive Diffusions for Scalable Learning Over Graphs," in IEEE Transactions on Signal Processing, vol. 67, no. 5, pp. 1307-1321, March1, 2019.
doi: [10.1109/TSP.2018.2889984](https://doi.org/10.1109/TSP.2018.2889984)

D. Berberidis, A. N. Nikolakopoulos and G. B. Giannakis, "AdaDIF: Adaptive Diffusions for Efficient Semi-supervised Learning over Graphs," 2018 IEEE International Conference on Big Data (Big Data), Seattle, WA, USA, 2018, pp. 92-99.
doi: [10.1109/BigData.2018.8622130](https://doi.org/10.1109/BigData.2018.8622130)

__OVERVIEW__

__SSL__ implements and runs tests for different __semi-supervised learning__ methods on __multiclass or multilabel__ graphs with available ground-truth labels. 

Two modes available:
- __test__: Takes as input a graph and labels over all nodes. Randomly sumples a number of nodes (`num_seeds`) and predicts the labels of teh remaining ones. Experiments are repeated for a predefined number of times (`num_iters`) and the mean Micro F1 and Macro F1 scores are reported.
- __predict__: This is the operational mode. A graph is given and a file with a subset of nodes and its labels. The selected method is implemented and the predicted labels over all the nodes of the graph in a predefined (`outfile`) output file. 		

Methods included:
- PPR: Personalized PageRank
- __TunedRwR__: Tuned random walk with restarts ( see [here](https://experts.umn.edu/ws/portalfiles/portal/99184908)  )
- __AdaDIF__: Adaptive Diffusions for Efficient Semi-supervised Learning over Graphs ( see [here](https://arxiv.org/abs/1804.02081) )




__INPUT FILES FORMAT__

__SSL__ loads the graph in __adjacency list__ format from a `.txt` file that contains edges as tab separated pairs of node indexes in the format: `node1_index \tab node2_index`. Node indexes should be in range `[1 , 2^64 ]`. 

For __multiclass__ graphs, the labels are loaded from a `.txt` file where each line is of the format: `node_index \tab label` . Labels have to be integers in `[-127,127]`. 

For __multilabel__ graphs, labels are loaded from a `.txt` file in compressed __one-hot-matrix__ form (see `graphs/HomoSapiens/class.txt` for example).

when in __test__ mode, all nodes must be labeled (present in the label file). 

When in __predict__(ion) mode, any subset of nodes can be labeled.  

__OUTPUT FILES FORMAT__

- __Multiclass__: Similar to input, each line is `node_index \tab predicted_label`
- __Multilabel__: The output for multilabel graphs is a ranking for every node. Each line follows the format `node_index: \tab pred_1 pred_2 ... pred_c`, where `pred_i` is the i-th most probable label for this node.

__COMPILATION__

Dependencies: `blas` and `pthread`  must be installed

Command line: `make clean` and then `make`

__EXECUTION__
		      	 
Command line: `./SSL [OPTIONS]`

__OPTIONS__

Command line optional arguments with values:

ARGUMENT | VALUES | DEFAULT | DESCRIPTION
-------- | ------ | ------- | -----------
`--mode` |  `test` <br/> `predict`| `test` | Operational mode (see Overview)    	
`--method` |  `Tuned_RwR` <br/> `AdaDIF` <br/> `PPR`| `AdaDIF` | Selection of prediction method (see Overview)   	
`--graph_file` | (adjacency list)`.txt` | `graphs/BlogCatalog/adj.txt` | See Input Files Format
`--label_file` | (label list or one-hot)`.txt` | `graphs/BlogCatalog/class.txt` | See Input Files Format
`--outfile` | (predicted labels)`.txt` | `out/label_predictions.txt` | File where predictions are stored when in `--mode = __predict__` (see Output Files Format)
`--num_seeds` | `[1, 2^16]` | `1030` | Number of nodes that are labeled ( only works when `--mode = __test__` )
`--walk_length` | `[1, 2^16]` | `10` | Length of AdaDIF (and/or PPR) random walk.
`--lambda_trwr` | `>=0.0` | `1.0` | Regularization parameter for Tuned RwR method 
`--lambda_addf` | `>=0.0` | `5.0` | Smoothness over the graph regularization parameter for AdaDIF method  
`--num_iters` | `[1, 2^16]` | `1` | Number of experiments performed ( only works when `--mode = __test__` )

Default values can be changed by editing `defs.h`

Command line optional arguments without values:

ARGUMENT | RESULT
-------- | ------
`--unconstrained` | switches AdaDIF to unconstrained mode
`--single_thread` | forces single thread execution
`--multiclass` | specifies multiclass input / output (default is multilabel)















