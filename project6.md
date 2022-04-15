# PROJECT 6: WEB SOLUTION WITH WORDPRESS

> ## STEP 1 -- PREPARE THE WEB SERVER

- Create 2 Red Hat instances

  - The first Red Hat install is for the web server
  - Create 3 volumes for the web server, one at a time
  - Click on create volume to create the volumes
  - Each volume will have a size of 10GB and must be in the availability region of the web server

- Login to the web server

  List all the block

  `lsblk`

- Check the all the mounts and free space on the web server using

  `df -h`
