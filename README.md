![INFORMS Journal on Computing Logo](https://INFORMSJoC.github.io/logos/INFORMS_Journal_on_Computing_Header.jpg)

# Binary Quantum Control Optimization with Uncertain Hamiltonians

This project is distributed in association with the [INFORMS Journal on
Computing](https://pubsonline.informs.org/journal/ijoc) under the [MIT License](LICENSE).

The software and data in this repository are associated with the paper 
[Binary Quantum Control Optimization with Uncertain Hamiltonians](https://doi.org/10.1287/ijoc.2024.0560) 
by Xinyu Fei, Lucas T. Brady, Jeffrey Larson, Sven Leyffer, Siqian Shen.

## Cite

To cite the contents of this repository, please cite both the paper and this
repo, using their respective DOIs.

https://doi.org/10.1287/ijoc.2024.0560

https://doi.org/10.1287/ijoc.2024.0560.cd

Below is the BibTex for citing this snapshot of the repository.

```
@misc{Huang2024,
  author =        {Fei, Xinyu and Brady, Lucas T and Larson, Jeffrey and Leyffer, Sven and Shen, Siqian},
  publisher =     {INFORMS Journal on Computing},
  title =         {{Binary Quantum Control Optimization with Uncertain Hamiltonians}},
  year =          {2024},
  doi =           {10.1287/ijoc.2024.0560.cd},
  url =           {https://github.com/INFORMSJoC/2024.0560},
  note =          {Available for download at https://github.com/INFORMSJoC/2024.0560},
}
```

## Description

The goal of this software is to implement the gradient-based and rounding algorithms proposed in the paper 
to solve the binary quantum control optimization problem with uncertain Hamiltonians.

## Building
### Prerequisites
* Python >= 3.8
* qiskit >= 0.29.0, scipy >= 1.6.2, qutip >= 4.6

### Installation Steps
1. Clone the repository
2. Set up a virtual environment (optional)
3. Install the above dependencies by 
```
pip install qiskit scipy qutip
```

## Important modules
* ```example```: different quantum control examples
* ```script```: testing script examples
* ```utils```: utility functions
* ```paper_figures```: figures in the paper


## Usage
### Parameter lists
**Required parameters**
* ```--name```: name of the instance
* ```--n```: number of qubits
* ```--evo_time```: evolution time
* ```--n_ts```: number of time steps
* ```--initial_type```: initial type of the control time intervals, including 'warm', 'average', and 'random'
  * 'warm': the initial control time intervals are set as the value of obtained discretized binary control intervals
  * 'average': the initial control time intervals are set as evolution time / number of intervals
  * 'random': the initial control time intervals are set as random values between 0 and evolution time
* ```--c_control```: file path of the discretized continuous control
* ```--num_scenario```: number of sampled scenarios for stochastic optimization model
* ```--cvar```:  CVaR penalty parameter
* ```--var```: variation of Hamiltonian noise, can be a list of values
* ```--binary```: whether to solve the binary control problem
* ```--weight```: weight parameter balancing risk-neutral and risk-averse measurements
* ```--ts_b```: time steps for rounding method

**Energy minimization problem only parameters**
* ```--num_edge```: number of edges in the randomly generated graph for Hamiltonian controllers
* ```--rgraph```: whether to generate a random graph for Hamiltonian controllers 
* ```--seed```: random seed for generating the random graph

**Molecule compilation problem only parameters**
* ```--molecule```: molecule name, including 'BeH2', 'LiH', 'H2'
* (Optional) ```--lr```: learning rate for the gradient-based optimization algorithm


### Stored results
All the control results are stored in the folder ```result/control/```. 
All the output control figures are stored in ```result/figure/```. 
The output files are stored in ```result/output/```. 
One can change the 
paths in files to change the positions. 

**Before starting your own experiments, we suggest deleting the above three folders to clear all the existing results.** 

### Example data files
* Target unitary operator for circuit compilation problem (input as parameter ```result/target```).

Required files should be **.csv** files. 

### Usage examples
To run an energy minimization problem with 6 qubits, randomly generated graph for Hamiltonian controllers, 
evolution time 5, time steps 50, scenarios 300, CVaR penalty 0.05, variation of Hamiltonian noise 0.01,
weight parameter 0.5, and rounding time steps 200:
```shell
python example/StochasticModel/energy_ratio.py --name='EnergyRevision' --n=6 --num_edges=3 --rgraph=1 --g_seed=1 --evo_time=5 --n_ts=50 --r_seed=0 \
  --initial_type='CONSTANT' --offset=0.5 --num_scenario=300 --cvar=0.05 --var=0.01 --binary=1 --weight=0.5 --ts_b=200
```
To run an out of sample test for energy minimization problem with 10 groups, each having 500 scenarios:
```shell
python example/OutSampleTest/energy_ratio.py --n=6 --num_edges=3 --rgraph=1 --g_seed=1 --evo_time=5 --n_ts=50 --r_seed=1000 --group=10 --num_scenario=500 \
  --var=0.01 --cvar=0.05 --draw=0 --control="../../result/control/Direct/Energy6_evotime5.0_n_ts50_ptypeCONSTANT_offset0.5_instance1_scenario300_cvar0.05_mean0_var0.01_weight1.0.csv"
```
To run circuit compilation problem on molecule H2 with evolution time 20, time steps 50, scenarios 20, 
CVaR penalty 0.05, variation of Hamiltonian noise 0.01, weight parameter 0.5, learning rate 0.05, and rounding time steps 4000:
```shell
python example/StochasticModel/Molecule.py --name="Molecule" --molecule="H2" --qubit_num=2 --evo_time=20 --n_ts=50 --r_seed=0 --gen_target=0 \
   --target="../../result/target/Molecule_H2_target.csv" --sum_penalty=1 --max_iter=2000 \
   --initial_type="CONSTANT" --offset=0.2 --num_scenario=20 --cvar=0.05 --var=0.01 --binary=1 --ts_b=4000 --weight=0.5 --lr=0.05
```
To run an out of sample test for circuit compilation problem with 10 groups, each having 500 scenarios:
```shell
python example/OutSampleTest/Molecule.py --name="Molecule" --molecule="H2" --qubit_num=2 --evo_time=20 --n_ts=50 --r_seed=1000 --gen_target=0 \
  --target="../../result/target/Molecule_H2_target.csv" --group=10 --num_scenario=500 --cvar=0.05 --var=0.05 --draw=0 \
  --control="../../result/control/Stochastic/Molecule_H2_evotime20.0_n_ts50_ptypeCONSTANT_offset0.2_sum_penalty1.0_scenario20_cvar0.05_mean0_var0.05_weight0.5.csv"
```

## Acknowledgement
We thank Dr. Lucas Brady for providing the code used in the paper [**Optimal Protocols in Quantum Annealing and 
QAOA Problems**](https://arxiv.org/pdf/2003.08952.pdf).

We refer to the paper [**Partial Compilation of Variational Algorithms for 
Noisy Intermediate-Scale Quantum Machines**](https://arxiv.org/pdf/1909.07522.pdf) for generating the 
circuit compilation problem. Their code is presented at https://github.com/epiqc/PartialCompilation.


## Developers
Xinyu Fei (xinyuf@umich.edu)

## Contact
Xinyu Fei (xinyuf@umich.edu)

Siqian Shen (siqian@umich.edu)