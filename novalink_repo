###### tags: `novalink` `apt` `repo`

# Novalink : creating a local repo for a new novalink release


## Download sources 

### Option 1 : ISO file

Download the ISO from IBM site
http://public.dhe.ibm.com/systems/virtualization/Novalink/installer/PowerVM_NovaLink_V1.0.0.9_02222018-258.iso

#### loopmount it and copy it to the source directory
```shell 
# loopmount -i PowerVM_NovaLink_V1.0.0.9_02222018-258.iso  -o "-V cdrfs -o ro " -m /mnt/
# mkdir  /export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt/
# rsync -xavuz --progress -h /mnt/ /export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt/

```

#### check the pvmrepo.tgz file and extract it to the source directory

```
# ls pvm/repo/export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt/pvm/repo/pvmrepo.tgz
-r--r--r--    1 root     system    438699984 Feb 22 16:41 /mnt/pvm/repo/pvmrepo.tgz


# mkdir /export/nim/lpp_source/powervc/novalink/1.0.0.9/pvmrepo
# cd /export/nim/lpp_source/powervc/novalink/1.0.0.9/pvmrepo
# gunzip ../mnt/pvm/repo/pvmrepo.tgz
# tar xfvl ../mnt/pvm/repo/pvmrepo.tar

# ls -ltr 1.0.0.9/pvmrepo/
total 859488
-rwxr-xr-x    1 root     system         3132 May 24 2016  novalink-gpg-pub.key
-rwxr-xr-x    1 root     system    440053760 Mar 07 10:45 pvmrepo.tar
drwxr-xr-x    4 root     system          256 Mar 07 10:46 pool
drwxr-xr-x    2 root     system          256 Mar 07 10:46 conf
drwxr-xr-x    3 root     system          256 Mar 07 10:46 dists
drwxr-xr-x    2 root     system          256 Mar 07 10:46 db
```
Dont forget to change the permissions to allow other users than the owner to use it : 

```shell
# chmod 755 1.0.0.9
```

#### modify the apt sources on novalink

##### change the OS source 
```
#cat /etc/apt/sources.list

deb http://<NFSSERVER>/export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt/ xenial main restricted universe
```
##### change the pvm novalink source
```
cat /etc/apt/sources.list.d/pvm.list
deb http://<NFSSERVER>/export/nim/lpp_source/powervc/novalink/1.0.0.9/pvmrepo novalink_1.0.0 non-free optional
```

## update repo and apt upgrade novalink packages

``` 
# apt update
Ign:1 http://<NFSSERVER>/export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt xenial InRelease
Hit:2 http://<NFSSERVER>/export/nim/lpp_source/powervc/novalink/1.0.0.9/pvmrepo novalink_1.0.0 InRelease
Hit:3 http://<NFSSERVER>/export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt xenial Release
Reading package lists... Done
Building dependency tree
Reading state information... Done
All packages are up to date.

# apt install pvm-novalink
[..]

Setting up pvm-core-dev (1.0.0.9-180219-4816) ...
Setting up pvm-rest-server (1.0.0.9-180208-98) ...
Setting up pvm-rest-app (1.0.0.9-180214-2162) ...
Setting up pvm-novalink (1.0.0.9-180222-3008) ...

# dpkg -l |grep pvm
ii  pvm-cli                                   1.0.0.9-180222-2851                        all          Power VM Command Line Interface
ii  pvm-core                                  1.0.0.9-180219-4816                        ppc64el      PVM core runtime package
ii  pvm-core-dev                              1.0.0.9-180219-4816                        ppc64el      PVM core development and test package
ii  pvm-novalink                              1.0.0.9-180222-3008                        ppc64el      Meta package for all PowerVM Novalink packages
ii  pvm-rest-app                              1.0.0.9-180214-2162                        ppc64el      The PowerVM NovaLink REST API Application
ii  pvm-rest-server                           1.0.0.9-180208-98                          ppc64el      Holds the basic installation of the REST WebServer (Websphere Liberty Profile) for PowerVM NovaLink


```

## Modify the ansible playbook's templates once it works


```python
# cat /srv/ansible/roles/novalink_upgrade/templates/pvm.list.j2
deb http://{{ repo }}/export/nim/lpp_source/powervc/novalink/1.0.0.9/pvmrepo novalink_1.0.0 non-free optional

# cat /srv/ansible/roles/novalink_upgrade/templates/sources.list.j2
deb http://{{repo}}/export/nim/lpp_source/powervc/novalink/1.0.0.9/mnt/ xenial main restricted universe multiverse

# ansible-playbook -i inventories/hosts.novalink_upgrade  site.yml --ask-vault-pass
[..]
```

---

### Option 2 : sync the online repo

#### Replicate the online repository

wget http://public.dhe.ibm.com/systems/virtualization/Novalink/installer/

And repeat the previous steps !

# LINKS 


https://www.ibm.com/developerworks/community/wikis/home?lang=es#!/wiki/W51a7ffcf4dfd_4b40_9d82_446ebc23c550/page/Netbooting%20on%20POWER%20-%20An%20Introduction

https://www.ibm.com/support/knowledgecenter/POWER8/p8eig/p8eig_setting_up_netboot.htm

https://www.gnu.org/software/grub/manual/grub/grub.html#Network

