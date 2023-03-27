# NERSC scripts for Muon Collider Detector Studies

Collection of utilities, scripts and instructions for developing and using Muon Collider software at NERSC.

## Perlmutter at NERSC: Getting Started
The current primary cluster at NERSC is perlmutter. Information on how to ssh into it and how to setup MFA can be found [here](https://docs.nersc.gov/systems/perlmutter/).
See also the section on using `sshproxy.sh` to avoid having to enter your MFA at every login.
It is sometimes useful to define in your system (if *nix-based) an alias to the ssh command as, for example:
```bash
alias nk='/home/spagan/bin/sshproxy.sh -u ${USER}'
alias pmt='ssh -X -Y -i ~/.ssh/nersc ${USER}@perlmutter-p1.nersc.gov'
```
which works nicely to quickly setup the authentication key and log in to the cluster. You might need to adjust the user above to the specific username on the NERSC account, if different than the one you use on your local machine.

When you log in the first time, clone this repository in your home folder. It contains scripts that will be useful for your daily work.
In the following, it is assumed this repository is clone in `${HOME}/mcd-utils`.
It is recommended to use SSH keys as authentication method to github.

The home folder on this cluster is backed up daily and is meant to store mostly code, since space is limited.
For storing data (but practically you can store most of the code there as well), it is recommended to create a folder in the CFS filesystem:
```bash
mkdir /global/cfs/cdirs/atlas/${USER}/
mkdir /global/cfs/cdirs/atlas/${USER}/MuonCollider/
mkdir /global/cfs/cdirs/atlas/${USER}/MuonCollider/code
mkdir /global/cfs/cdirs/atlas/${USER}/MuonCollider/data
```

Another utility that is widely used to submit parallel jobs is [pytaskfarmer](https://gitlab.cern.ch/berkeleylab/pytaskfarmer), and its extension for Muon Collider studies [mcctaskfarmer](https://gitlab.cern.ch/berkeleylab/MuonCollider/mcctaskfarmer). Clone both repositories in your home folder:
```bash
git clone ssh://git@gitlab.cern.ch:7999/berkeleylab/pytaskfarmer.git
git clone ssh://git@gitlab.cern.ch:7999/berkeleylab/MuonCollider/mcctaskfarmer.git
```
Note: if you don't have a gitlab.cern.ch account setup with ssh keys, you might need to use the HTTPS repository instead (no authentication needed to clone/pull).

Every time you log in, to start the container with the muon collider software you can simply call the following script:
```bash
./mcd-utils/scripts/mcd-sw-shifter
```
This command will also source the `scripts/bashrc_muc.sh` file that sets up some useful aliases, e.g. the commands `acode` and `adata` will take you to the respective CFS filesystem folders created above.


## Submitting jobs
A few utility scripts are provided to ease the job submission on the cluster:
* `scripts/sbatch-slr.sh` this is the main script you can use with `sbatch` to submit jobs. Command-line arguments override the defaults provided at the top of the script for `sbatch`. See the top of the file for more information and syntax.
* `scripts/sim-worker.sh` and `scripts/reco-worker.sh` are examples of the actual job which calls either `ddsim` or `Marlin` and handles the output. These are meant to be copied into a different folder and customized as needed, although for most usages it will be enough to just edit the first few lines.
* `scripts/generate-tasklist.sh` is a simple script that can be helpful to automatically generate long lists of tasks to be executed. It assumes some nomenclature that would be natural is used in conjunctions with the scripts above.

