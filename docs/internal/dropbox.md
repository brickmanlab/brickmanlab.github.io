# Moving Dropbox to OneDrive

This is a step-by-step guide how I moved our Dropbox into OneDrive organized
by KU IT.

First, ssh into the server

```bash
ssh danhead01fl
tmux new -s dropbox-transfer
module load rclone/1.65.1
```

## Linking remotes

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

Test: `rclone ls Onedrive: | head -n 5`

### Dropbox

```text
> n
> dropbox
> client_id <ENTER>
> client_secret <ENTER>
> y
forward port `ssh -fNL localhost:53682:localhost:53682 danhead01fl` and access the website locally
```

Test: `rclone ls Dropbox: | head -n 5`

## Execute

Moving stuff to OneDrive did not work because of multiple problems:

- random errors, because of different file size
- path name too long

```bash
# rclone copy --progress --checksum --dry-run Dropbox: Onedrive/Dropbox
rclone copy --progress --checksum Dropbox:Pictures Onedrive:Dropbox/Pictures
rclone copy --progress --checksum Dropbox:Computerome Onedrive:Dropbox/Computerome
rclone copy --progress --checksum Dropbox:Courses Onedrive:Dropbox/Courses
rclone copy --progress --checksum Dropbox:Grants Onedrive:Dropbox/Grants
rclone copy --progress --checksum Dropbox:Other Onedrive:Dropbox/Other
rclone copy --progress --checksum Dropbox:Papers Onedrive:Dropbox/Papers
rclone copy --progress --checksum Dropbox:People Onedrive:Dropbox/People
rclone copy --progress --checksum Dropbox:sc_seq_analysis Onedrive:Dropbox/sc_seq_analysis
# copy one more time everything that failed
# usually it was html reports from homer
rclone copy --progress Dropbox: Onedrive:Dropbox
```

What did work was just moving the files on SAMBA drives

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
