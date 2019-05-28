# MMR-Universal

This is the code relative to the paper
<p align="left"><img src="https://raw.githubusercontent.com/fra31/mmr-universal/master/images/pl_gh_2.png" width="800">

**Francesco Croce, Matthias Hein**  
University of Tübingen

[https://arxiv.org/abs/1905.11213](https://arxiv.org/abs/1905.11213)

## Main idea
We introduce a regularization scheme which aims at expanding the linear regions of ReLU-networks in both L1- and Linf-sense.
We show that in this way we are able to achieve *simultaneously provable robustness wrt all the
Lp-norms for p>=1*.

We compute the largest Lp-balls contained, first, in the union of an L1- and an Linf-ball and, second, in the convex hull of that
union, noticing that the latter is significatly larger than the former.

<p align="center"><img src="https://raw.githubusercontent.com/fra31/mmr-universal/master/images/pl_gh_1.png" width="800">

Then, exploiting this observation, we extend the *Maximum Margin Regularizer* of [(Croce et al, 2019)](https://arxiv.org/abs/1810.07481) to our new MMR-Universal, which provides
models which are provably robust according to the current
state-of-the-art certification methods based on [Mixed Integer Programming](https://arxiv.org/abs/1711.07356)
or its [LP-relaxations](https://arxiv.org/abs/1711.00851).

All the models trained with MMR-Universal and reported in the paper can be found in the folder `models`.

## Training MMR-Universal models

To train a CNN with MMR-Universal:

`python train.py --dataset=mnist --p=univ --gamma_l1 1.0 --gamma_linf 0.15
--lmbd_l1 3.0 lmbd_linf 12.0 --nn_type=cnn_lenet_small --exp_name=cnn_mmr_univ`

Note that this is an extension of the [MMR implementation](https://github.com/max-andr/provable-robustness-max-linear-regions),
so it is possible to train MMR models wrt a single norm with\
`--p 2` or `--p inf`.

More details about the parameters available in `train.py`.

## Evaluation

`eval.py` combines multiple methods to calculate empirical and provable robust error:

`python eval.py --n_test_eval=100 --p=inf --dataset=mnist --nn_type=cnn_lenet_small --model_path=/path/to/model.mat`

The supported norms are `--p inf`, `--p 2` and `--p 1`. Note that at the moment the evaluation of empirical
robustness wrt the L1-norm is not integrated.

## Requirements
All main requirements are collected in the Dockerfile.
The only exception is `MIPVerify.jl` and Gurobi (free academic licenses are available). 
For this, please use the 
[installation instructions](https://vtjeng.github.io/MIPVerify.jl/latest/#Installation-1)
provided by the authors of `MIPVerify.jl`. But note that Gurobi with a free academic license cannot 
be run from a docker container.

Also note that we use our own forks of `kolter_wong` ([Wong et al, 2018](https://arxiv.org/abs/1711.00851)) and 
`MIPVerify.jl` ([Tjeng et al, 2018](https://arxiv.org/abs/1711.07356)) libraries. 
And that in `attacks.py` we redefine the class `MadryEtAl` of `Cleverhans` library to support 
normalized L2 updates for the PGD attack.

## Citations
```
@unpublished{crocehein2019multiplenorms,
  title={Provable robustness against all adversarial $l_p$-perturbations for $p\geq 1$},
  author={Croce, Francesco and Hein, Matthias},
  note={preprint, arXiv:1905.11213},
  year={2019}
}
```
