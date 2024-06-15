# My Dev Setup For Data & AI (2024)

## Summary
- **Main Operating System (OS):** Windows 11 (applicable to Windows 10)
- **Secondary OS:** Linux Ubuntu via Windows Subsystem for Linux (WSL)
- **Terminals:** BASH (with custom prompt for Git branches) + ZSH (OhMyZsh)
- **IDE:** Visual Studio Code (VS Code) (installed on Windows)
- **Version Control System:** Git + GitHub (with automated SSH-agent and passphrases)
- **Python Environment Manager:** Miniconda
- **Containerization:** Docker Desktop (installed on Windows)
- **Database:** PostgreSQL + pgcli + pgAdmin


## Setup Windows Subsystem for Linux (WSL)

### Useful references
https://learn.microsoft.com/en-us/windows/wsl/install-manual
https://learn.microsoft.com/en-us/windows/terminal/install

### Operating system
Windows 11 or Windows 10

![Windows 11 or Windows 10](./assets/win10-win11-logos.png)


### Hardware virtualization
Enabled (check on Task Manager -> Performance -> Virtualization: Enabled)
If it's not, I suggest enabling it on BIOS (on your CPU options enable Intel VT-x or AMD-V)
(not sure if this is mandatory, but I always do this to ensure I can create virtual machines
it might have some performance impact on your WSL2 setup, just in case, enable it if you can)

### Turn Windows features on or off
Enable both:
- Virtual Machine Platform
- Windows Subsystem for Linux

### Restart machine

### Install / Update WSL
Using Command Prompt (CMD) run several times
```bash
wsl --update
```
until the last version is installed

### Check available Linux distros
```bash
wsl --list --online
```

### Choose and install Linux distro
```bash
wsl --install -d Ubuntu-22.04
```

### Define UNIX username

### Define password

### Install terminal
From Microsoft Store install the official Terminal app by Microsoft Corporation, if you don't have it yet

### Default terminal
Make Linux distro the default terminal on Terminal app

### Upgrade and Update Linux distro
```bash
sudo apt update
sudo apt upgrade
```

## Review Linux Commands
Basic Linux Commands for Beginners


## Setup Git and GitHub with SSH

### Useful references
https://docs.github.com/en/authentication/connecting-to-github-with-ssh

### Install Git
Git should be installed by default, if not, run
sudo apt install git

### Generate SSH key
Use same email as GitHub account
ssh-keygen -t ed25519 -C "techwithcosta@gmail.com"
Do add passphrase, easy to remember

### Create GitHub account if you don't have one yet

### Add the public SSH key to GitHub account
check .ssh folder, .pub file
cat id_ed25519.pub

### Test SSH connection to GitHub
ssh -T git@github.com
confirm GitHub key fingerprint with website
https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/githubs-ssh-key-fingerprints

### Confirm keys from known hosts
cat known_hosts

### Remove old known hosts file
rm known_hosts.old

### Configure Git with your identity
git config --global user.name "Diogo Costa"
git config --global user.email techwithcosta@gmail.com
Check ".gitconfig" File

### Create folder for projects on user's home directory "git"
mkdir git

### Clone original course repo
inside "git" folder
git clone git@github.com:DataTalksClub/llm-zoomcamp.git

### Create and clone your course repo
inside "git" folder
git clone git@github.com:techwithcosta/llm-zoomcamp-2024.git


## Setup Linux Terminals
https://tinyurl.com/techwithcosta-yt-010
https://tinyurl.com/techwithcosta-yt-011

### Backup .bashrc file
cp .bashrc .bashrc.bak

### Remove prompt config rows from .bashrc file

```bash
if [ "$color_prompt" = yes ]; then
    PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '
else
    PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
fi
```

### Add prompt config rows from .bashrc file

```bash
# Add git branch to prompt
parse_git_branch() {
    git rev-parse --is-inside-work-tree >/dev/null 2>&1 || return
    git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/' | sed 's/^/ /'
}
if [ "$color_prompt" = yes ]; then
 PS1='${debian_chroot:+($debian_chroot)}\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[01;31m\]$(parse_git_branch)\[\033[00m\]\$ '
else
 PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w$(parse_git_branch)\$ '
fi
```

### Refresh bash

### Check main branch on original course repo folder

### Delete .bashrc backup if main shows up

```bash
rm .bashrc.bak
```

