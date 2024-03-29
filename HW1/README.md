# CSEN-241-HW1

## User Guide

This part describes how to use this project.

### Prerequisites

1. Downloaded ubuntu image from official website (`ubuntu-20.04.6-live-server-amd64.iso` for my machine), and put it into `./images/` folder.

### Step 1: Create disk images

```bash
./create_images.sh
```

This command creates images in multiple formats that need to be tested in this homework.

### Step 2: Install Ubuntu OS to disk images

```bash
./install_system_raw.sh
```

```bash
./install_system_qcow2.sh
```

These two command are better to be executed manually and separately because installing Ubuntu is an interactive process that requires you to choose some options and enter default username and password through keyboard in QEMU GUI.

During installation, choose "Install openssl", and do not choose to install any other optional software. When the top of screen shows "Installation completed!", You can choose the option "Cancel updating and reboot".

### Step 3: Check if VM can be lauched successfully

```bash
./start_raw_with_graphic.sh
```

```bash
./start_qcow2_with_graphic.sh
```

These two command are also recommended to be execute manually and separately because we want to ensure OS is successfully installed before we move on to the next step.

### Step 4: Provision software to VM

Install sysbench on guest OS

```bash
sudo apt install sysbench -y
```

### Step 5: Copy test script into VM

First, Generate an ssh key pair for ssh connection to VM.

```bash
ssh-keygen -f ~/.ssh/qemu
```

For each VM instance, first launch VM with port forwarding. Then copy ssh public key to guest OS. Finally, copy sysbench_script into guest OS.

```bash
# Start VM with port forwarding
./start_raw_with_port_forwarding.sh

# Copy ssh public key into VM
ssh-copy-id -i ~/.ssh/qemu.pub  -p 8888 qizhe@localhost

# Copy test script into VM 
scp -p 8888 sysbench_script.sh qizhe@localhost:~/sysbench_script.sh
```

```bash
# Start VM with port forwarding
./start_qcow2_with_port_forwarding.sh

# Copy ssh public key into VM
ssh-copy-id -i ~/.ssh/qemu.pub  -p 8888 qizhe@localhost

# Copy test script into VM 
scp -p 8888 sysbench_script.sh qizhe@localhost:~/sysbench_script.sh
```


### Step 5: Run tests

```bash
./tests
```

The 