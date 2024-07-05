# Running pipelines

By default, we run [nf-core](https://nf-co.re) pipelines. To run a pipeline, read
the official [documentation with an example](https://nf-co.re/configs/ku_sund_danhead).

## Monitoring runs with Nextflow Tower

This is a guide on how to use [Nextflow Tower](https://help.tower.nf/23.1/) to
monitor nf-core pipeline runs.

We have created an API token for our GitHub account (brickmanlab) and restricted
it to run only pipelines, nothing else. The `TOWER_WORKSPACE_ID` and `TOWER_ACCESS_TOKEN`
are stored in `Brickman/config/brickman.bashrc`.

To do more advance stuff, you have to create your own [personal access token](https://tower.nf/tokens).

### Tower CLI installation

The tower cli[^1] is required to be installed only once to connect the server as
a computing resource. Afterward, it's not required any more[^2].

``` bash
# Download the latest version of Tower CLI:
wget https://github.com/seqeralabs/tower-cli/releases/download/v0.7.3/tw-0.7.3-linux-x86_64

# Make the file executable and move to directory accessible by $PATH variable:
mkdir ~/.local/bin && mv tw-* tw && chmod +x ~/.local/bin/tw
```

[^1]: [Tower CLI configuration](https://github.com/seqeralabs/tower-cli/#2-configuration)
[^2]: [Tower Agent](https://help.tower.nf/22.3/agent/)
