# ansible playbooks

## Setup linux hosts

### install openssh-server on linux remote hosts
```bash
sudo apt install -y openssh-server
```

-----

## Setup windows remote hosts
<!-- https://www.youtube.com/watch?v=-vPXS8UuJoI&t=93s -->
### create ansible user
```powershell
# create ansible user
$Password = Read-Host -AsSecureString
New-LocalUser "ansible" -Password $Password -FullName "ansible" -Description "ansible"
Add-LocalGroupMember -Group "Administrators" -Member "ansible"
```

### install WinRM
```powershell
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"

(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)

powershell.exe -ExecutionPolicy ByPass -File $file
```

-----

## Setup local machine

### install ansible
```bash
sudo apt install -y software-properties-common
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt update
sudo apt install -y ansible
ansible --version

ansible-galaxy collection install chocolatey.chocolatey
```

### generate ansible ssh key for linux remote hosts
```bash
# generate ssh key, save as "~/.ssh/ansible"
ssh-keygen -t ed25519 -C "ansible"      
```

### copy generated ansible ssh key, to linux remote hosts
```bash
ssh-copy-id -i ~/.ssh/ansible <ip-or-hostname>
```

ansible_user: LocalUsername
ansible_password: Password
ansible_connection: winrm
ansible_winrm_transport: basic