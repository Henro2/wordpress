

## IMPLIMENTING WORDPRESS WEBSITE USING LOGICAL VOLUME MANAGEMENT STORAGE

**STEP1** (Web server Preperation)

-  Spined up Ubuntu redhat instance in AWS called **ApacheServerRH**



   ![Alt text](Images/instance.jpg)
- I created 3 volumes with 10gb each in the same availability zone and attached the volumes to my server. 

   ![Alt text](Images/volumes.jpg)


- With Termius cleint, I ssh into my server and updated it with **sudo yum update** 

    ![Alt text](Images/update.jpg)



- Using **lsblk**, I inspected the block divices i attached earlier and noted each name giving to them as xvdf,xvdg,xvdh

   ![Alt text](Images/lsblk1.jpg)


- I used **df -h** command to check all mounts and free space in my redhat server.

  ![Alt text](Images/dfdh1.jpg)



- Using **gdisk**, I created single partition on each of the 3 disks using the followind commands.
    sudo gdisk /dev/xvdf


    sudo gdisk /dev/xvdg


    sudo gdisk /dev/xvdh

    ![Alt text](Images/gdisk.jpg)



- Using **lsblk**, command, I checked to see the newly configure partition in the 3 disks.

  ![Alt text](Images/lsblk2.jpg)

-  I installed lvm2 package using **sudo yum install lvm2**

    ![Alt text](Images/lvm2innstall.jpg)

- Using **pvcreate**, I marked the 3 disks as physical volumes to be used my the LVM


   sudo pvcreate /dev/xvdf


   sudo pvcreate /dev/xvdg1


    sudo pvcreate /dev/xvdh1


    ![Alt text](Images/pvcreate.jpg)


-  To varify that my physical volumes have been created successfully, I used **sudo pvs** command

   ![Alt text](Images/pvs.jpg)

-  I used vgcreate to add the 3 Physical volumes to a volume group and name it **webdata-vg**

   ![Alt text](Images/webdata.jpg)



- Using **sudo vgs**, I varified that my Volume group was created successfully 
 ![Alt text](Images/vgs.jpg)

 - Using lvcreate utility, i created 2 logical volumes by diving the disk into half and call it **apps-lv** and use the remaining space for **logs-lv**, with this command

 -  **sudo vgdisplay -v #view complete setup - VG, PV, and LV
sudo lsblk**

 
 
    **sudo lvcreate -n apps-lv -L 14G webdata-vg**


    **sudo lvcreate -n logs-lv -L 14G webdata-vg**

    ![Alt text](Images/apps.jpg)



- I used sudo lvs to varify that my logical volumes was cteated successfully.

  ![Alt text](Images/sudolvs.jpg)


-  I used **sudo vgdisplay -v #view complete setup - VG, PV, and LV** to varify all my setup 
  ![Alt text](Images/allsetup.jpg)


  -  Sudo **slblk** 


 

    
    
![Alt text](Images/lsblk3.jpg)


-  I used **sudo mkfs -t ext4 /dev/webdata-vg/apps-lv**

 
    **sudo mkfs -t ext4 /dev/webdata-vg/logs-lv**


    to format the logical volumes in **ext4**  format


   ![Alt text](Images/format.jpg)



-  I now created the directory to store my website files 

    **sudo mkdir -p /var/www/html**


- I also created a directory to store my logs 

    **sudo mkdir -p /home/recorvery/logs**

    ![Alt text](Images/logdir.jpg)


-  I mouted my file system and log using 


     **sudo mount /dev/webdata-vg/apps-lv /var/www/html/**

    **sudo mount /dev/webdata-vg/logs-lv /var/log**


  -  Using sudo blkid to get UUID to update /etc/fstab file 

     ![Alt text](Images/uuidget.jpg)


     ![Alt text](Images/uuidd.jpg)  


     -  Sudo -a to mout it 


-    sudo **systemctl daemon-reload**,  to reolad .



-  df -h to varify that my setup is running 

   ![Alt text](Images/setuprunning.jpg)



   ## INSTALLING WORDPRESS AND CONFIGURING TO USE MYSQL DATABASE 

   

















   
   


 


   