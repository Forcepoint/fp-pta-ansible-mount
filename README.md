# mount

Create the needed directory and mount a network share or other thing into it.

This role was created to fill a need for placing built ISO files from CI/CD into a file share
which was in turn mounted as a datastore in vSphere. This allowed easy use of the
vsphere-iso Packer plugin instead of using vmware-iso because the needed ISO was
already available to any VMs. There was no need to download the ISO locally to the 
machine running Packer.

## Requirements

None

## Role Variables

### REQUIRED

* mount_local_path: The local path to where the mount should go.
* mount_local_user: A local user to chown the mount path.
* mount_local_group: A local group to chown the mount path.
* mount_target_path: The mount target (i.e. a network share).
* mount_target_username: The username (hopefully vaulted) for the mount target (i.e. a network share).
* mount_target_password: The password (hopefully vaulted) for the mount target (i.e. a network share).
* mount_credential_path: The path to where the credential file containing the share username and password. 
  A file is used instead of coding the creds into fstab directly because fstab is readable by all users.
  The credential file will be owned by root, so placing it under the /root directory is a good idea.
* mount_fstype: The mount type (i.e. cifs).
* mount_options: Options for the mount command. 
* mount_yum_packages: Additional yum packages to install for your mounting needs (i.e. cifs-utils).

### OPTIONAL

None

## Dependencies

None

## Example Playbook

    - hosts: all
      vars:
        mount_local_path: /mnt/foobar/
        mount_local_user: service
        mount_local_group: service
        mount_target_path: //server.company.com/foobar/
        mount_target_username: "builds"
        mount_target_password: "SuPeRsEcReT"
        mount_credential_path: /root/.credential_mount_foobar
        mount_fstype: cifs
        mount_options: "credentials={{ mount_credential_path }},uid=1000,gid=1000,domain=BLAH"
        mount_yum_packages:
          - cifs-utils
          - rclone
      roles:
         - role: mount

## License

BSD-3-Clause

# Author Information

Jeremy Cornett <jeremy.cornett@forcepoint.com>