### Install zsh

```bash
sudo apt install zsh
```

### Install OhMyZsh

```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
make zsh default shell

### Automate SSH-Agent passphrases for both bash and zsh

paste following code on both .bashrc and .zshrc files

```bash
# Auto-lauching ssh-agent
env=~/.ssh/agent.env

agent_load_env () { test -f "$env" && . "$env" >| /dev/null ; }

agent_start () {
    (umask 077; ssh-agent >| "$env")
    . "$env" >| /dev/null ; }

agent_load_env

# agent_run_state: 0=agent running w/ key; 1=agent w/o key; 2=agent not running
agent_run_state=$(ssh-add -l >| /dev/null 2>&1; echo $?)

if [ ! "$SSH_AUTH_SOCK" ] || [ $agent_run_state = 2 ]; then
    agent_start
    ssh-add
elif [ "$SSH_AUTH_SOCK" ] && [ $agent_run_state = 1 ]; then
    ssh-add
fi

unset env
```

### Validate that terminal only asks for passphrase once by refreshing terminal 2 times

Test SSH connection to GitHub
```bash
ssh -T git@github.com
```

## Setup VS Code With WSL (IDE)

### Download Visual Studio Code (VS Code)
https://code.visualstudio.com/download

### Install VS Code
Activate all ticks except desktop icon
Pin to taskbar

### Follow the welcome steps if you want and read docs to learn more about VS Code functionalities
https://code.visualstudio.com/docs
https://code.visualstudio.com/docs/remote/wsl
https://code.visualstudio.com/docs/terminal/basics
https://code.visualstudio.com/docs/python/python-quick-start
https://code.visualstudio.com/docs/containers/overview
Explore WSL, Git + GitHub setup, Python, Docker etc.

### Install WSL extension
It should pop up automatically
If not, go to extensions panel and find it
Show installed extension

### Close VS Code and open it from Linux terminal using "code ." on original repo folder
Remote connection to Linux distro will be created automatically by downloading VS Code Server

### Show WSL tab

### Show git / GitHub tab + Source Control panel
Edit readme and save.
Check zsh cross on terminal and source control panel, could commit from here. Discard changes.

### Additional extensions
- Code Spell Checker by Street Side Software
- Todo Tree by Gruntfuggly

### Show Explorer panel
### Open README.md and CTRL + SHIFT + P, search for "markdown preview to the side"

### Show Search

### Show Run and Debug panel

### Open new terminals BASH and ZSH and TMUX


## Setup VS Code For Python on WSL With Miniconda
https://engineeringfordatascience.com/posts/install_miniconda_from_the_command_line/

### Install latest version of Miniconda
```bash
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
~/miniconda3/bin/conda init zsh
```

### Prevent conda from activating the base env by default
```bash
conda config --set auto_activate_base false
```

### Create conda environment
Create conda env for project where "llm-zoomcamp-2024-env" is my env name. I like to use the project folder name + "-env". Change the Python version if needed. If you omit "python=3.10", an new env is created but without Python installed and you would need to install it manually.
```bash
conda create -n llm-zoomcamp-2024-env python=3.10
```

### Activate conda environment
```bash
conda activate llm-zoomcamp-2024-env
```

### Open project on VS Code
Open VS Code from project folder using
```bash
code .
```

### Create and save Python file
```bash
code main.py
```

### Additional extension
Python by Microsoft

### Associate correct Python interpreter on VS Code
Click the Python interpreter tab on bottom right corner or CTRL + SHIFT + P and search "interpreter", "Python: Select Interpreter".
Choose the env you've just created.
Refresh terminals inside VS Code.
From now on whenever you open the project with VS Code, this conda env will be activated in the terminal, and the interpreter correctly selected.
Whenever we install packages via the terminal, ensuring the correct env is activated, the packaged will be isolated into it, avoiding corruption of other projects.

### Restart VS Code and validate activated env + selected interpreter

### Python examples
- script.py with print('hello world'), execute using UI play and terminal

### Additional extension
Jupyter by Microsoft

### Python examples
- cells.py with #%%, execute cell using UI, CTRL + ENTER or SHIFT + ENTER, install ipykernel package, show interactive window, jupyter tab
- save the file as cells.ipynb to see it as a classic jupyter notebook, to run it select an interpreter

### Python example (Jupyter notebook browser)
If you really want to run a jupyter notebook on browser
```bash
pip install jupyter
jupyter notebook
```
Jupyter server will start and ports will be forwarded. Then open the browser. Copy token from
```bash
jupyter server list
```
Paste token on browser, set up a new password, login, open the notebook from the file explorer.

### GIt example
Commit Python file.

## Download and install Docker Desktop on Windows

Run the following and confirm that Docker is not accessible
```bash
docker
```

https://www.docker.com/products/docker-desktop/

- Use WSL2 instead of Hyper-V during setup
- Run docker after installation
- Sign up and / or sign in
- Check for updates
- Update and restart PC
- Start Docker engine

https://docs.docker.com/desktop/wsl/

- Go to Settings -> General and check if "Use the WSL 2 based engine" is ticked
- Go to Settings -> Resources -> WSL Integration: check if "Enable integration with my default WSL distro" is ticked and enable your distro "Ubuntu-22.04"
- Apply & restart

Now the following command should output something
```bash
docker
```

Run your first container
```bash
docker run -d -p 8080:80 docker/welcome-to-docker
```

Open browser and go to http://localhost:8080/

If you get this error
```bash
docker: Error response from daemon: Head "https://registry-1.docker.io/v2/docker/welcome-to-docker/manifests/latest": unauthorized: incorrect username or password.
```

Log out from Docker using the system tray icon. Then login via terminal
```bash
docker login --username techwithcosta
```
Enter password

Run your first container
```bash
docker run -d -p 8080:80 docker/welcome-to-docker
```

Open browser and go to http://localhost:8080/

## Setup Docker with PostgreSQL (database)

### Define containers

Create **docker-compose.yaml** file

```yaml
services:
  pgdatabase:
    image: postgres:13
    environment:
      - POSTGRES_USER=root
      - POSTGRES_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - "./ny_taxi_postgres_data:/var/lib/postgresql/data"
    ports:
      - "5432:5432"
  pgadmin:
    image: dpage/pgadmin4
    environment:
      - PGADMIN_DEFAULT_EMAIL=admin@admin.com
      - PGADMIN_DEFAULT_PASSWORD=root
      - POSTGRES_DB=ny_taxi
    volumes:
      - pgadmin_data:/var/lib/pgadmin/data
    ports:
      - "8080:80"
