# Singularity recipe for fetalReconstruction software

Making it easier to start using [https://github.com/bkainz/fetalReconstruction/](https://github.com/bkainz/fetalReconstruction/) on e.g. HPC.

[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/3068)


## Basic usage

The quickest way to start using fetalReconstruction via this Singularity image 
is to pull the image from the SingularityHub on-line repository:

```bash
export SINGULARITY_CACHEDIR="/home/$USER/singularity_cache"
mkdir -p "${SINGULARITY_CACHEDIR}"
singularity pull --name fetalReconstruction.sif shub://willfurnass/fetalReconstruction_singularity:latest 
```

You are then able to run commands that live 'within' a fetalSingularity 'container' e.g.

```bash
singularity exec --nv "${SINGULARITY_CACHEDIR}/fetalReconstruction.sif" /usr/local/bin/PVRreconstructionGPU  # or
singularity exec --nv "${SINGULARITY_CACHEDIR}/fetalReconstruction.sif" /usr/local/bin/SVRreconstructionGPU 
```

## Example of a Grid Engine job using test data

The following is an example 

```bash
#!/bin/bash
#$ -l gpu=1
#$ -l h_rt=04:00:00

set -eu

export SINGULARITY_CACHEDIR="/fastdata/$USER/singularity_cache"
mkdir -p "${SINGULARITY_CACHEDIR}"
singularity pull --name fetalReconstruction.sif shub://willfurnass/fetalReconstruction_singularity:latest

# Path to test data _inside_ the fetalReconstruction image
export DATADIR=/opt/fetalReconstruction/data 

# Run fetalReconstruction test
singularity exec --nv "${SINGULARITY_CACHEDIR}/fetalReconstruction.sif" /opt/fetalReconstruction/bin/linux64/SVRreconstructionGPU \
    -o 3TReconstruction.nii.gz \
    -i $DATADIR/14_3T_nody_001.nii.gz \
       $DATADIR/10_3T_nody_001.nii.gz \
       $DATADIR/21_3T_nody_001.nii.gz \
       $DATADIR/23_3T_nody_001.nii.gz \
    -m $DATADIR/mask_10_3T_brain_smooth.nii.gz \
    --resolution 1.0
```

## Updates

A note re SingularityHub: the fetalReconstruction image provided via SingularityHub is rebuilt whenever there is a push to this GitHub repository.

## License

MIT license; see the [fetalReconstruction license](https://github.com/bkainz/fetalReconstruction/#license).

