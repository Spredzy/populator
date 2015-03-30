# populator

An ansible play that allows one to spawn VMs on a remote libvirt hypervisor

## Objective

Ever wanted to deploy cloud images on a libvirt hypervisor without having to install full-blow software ?

This play might do what you want.

Retrieve a cloud image from wherever you want in your hypervisor, configure your nodes in the nodes.json file and simply run ansible.
It will provision the domain and voila !

## Usage

On the hypervisor

```
# > wget -O /var/lib/libvirt/images/XX.qcow2 https://some.url/somedistro.qcow2`
```

On your laptop

```
ansible hypervisor.yml --extra-vars "@nodes.json"
```

with a `nodes.json` file that looks like

```json
{
  "host": "myhypervisor.example.com",
  "remote_user": "user1",
  "nodes": [
    {
      "name": "web01",
      "uuid": "f1a61b1d-a1c8-444c-9eea-18aaf24c0d29",
      "dest_qcow2": "/var/lib/libvirt/images/web01.qcow2",
      "base_qcow2": "/var/lib/libvirt/images/centos7.qcow2",
      "size_qcow2": "40G",
      "dest_iso": "/var/lib/libvirt/images/web01.iso",
      "tmp_dir": "/tmp"
    },
    {
      "name": "mysql01",
      "uuid": "d6729e6e-fa76-43bf-80b8-bc7ffd3f0eaa",
      "dest_qcow2": "/var/lib/libvirt/images/mysql01.qcow2",
      "base_qcow2": "/var/lib/libvirt/images/centos7.qcow2",
      "size_qcow2": "40G",
      "dest_iso": "/var/lib/libvirt/images/mysql01.iso",
      "tmp_dir": "/tmp"
    }
  ]
}
```

### Variables

Per system :

* host: hypervisor hostname
* remote_user: user to ssh into with

Per nodes :

* name: hostname of the VM
* uuid: UUID of the domain
* dest_qcow2: Path where the generated qcow2 will be located
* base_qcow2: Path where the base qcow2 is located
* size_qcow2: Size of the generated qcow2
* dest_iso: Path where the generated iso file will be located
* tmp_dir: Where the tmp files will be located
