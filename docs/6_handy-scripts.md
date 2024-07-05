# Handy scripts

## GEO submission

1. Login to GEO using Google
2. Login to danGPU and change path to where the files are located
   1. It's either `~/Brickman/projects/` or `~/ucph/ndir/SUN-RENEW-Brickman/`
3. Download and update [GEO template](https://www.ncbi.nlm.nih.gov/geo/info/seq.html).
   1. NOTE: always make sure you use the latest version
4. Click `Transfer files` and copy the login information for the ftp
5. Adjust the variable from above step

**NOTE**: before running the command below, make sure you are already in the
folder and you see all the folder/files you want to upload. It will make the
steps below simpler.

```bash
# we run tmux session in case we loose connection
tmux new -s geo

# this loges you to FTP
sftp geoftp@sftp-private.ncbi.nlm.nih.gov
password: <PASSWORD>

cd uploads/<FOLDER>
mkdir <RNAseq>
cd <RNAseq>
mput *
```
