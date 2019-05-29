# Singularity recipe for fetalReconstruction software

Making it easier to start using [https://github.com/bkainz/fetalReconstruction/](https://github.com/bkainz/fetalReconstruction/) on e.g. HPC.

<!-- TODO: update https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg -->

The quickest way to start using fetalReconstruction via this Singularity image 
is to pull the image from the SingularityHub on-line repository:

```sh
export SINGULARITY_CACHEDIR="/home/$USER/singularity_cache"
mkdir -p "${SINGULARITY_CACHEDIR}"
singularity pull --name fetalReconstruction.sif shub://willfurnass/fetalReconstruction_singularity:latest 
```

You are then able to run commands that live 'within' a fetalSingularity 'container' e.g.

```sh
singularity exec "${SINGULARITY_CACHEDIR}/fetalReconstruction.sif" /usr/local/bin/PVRreconstructionGPU
```

A note re SingularityHub: the fetalReconstruction image provided via SingularityHub is rebuilt whenever there is a push to this GitHub repository.

