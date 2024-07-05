# VScode R

A solution to run R scripts on computing nodes dancmpn01fl and dancmpn02fl using VScode code tunnel

This is much faster than the current RStudio solution, and frees up danGPU for those who actually need GPU resources :)

## code tunnel

The VScode CLI can be used to set up a tunnel so that we can connect to an srun job on the compute node:

```bash
ssh user@danhead01fl.unicph.domain
tmux new-session
srun -c 2 --mem=30gb --time=0-10:00:00 -w dancmpn02fl --pty bash
source ~/.bash_profile
module load vscode_cli gcc/11.2.0 R/4.3.1
code tunnel
```

- Choose `Microsoft account` when asked how you would like to log in to VScode
- Open the link and enter the given code to authenticate
- Chose your KU microsoft account and click continue
- Open the link in your web browser and accept the connection
- ALTERNATIVELY: Open your VScode desktop app, click `open a remote window` in bottom left corner, choose connect to tunnel > microsoft account > tunnel name and accept connection
- You can now run R interactively on the chosen compute node using VScode

**Note:** in the VScode desktop app, file > preferences > settings > R, Rpath and Rterm should be left blank and uncheck "always use active terminal"

**IMPORTANT!** When using `code tunnel` the connection between your local VScode instance and the compute node is bridged by a Microsoft server. This means `code tunnel` should not be used to work with sensitive human data. However, the connection is secured and authenticated using your Microsoft account and all data is encrypted during transit.

## code-server

Currently running code-server only works on danGPU which is NOT what we want!

VScode code-server alternative to code tunnel that consists of running [code-server](https://coder.com/docs/code-server/install) on a compute node and accessing via a web browser using ssh tunnels

```bash
ssh user@danhead01fl.unicph.domain
tmux new-session
srun -c 2 --mem=30gb --time=0-24:00:00 -p gpuqueue --pty bash
source ~/.bash_profile
module load code-server
code-server
# On local machine
ssh -fNL localhost:8080:localhost:8080 user@dangpu01fl.unicph.domain
```

It is not possible to set up an ssh tunnel from dancmpn01fl/2 to danhead for the code-server instance, error:

```bash
error listen EADDRINUSE: address already in use 127.0.0.1:8080
```

## Downloads

Do only once to make modules

### VScode CLI

```bash
curl -Lk 'https://code.visualstudio.com/sha/download?build=stable&os=cli-alpine-x64' --output vscode_cli.tar.gz
tar -xf vscode_cli.tar.gz
```

### VScode code-server

```bash
curl -fL https://github.com/coder/code-server/releases/download/v4.90.2/code-server-4.90.2-linux-amd64.tar.gz | tar -C /maps/projects/dan1/data/Brickman/shared/modules/software/code-server/4.90.2 -xz
```