volumes:
  pgadmin_data:
```
Stop all containers to ensure all ports are available.

```bash
docker compose up
```

### Setup pgcli

Install pgcli dependencies
https://www.pgcli.com/install

```bash
sudo apt-get install libpq-dev
```

Install pgcli with correct env activated (it's a Python package)
```bash
pip install pgcli
```

### Connect to PostgreSQL via pgcli

Access the PostgreSQL DB on Docker container, use the credentials from docker file -h host, -p port, -u user, -d db name
```bash
pgcli -h localhost -p 5432 -u root -d ny_taxi
```
Enter password

Check empty list of tables inside DB
```bash
\dt
```
Quit by pressing "q"

### Connect to PostgreSQL via pgAdmin

Both containers have to be connected to the same network in order to pdAdmin connect to PostgreSQL DB.

Go to browser and http://localhost:8080/. pgAdmin should open. Login using credentials on docker file.

On pdAdmin go to Object -> Register -> Server. Give it a name "mydb", go to Connection "Host name/address" should be "pgdatabase" or "host.docker.internal". Port is 5432, Username "root", Password "root".

**1. Using pgdatabase as Hostname**
When you use pgdatabase as the hostname, it works because Docker Compose creates a default network for the services defined in the same docker-compose.yml file. Containers in the same Docker Compose project can communicate with each other using their service names as hostnames. This means pgadmin can reach pgdatabase by resolving pgdatabase to the correct IP address of the PostgreSQL container.

**2. Using host.docker.internal as Hostname**
When you use host.docker.internal as the hostname, it works because host.docker.internal is a special DNS name that resolves to the internal IP address of the host machine. This allows containers to access services running on the host machine. Since you mapped port 5432 on the host to port 5432 on the pgdatabase container, pgAdmin can connect to PostgreSQL using the host's IP address.

https://stackoverflow.com/questions/25540711/docker-postgres-pgadmin-local-connection

### Create sample table via pgAdmin

Create sample table with new query: https://www.w3schools.com/postgresql/postgresql_create_table.php

```sql
CREATE TABLE cars (
  brand VARCHAR(255),
  model VARCHAR(255),
  year INT
);
```

Insert some data

```sql
INSERT INTO cars (brand, model, year)
VALUES
  ('Volvo', 'p1800', 1968),
  ('BMW', 'M1', 1978),
  ('Toyota', 'Celica', 1975);
