## :memo:Bash advanced
### When problem entring a folder your own
`chmod -R 755 foldername`

### Add members to a Group
`usermod -aG group user`

### Kill process based on process name pattern
`pkill -f testraw`

### Check group members
`cat /etc/group`

### Make folders recursively owned by a user and a group
```
sudo chown -R user:group *
sudo chmod g+w -R node_modules
```

### Show command using PID
`ps -o cmd fp PID |less -+S`

### Check the PID of last executed command
`echo $!`

### Check memory usage by a user in GB
`ps hax -o rss,user | grep sandra |awk 'BEGIN {FS = " "} ; {sum+=$1} END {print sum/1000000}'`

### Check memory usage by all users in GB
`ps hax -o rss,user | awk 'BEGIN {FS = " "} ; {sum+=$1} END {print sum/1000000}'`

### Ipssmonstera IP address and map drive
```
130.92.155.10
sudo mount -t cifs -o username=choudhury //130.92.155.10/EcoGen /media/EcoGen
```

### Clear buffer/cache memory
`echo 3 | sudo tee /proc/sys/vm/drop_caches`

### Restart p910 display
`sudo systemctl restart display-manager.service`


### Change timestamp:
`find . -exec touch {} \;`

### Find files
`find / -name '*libfontconfig1*'`
Or use locate  

### Find .gz files containing a pattern
`find . -name \*.gz  | $HOME/parallel-20190722/usr/local/bin/parallel -q zgrep -H -e "A00574:80:H7TYWDRXX:2:2101:1108:1000"`  
Or in slurm
```
sbatch -J find3 -c 20 --wrap "find . -name \*.gz  | $HOME/parallel-20190722/usr/local/bin/parallel -q zgrep -H -e "A00574:80:H7TYWDRXX:2:2101:1108:1000""
```

### Copy files
use screen, execute command and then Ctrl a+d to detatch
```
screen -rd #to reattach
rsync -avz C201SC18110128/C101HW18110129_TR/raw_data/W22* rchoudhury@binfservms01.unibe.ch:/data/users/rchoudhury/.
```

### md5sum
```
$ md5sum file1.fastq.gz # before
$ md5sum -c checksums.md5 # after
md5sum: WARNING: 1 of 3 computed checksums did NOT match
```

### Install miniconda and set local lib path to miniconda env, useful for LD_LIBRARY_PATH
```
cd $CONDA_PREFIX
mkdir -p ./etc/conda/activate.d
mkdir -p ./etc/conda/deactivate.d
touch ./etc/conda/activate.d/env_vars.sh
touch ./etc/conda/deactivate.d/env_vars.sh
```

Edit`./etc/conda/activate.d/env_vars.sh`as follows:
```
#!/bin/sh
export MY_KEY='secret-key-value'
export MY_FILE=/path/to/my/file/
```

Edit `./etc/conda/deactivate.d/env_vars.sh`as follows:
```
#!/bin/sh
unset MY_KEY
unset MY_FILE
```
For $PATH  and $LD_LIBRARY_PATH:
in activate.d/env_vars.sh
```
    export OLD_LD_LIBRARY_PATH=${LD_LIBRARY_PATH}
    export LD_LIBRARY_PATH=/your/path:${LD_LIBRARY_PATH}
```
and then in deactivate.d/env_vars.sh
```
    export LD_LIBRARY_PATH=${OLD_LD_LIBRARY_PATH}
    unset OLD_LD_LIBRARY_PATH
```
### Start jupyter notebook with cluster computer
```
conda create -n withjupyter python=3
conda activate withjupyter
jupyter notebook --generate-config
jupyter notebook password

ssh -L 8000:localhost:8888 rchoudhury@binfservms01.unibe.ch
openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout mykey.key -out mycert.pem
conda install ipyparallel
ipcluster nbextension enable --user
```

### In slurm, set alias for useful sinfo, squeue and srun commands
```
alias si="sinfo --Node --format \"%N %P %g %C %O %T %E %H %m %e %d %l\""
alias sq="squeue --format \"%.18i %.9P %.8j %.8u %.2t %.10M %C %m %L %.6D %Q %R\""
alias sr="srun --pty /bin/bash"
```
