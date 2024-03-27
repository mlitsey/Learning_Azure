

## envcopy call hook script

## SUP unmount disks
`sudo umount /epic/sup01`

## SUP detach disks 
```bash 
az vm disk detach -g odb__group --vm-name odb-sup-snap --name sup001 
az vm disk detach -g odb__group --vm-name odb-sup-snap --name sup002

# in a loop
for disk in `cat ~/disks.txt`; do az vm disk detach -g odb__group --vm-name odb-sup-snap --name $disk # --force-detach
```

## SUP delete old disks
```bash
az disk delete --resource-group --name sup001 --no-wait true --yes
az disk delete --resource-group --name sup002 --no-wait true --yes

# in a loop
for disk in `cat ~/disks.txt`; do az disk delete -g odb__group --name $disk --no-wait true --yes
```

## PRD freeze DB 
-  `ssh epicadm@EpicPrdServer:/epic/prd/bin/instfreeze`
    - PRD should be set to auto thaw in case of issues
    - ssh key share will need to be setup
    - ssh-copy-id <keyname> serverName

## create snap
```bash
az snapshot create -g odb__group -n prd001_backup --source prd001 --incremental true
az snapshot create -g odb__group -n prd002_backup --source prd002 --incremental true

# in a loop
for disk in `cat ~/disks.txt`; do az snapshot create -g odb__group -n $disk"_backup" --source $disk --incremental true; done
```

## PRD Thaw
`ssh epicadm@EpicPrdServer:/epic/prd/bin/instthaw`

## create disks
```bash
# location and zone must match the vm that disks will be mounted on
az disk create --resource-group odb__group --name sup001 --source prd001_backup --location eastus --zone 1
az disk create --resource-group odb__group --name sup002 --source prd002_backup --location eastus --zone 1

# in a loop 
while IFS='|' read -r i j; do
        echo "$i:$j"
        az disk create --resource-group odb__group --name $i --source $j --location eastus --zone 1
done < "/home/epicadm/data_disks.txt"

# data_disks.txt
sup001|prd001_backup
sup002|prd002_backup
```

## attach disks
```bash
az vm disk attach --vm-name odb-sup-snap -g odb__group --disks sup001 sup002
lsblk
```

## rename volume group
```bash
sudo vgrename prdvg supvg
lsblk
```

## rename logical volume
```bash
sudo lvrename /dev/supvg/prd01 sup01
lsblk
sudo lvs -o lv_name,vg_name,lv_size,stripes,stripesize,devices
```

## mount drives
```bash
sudo mount /dev/mapper/supvg-sup01 /epic/sup01
ll /epic/sup01
```

