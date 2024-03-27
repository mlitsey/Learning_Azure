# Linux Multi Disk Volume Group refresh from production server


## envcopy call hook script 

```bash
LOG=/epic/logs/sup_prd-refresh-$(date +"%Y-%m-%d-%H.%M").log

# Zero out log file, redirect all STDIN and STDERR output to log file
exec >${LOG} 2>&1


echo "kill user process in /epic/sup01 subdirectories"
fuser -kmMs /epic/sup01
#fuser -uk $(find /epic/sup01/ -type d)

## SUP unmount disks
`sudo umount /epic/sup01`

#echo "$(date +"%Y-%m-%d %H:%M:%S") Exporting target volume groups on snapshot host"
#/usr/sbin/vgchange -a n $volumeGroup



```

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
```bash
# Freeze Cache on PRD Cache server
echo "$(date +"%Y-%m-%d %H:%M:%S") Freezing Cache on PRD Cache server"
if [[ -v COMMIT ]]; then ssh -l ${EPICUSER} ${EPICHOST} ${EPICFREEZE}; fi
echo "$(date +"%Y-%m-%d %H:%M:%S") Cache frozen on PRD Cache server"
```

## create snap
```bash
az snapshot create -g odb__group -n prd001_backup --source prd001 --incremental true
az snapshot create -g odb__group -n prd002_backup --source prd002 --incremental true

# in a loop
for disk in `cat ~/disks.txt`; do az snapshot create -g odb__group -n $disk"_backup" --source $disk --incremental true; done
```

## PRD Thaw
`ssh epicadm@EpicPrdServer:/epic/prd/bin/instthaw`
```bash
# Thaw Cache on PRD Cache server
echo "$(date +"%Y-%m-%d %H:%M:%S") Thawing Cache on PRD Cache server"
if [[ -v COMMIT ]]; then ssh -l ${EPICUSER} ${EPICHOST} ${EPICTHAW}; fi
echo "$(date +"%Y-%m-%d %H:%M:%S") Cache thawed on PRD Cache server"
```

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
```bash
# some or all steps below are redundant from physical hardware script

echo "/usr/sbin/lvrename /dev/${NEWLVVG[$OLDLV]}/${OLDLV[$OLDLV]} ${NEWLV[$OLDLV]}"
    if [[ -v COMMIT ]]; then /usr/sbin/lvrename /dev/${NEWLVVG[$OLDLV]}/${OLDLV[$OLDLV]} ${NEWLV[$OLDLV]}; fi

    echo "mount -t xfs /dev/mapper/${NEWLVVG[$OLDLV]}-${NEWLV[$OLDLV]} ${MOUNTDIR[$OLDLV]}"
    if [[ -v COMMIT ]]; then mount -t xfs /dev/mapper/${NEWLVVG[$OLDLV]}-${NEWLV[$OLDLV]} ${MOUNTDIR[$OLDLV]}; fi

    echo "umount ${MOUNTDIR[$OLDLV]}"
    if [[ -v COMMIT ]]; then umount ${MOUNTDIR[$OLDLV]}; fi

    # check uuid of cachevg
    echo "$(date +"%Y-%m-%d %H:%M:%S") Checking UUID for cachevg"
    if [[ -v COMMIT ]]; then blkid |grep cachevg; fi

    echo "/usr/sbin/xfs_admin -U generate /dev/mapper/${NEWLVVG[$OLDLV]}-${NEWLV[$OLDLV]}"
    if [[ -v COMMIT ]]; then /usr/sbin/xfs_admin -U generate /dev/mapper/${NEWLVVG[$OLDLV]}-${NEWLV[$OLDLV]}; fi

    echo "mount -t xfs /dev/mapper/${NEWLVVG[$OLDLV]}-${NEWLV[$OLDLV]} ${MOUNTDIR[$OLDLV]}"
    if [[ -v COMMIT ]]; then mount -t xfs /dev/mapper/${NEWLVVG[$OLDLV]}-${NEWLV[$OLDLV]} ${MOUNTDIR[$OLDLV]}; fi

    # check uuid of cachevg after xfs_admin
    echo "$(date +"%Y-%m-%d %H:%M:%S") Checking UUID for cachevg after generating new one"
    if [[ -v COMMIT ]]; then blkid |grep cachevg; fi

# Start database or backup
echo "$(date +"%Y-%m-%d %H:%M:%S") Starting removing IRIS lock files"

for OLDLV in $KEYS
do
  echo "rm ${MOUNTDIR[$OLDLV]}/*/iris.lck"
  if [[ -v COMMIT ]]; then rm ${MOUNTDIR[$OLDLV]}/*/iris.lck ; fi
done


# End of script
echo "$(date +"%Y-%m-%d %H:%M:%S") Epic refresh script complete"
```


