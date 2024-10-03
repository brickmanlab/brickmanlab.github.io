# Moving Dropbox to SUND

This is a step-by-step guide how I moved our Dropbox into SUND organized
by KU IT. In first attempt I have tried moving the files into OneDrive, but
because there might be issues with long filenames I eventually ran into more and
more problems

- random errors, because of different file size
- path name too long

Simpler solution is just to move things to SAMBA drives.

First, ssh into the server

```bash
ssh danhead01fl
tmux new -s dropbox-transfer
module load rclone/1.65.1
```

## Linking remotes

### Dropbox

```text
> n
> dropbox
> client_id <ENTER>
> client_secret <ENTER>
> y
forward port `ssh -fNL localhost:53682:localhost:53682 danhead01fl` and access the website locally
```

### Onedrive

```text
> n
> onedrive
> client_id <ENTER>
> client_secret <ENTER>
> region <ENTER>
> y
forward port `ssh -fNL localhost:53682:localhost:53682 danhead01fl` and access the website locally
> config_type 3
> https://alumni.sharepoint.com/sites/UCPH_BrickmanLab
> y
```

### Test connections

```bash
rclone lsd Dropbox:
rclone lsd dropbox_jb:
rclone lsd Onedrive:
```

## Copy files

I have started first with manual folders because we had to many folders and sometimes
there are timeout issues.

```bash
rclone copy --progress --checksum Dropbox:Computerome ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/Computerome
rclone copy --progress --checksum Dropbox:Courses ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/Courses
rclone copy --progress --checksum Dropbox:Grants ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/Grants
rclone copy --progress --checksum Dropbox:Other ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/Other
rclone copy --progress --checksum Dropbox:Papers ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/Papers
rclone copy --progress --checksum Dropbox:Pictures ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/Pictures
rclone copy --progress --checksum Dropbox:People ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/People
rclone copy --progress --checksum Dropbox:sc_seq_analysis ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/sc_seq_analysis
```

After the initial copy, I ran again copy this time of all the folders, most of
them should be present already. This is to make sure all files were moved.

```bash
rclone copy \
    --progress --checksum \
    --exclude="People/Fung/Home/IRCMS_interview_2024**" \
    --exclude="People/Fung/Home/MB1016613_backup**" \
    --exclude="GEO_data/**" \
    Dropbox: ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/

rclone copy --progress --checksum Dropbox:GEO_data ~/ucph/ndir/SUN-RENEW-Brickman/GEO_data/
rclone copy --progress --checksum dropbox_jb: ~/ucph/ndir/SUN-RENEW-Brickman/Dropbox/JoshBrickman
```
