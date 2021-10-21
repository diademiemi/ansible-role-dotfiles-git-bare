# Ansible Role: Dotfiles with Git bare
Ansible role to install dotfiles using a Git bare repository. No ugly symlinks, and easy to add new files!  
By default, this will install [my dotfiles](https://github.com/diademiemi/cfg), but you can define any Git repository.  
This repository should be structured as a home directory, for example, it should look like this:
```
repo-root/
	.config/
		.ranger/
			rc.conf
	.vimrc
	.zshrc
```
If any submodules are contained within these directories, these will be cloned too.  

## Requirements
This requires Git to be installed on the system, this playbook will attempt to install Git with `ansible.builtin.pkg`.  

## Warning
**This role will overwrite any existing files!**  

## Information
This role uses Git's bare repository mechanism, this allows cloning a repository and checking it out in a separate directory. This way, you don't need to symlink your dotfiles, and they're easy to update.  
You can add a new file to this repository by running the following:  
```
git --git-dir=/home/diademiemi/.cfg --work-tree=$HOME add path/to/some/file
```
###### Using default variables from the Variables section, please replace according to your setup

to commit this, run:
```
git --git-dir=/home/diademiemi/.cfg --work-tree=$HOME commit -m "message"
```
###### using default variables from the variables section, please replace according to your setup

then to push it, you can run:
```
git --git-dir=/home/diademiemi/.cfg --work-tree=$HOME push
```
###### Using default variables from the Variables section, please replace according to your setup

I recommend setting an alias of the following,
```
alias cfg='git --git-dir=/home/diademiemi/.cfg --work-tree=$HOME'
```
This sets an alias to cfg, this way you can use the `cfg` command as if it were the normal `git` command
Then you can run commands like `cfg status`, `cfg add`, `cfg commit -m "message"`, `cfg push`  

## Variables
The following variables are used, see `defaults/main.yml`: 
```
dotfiles_repo: "https://github.com/diademiemi/cfg.git"
```
This sets the URL for the Git repository to clone.  

```
dotfiles_repo_directory: "/home/diademiemi/.cfg"
```
This is where the bare repository will be cloned to, you should never have to interact with this, so just set it to any non-existant path your user can write to.  

```
dotfiles_user: "diademiemi"
```  
This determines which users home directory these files will be checked out to, and which user will own the files.  

```
dotfiles_checkout:
  - .zshrc
  - .zsh/
  - .vimrc
```
This is a list of the files which will be checked out, this will recurse into directories. If any submodules are contained within these directories, these will be cloned too.  
To check out all files, delete this variable or create just one entry containing `/*`, like this:  
```
dotfiles_checkout:
  - /*
```

## License
[MIT](./LICENSE)
