# Windows Server 2022 Packer Template

This folder contains a legacy JSON Packer template that builds a Windows Server 2022 qcow2 image using QEMU/KVM and WinRM.

## What it builds

- QEMU/KVM VM named "windows-server-2022"
- Output image in `build/os-base/` as qcow2
- WinRM communicator with user `packer` and password `PackerPassw0rd!`
- VirtIO drivers are downloaded and installed during provisioning
- Cloudbase-Init is installed and Sysprep is run

## Prerequisites

- Packer installed (JSON templates still supported)
- QEMU/KVM installed and working
- Enough disk space for a ~20 GB image
- The Windows Server 2022 ISO copied into this directory

## Required files

Place these files in this directory (the same directory as the JSON template):

- `SERVER_EVAL_x64FRE_en-us.iso`
- `Scripts/CloudBase-init-Install.ps1`
- `Scripts/prep-sysprep.ps1`
- `Scripts/cloudbase-init-conf`
- `floppy/Autounattend.xml`

The template uses the current working directory via `{{env `PWD`}}` to locate these files, so run Packer from this directory.

## Usage

From this directory:

```bash
packer build winserver2k22_build.json
```

If you need to override the working directory, you can set it for a single command:

```bash
PWD=/path/to/packer-windows-builder packer build winserver2k22_build.json
```

## Notes

- WinRM runs over HTTP on port 5985 during the build.
- The template binds VNC to 0.0.0.0; adjust if you need to limit access.
- The VirtIO ISO is downloaded each run from `http://192.168.1.9:8080/drivers/virtio-win.iso`.
- The build uses an 18 GB qcow2 disk (Packer `disk_size` is 18432 MB).

## Output

After a successful build, the qcow2 image is in `build/os-base/`.