```

View all rows on pgAdmin.

### Access same table via pgcli

On pgcli run \dt see new table.
```sql
SELECT * FROM cars;
SELECT * FROM cars ORDER BY year;
SELECT * FROM cars WHERE year >= 1975;
```


## Install Terraform Infrastructure as Code (IaC)

Follow steps from: https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli

Ensure that your system is up to date and you have installed the `gnupg`, `software-properties-common`, and `curl` packages installed. You will use these packages to verify HashiCorp's GPG signature and install HashiCorp's Debian package repository.
```bash
sudo apt-get update && sudo apt-get install -y gnupg software-properties-common
```

Install the HashiCorp GPG key.
```bash
wget -O- https://apt.releases.hashicorp.com/gpg | \
gpg --dearmor | \
sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg > /dev/null
```

Verify the key's fingerprint.
```bash
gpg --no-default-keyring \
--keyring /usr/share/keyrings/hashicorp-archive-keyring.gpg \
--fingerprint
```

The gpg command will report the key fingerprint. Linux package checksum verification here, confirm fingerprint
https://www.hashicorp.com/trust/security?product_intent=terraform

Add the official HashiCorp repository to your system. The `lsb_release -cs` command finds the distribution release codename for your current system, such as `buster`, `groovy`, or `sid`.
```bash
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] \
https://apt.releases.hashicorp.com $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/hashicorp.list
```

Download the package information from HashiCorp.
```bash
sudo apt update
```

Install Terraform from the new repository.
```bash
sudo apt-get install terraform
```

Verify installation.
```bash
terraform
```

Install the autocomplete package.
```bash
terraform -install-autocomplete
```

Create a directory named `learn-terraform-docker-container`.
```bash
mkdir learn-terraform-docker-container
```

This working directory houses the configuration files that you write to describe the infrastructure you want Terraform to create and manage. When you initialize and apply the configuration here, Terraform uses this directory to store required plugins, modules (pre-written configurations), and information about the real infrastructure it created.

Navigate into the working directory.

```bash
cd learn-terraform-docker-container
```

In the working directory, create a file called `main.tf` and paste the following Terraform configuration into it.

```bash
code main.tf
```
Paste following code.

```terraform
terraform {
  required_providers {
    docker = {
      source  = "kreuzwerker/docker"
      version = "~> 3.0.1"
    }
  }
}

provider "docker" {}

resource "docker_image" "nginx" {
  name         = "nginx"
  keep_locally = false
}

resource "docker_container" "nginx" {
  image = docker_image.nginx.image_id
  name  = "tutorial"

  ports {
    internal = 80
    external = 8000
  }
}
```

Install extension on VS Code: HashiCorp Terraform by HashiCorp. Now the file gets highlighted.

Initialize the project, which downloads a plugin called a provider that lets Terraform interact with Docker.

If you get errors such as `Error: Failed to query available provider packages` you have permissions / connection problems with your WSL / Ubuntu. Workaround here https://github.com/microsoft/WSL/issues/8022

If you have to restore the `/etc/resolv.conf` file:
```bash
sudo apt-get install --reinstall resolvconf
```

```bash
terraform init
```

Provision the NGINX server container with `apply`. When Terraform asks you to confirm type `yes` and press `ENTER`.
```bash
terraform apply
```

Verify the existence of the NGINX container by visiting localhost:8000 in your web browser or running `docker ps` to see the container.
```bash
docker ps
```

To stop the container, run `terraform destroy`.
```bash
terraform destroy
```

essentially, before starting the course, a beginner should understand the following (this course is not beginner friendly)
- IT (setting up dev environments, infrastructure, VMs, software tools for coding, networks)
- Command line, terminal, Linux
- Git + GitHub
- Python
- SQL
- IaC Terraform
- Containerization (Docker)
- Cloud

de-zoomcamp linkedin posts + learnings
de-zoomcamp course solutions (all homework assignments 100%)
de-zoomcamp course leaderboard

- LLM Zoomcamp