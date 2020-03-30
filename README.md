# Domain Randomization

### Code Instructions

Cannot set up docker, use python virtual environments for now...

Hopper example:

```bash
conda activate adaptivedr
mpirun -np 30 python scripts/ppo_pytorch.py --backend mujoco --env_dist_stdev 1.0 --num_elites 10 --pop_size 30 --env_name Hopper --seed 0
```

Walker example:

```bash
conda activate adaptivedr
mpirun -np 30 python scripts/ppo_pytorch.py --backend mujoco --env_dist_stdev 1.0 --num_elites 10 --pop_size 30 --env_name Walker --seed 0
```

To run the experiment on the Walker environment with initial seed 0, please do:

```bash
$ docker run -v  $(pwd):/root/work/domain-randomization -v <absolute path to .mujoco folder>:/root/.mujoco -it sharadmv/domain-randomization:pytorch mpirun -np 30 pipenv run python scripts/ppo_pytorch.py --env_dist_stdev 1.0 --seed 0
```

During training, you can view the hyper-parameters of the run and training progress by opening up tensorboardX:

```bash
$ tensorboard --logdir runs
```

### TODO

- Add code to visualize results

### Contributors

[Sharad Vikram](https://github.com/sharadmv)
