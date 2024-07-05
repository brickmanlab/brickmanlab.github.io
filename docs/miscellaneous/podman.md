# Podman

## Setup

Storage for Podman needs to be configured to fix UID errors when running on UTF filesystem:

```bash
mkdir -p ~/.config/containers
cp /maps/projects/dan1/apps/podman/4.0.2/storage.conf $HOME/.config/containers/
```

Rootless Podman also requires username and allowed UID range to be listed in /etc/subuid and /etc/subgid

List running containers and run a publically available container image to confirm Podman is working:

```bash
podman ps
podman run -it docker.io/library/busybox
```

## Running the KU SUND DANGPU nf-core config with Podman

Currently this is not practical because file permissions cause the following error:

```plaintext
error during container init: error setting cgroup config for procHooks process: cannot set memory limit: container could not join or create cgroup
```

The nf-core config file, podman.config, can be found at /scratch/Brickman/pipelines/

Specify podman.config in nextflow run options to run a pipeline with Podman, e.g. for the rnaseq test profile:

```bash
nextflow run nf-core/rnaseq -r 3.8.1 -c podman.config -profile test --outdir nfcore_test
```
