# NOVALINK's PVMCTL cheatsheet

**List Managed systems :**

`# pvmctl sys list`

**List VMs managed by PowerVC :**

```shell
# pvmctl lpar list
Logical Partitions
+--------------------------------+----+---------------+----------+-----------+---------------+-------+-----+------+
|              Name              | ID |     State     |   RMC    |    Env    |    Ref Code   |  Mem  | CPU | Ent  |
+--------------------------------+----+---------------+----------+-----------+---------------+-------+-----+------+
|          nova65637bc           | 3  |    running    |   ----   | AIX/Linux | Linux ppc64le |  5120 |  2  | 0.5  |
|          s00vl9972101          | 4  |    running    |  active  | AIX/Linux | Linux ppc64le | 10240 |  2  | 1.0  |
| s00va9970949-b577e741-00000022 | 5  |    running    |  active  | AIX/Linux |               | 32768 |  3  | 0.3  |
|  lpar_m2pb1-f5165cac-00000010  | 6  | not activated | inactive | AIX/Linux |    00000000   |  4096 |  1  | 0.25 |
| s00va9970951-dbc2ccbe-00000024 | 7  |    running    |  active  | AIX/Linux |               | 32768 |  3  | 0.3  |
| s00va9970954-9784f7e0-00000027 | 8  |    running    |  active  | AIX/Linux |               | 32768 |  3  | 0.3  |
+--------------------------------+----+---------------+----------+-----------+---------------+-------+-----+------+
```


**open a terminal on partition id #5:****

`# mkvterm --id 5`

**create a virtual ethernet adapter :**

`# pvmctl vea create --pvid 4094 --vswitch MGMTSWITCH --trunk-pri 1 --parent-id name=nova65637bc`

**delete a lpar**

`# pvmctl lpar  delete -i name=s00va9970949-e9fa6999-00000019`

**start a lpar :** 
`# pvmctl vios power-on -i name=s00ia9964446`

**start a lpar in sms mode :**
`# pvmctl lpar power-on --bootmode sms -i name=s00vl9972101`


**stop a lpar :** **

`# pvmctl lpar power-off -i name=s00va9970948-1096794a-0000000a --hard`

**Listing SRIOV ports:**

`# pvmctl sriov list`

**Changing a label on a SRIOV port :**

`# pvmctl sriov update  --port-loc U78CD.001.FZHE979-P2-C4-T3 -s label="sriov_vdct"`


**List SRIOV logical ports ( = virtual functions) for a lpar :** 
`#  pvmctl ethlp list -p name=s00va9970948-1096794a-0000000a`

**Delete a logical port :** 
`# pvmctl ethlp delete -i loc_code=U78CD.001.FZHE973-P1-C1-T3-S1`

**changing priority on a VNIC adapter :**
`# pvmctl vnic update --object-id uuid=00cbf727-7206-4591-937c-a96744119064 -s failover_pri=5 --loc U78CD.001.FZHE979-P1-C4-T3-S1`


**restart some openstack services:**

```
# systemctl restart openstack-nova-compute.service
# systemctl restart openstack-nova-neutron.service
```


**checking openstack status :** 
`# /opt/ibm/powervc/bin/powervc-services status | grep "^Active"`

**checking status of a service :** 
`#  /opt/ibm/powervc/bin/powervc-services single openstack-nova-compute status`

**Display Active (Link Up) SRIOV ports with available logical ports to configure, on the vswitch $vswitch:**

` # pvmctl  sriov list --display-fields PhysicalPort.cfg_lps loc_code cfg_max_lps PhysicalPort.label PhysicalPort.sublabel  --hide-label --where PhysicalPort.link_status="Up" --where SRIOVAdapter.mode="Sriov" --where PhysicalPort.cfg_lps!=PhysicalPort.cfg_max_lps|sort -n | awk -F, -v var=${vswitch} 'match($4,var)'`


**Listing VLANS in openstack :**

**on the novalink :** 

`# openstack`


