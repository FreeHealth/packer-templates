# packer-templates

[![Travis](https://img.shields.io/travis/kaorimatz/packer-templates.svg?style=flat-square)](https://travis-ci.org/kaorimatz/packer-templates)

[Packer](https://www.packer.io/) templates for [Vagrant](https://www.vagrantup.com/) base boxes

Fork of https://github.com/kaorimatz/packer-templates

Diff summary:

  * Ubuntu 17.10 added (json & preseed)
  * ISO url scheme for Ubuntu slightly modified ( /XX.XX version path is included in the default value for the mirror variable for each json file)

## Usage
Each modfication of the json file defining a build leads to downloading the
complete iso once again. The iso file is not cached.

If you plan to make adjustments to the json file, it is recommended to download
the iso file of the distribution and to add the path leading to the iso to your
packer build command:

    $ packer build -only=virtualbox-iso -var mirror=/path/to/iso/folder ubuntu-17.10-amd64.json

Do not add a final slash to your path. Use full path from / (no ~).

Clone the repository:

    $ git clone https://github.com/FreeHealth/packer-templates.git && cd packer-templates

Build a machine image from the template in the repository:

    $ packer build -only=virtualbox-iso ubuntu-17.10-amd64.json

A new file ubuntu-17.10-amd64-virtualbox.box will appear in the current folder.

Add the built box to Vagrant:

    $ vagrant box add ubuntu-17.10-amd64 ubuntu-17.10-amd64-virtualbox.box

Start the VM:

   $ cd vagrantfiles/ubuntu-17.10
   $ vagrant up

The headless VM should be running, you can check the VirtualBox GUI manager.

## Configuration

You can configure each template to match your requirements by setting the following [user variables](https://packer.io/docs/templates/user-variables.html).

 User Variable       | Default Value | Description
---------------------|---------------|----------------------------------------------------------------------------------------
 `compression_level` | 6             | [Documentation](https://packer.io/docs/post-processors/vagrant.html#compression_level)
 `cpus`              | 1             | Number of CPUs
 `disk_size`         | 40000         | [Documentation](https://packer.io/docs/builders/virtualbox-iso.html#disk_size)
 `headless`          | 0             | [Documentation](https://packer.io/docs/builders/virtualbox-iso.html#headless)
 `memory`            | 512           | Memory size in MB
 `mirror`            |               | A URL of the mirror (or a full path to the folder) where the ISO image is available

### Example

Build an uncompressed Arch Linux vagrant box with a 4GB hard disk using the VirtualBox provider:

    $ packer build -only=virtualbox-iso -var compression_level=0 -var disk_size=4000 archlinux-x86_64.json
