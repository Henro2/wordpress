

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
 
 
    **sudo lvcreate -n apps-lv -L 14G webdata-vg**


    **sudo lvcreate -n logs-lv -L 14G webdata-vg**

    ![Alt text](Images/apps.jpg)



- I used sudo lvs to varify that my logical volumes was cteated successfully.

  ![Alt text](Images/sudolvs.jpg)








   
   


 


   