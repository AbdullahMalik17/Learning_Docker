

## What is Docker?

**Docker** is a containerization platform that allows you to package applications and their dependencies into lightweight, portable containers. Think of it as a way to create consistent, isolated environments for your applications.


### To Use Linux in Window 
**We have to Istall WSL in Window through following command : <br>**
          wsl --install 
### Key Concepts:

1. **Containers**: Lightweight, standalone packages that include everything needed to run an application (code, runtime, system tools, libraries, settings)

2. **Images**: Read-only templates used to create containers. Images are built from a Dockerfile and contain all the necessary components to run an application

3. **Dockerfile**: A text file with instructions on how to build a Docker image

4. **Docker Hub**: A cloud-based registry where you can find and share Docker images

### Why Use Docker?

- **Consistency**: "It works on my machine" becomes "It works everywhere"
- **Isolation**: Applications run in isolated environments, preventing conflicts
- **Portability**: Containers can run on any system that supports Docker
- **Scalability**: Easy to scale applications up or down
- **Resource Efficiency**: Containers share the host OS kernel, making them more efficient than virtual machines

### Common Use Cases:

- **Development**: Consistent development environments across teams
- **Testing**: Isolated test environments
- **Deployment**: Consistent production deployments
- **Microservices**: Breaking applications into smaller, manageable services
- **CI/CD**: Automated testing and deployment pipelines

## Getting Started

This repository contains Docker learning materials and examples.

### Prerequisites

- Docker Desktop installed on your system
- Basic understanding of command line operations

## Basic Command Line Operations in Linux 

### Navigation Commands

| Command | Description | Example |
|---------|-------------|---------|
| `pwd` | Print working directory (shows current location) | `pwd` |
| `ls` | List files and directories | `ls -la` (detailed list) |
| `cd` | Change directory | `cd /home/user` |
| `cd ..` | Go up one directory level | `cd ..` |
| `cd ~` | Go to home directory | `cd ~` |
| `cd /` | Go to root directory | `cd /` |

### File Operations

| Command | Description | Example |
|---------|-------------|---------|
| `touch` | Create empty file | `touch myfile.txt` |
| `mkdir` | Create directory | `mkdir newfolder` |
| `rm` | Remove file | `rm filename.txt` |
| `rm -r` | Remove directory recursively | `rm -r foldername` |
| `cp` | Copy file | `cp source.txt dest.txt` |
| `cp -r` | Copy directory | `cp -r source/ dest/` |
| `mv` | Move/rename file | `mv oldname.txt newname.txt` |
| `cat` | Display file contents | `cat filename.txt` |
| `head` | Show first 10 lines | `head filename.txt` |
| `tail` | Show last 10 lines | `tail filename.txt` |

### Text Editing

| Command | Description | Example |
|---------|-------------|---------|
| `echo` | Print text to terminal | `echo "Hello World"` |
| `echo >` | Write text to file | `echo "text" > file.txt` |
| `echo >>` | Append text to file | `echo "text" >> file.txt` |

## Text Editors: Nano and Vim

### Nano Editor (Beginner-Friendly)

#### Basic Usage
```bash
nano filename.txt          # Open/create a file
nano /path/to/file.txt     # Open file with full path
```

#### Nano Commands (Ctrl + Key)
| Command | Description |
|---------|-------------|
| `Ctrl + X` | Exit nano |
| `Ctrl + O` | Save file (Write Out) |
| `Ctrl + R` | Read another file |
| `Ctrl + W` | Search for text |
| `Ctrl + K` | Cut current line |
| `Ctrl + U` | Paste cut text |
| `Ctrl + G` | Show help |
| `Ctrl + C` | Show current position |

#### Nano Workflow Example
```bash
# Create/edit a file
nano myfile.txt

# Type your content
# Use Ctrl + O to save
# Use Ctrl + X to exit
```

---

### Vim Editor (Advanced)

#### Basic Usage
```bash
vim filename.txt           # Open/create a file
vim /path/to/file.txt      # Open file with full path
```

#### Vim Modes
Vim has **3 main modes**:
1. **Normal Mode** (default) - for navigation and commands
2. **Insert Mode** - for typing text
3. **Command Mode** - for saving/exiting

#### Essential Vim Commands

##### Getting Started
| Command | Description |
|---------|-------------|
| `i` | Enter Insert mode (start typing) |
| `Esc` | Return to Normal mode |
| `:w` | Save file |
| `:q` | Quit vim |
| `:wq` | Save and quit |
| `:q!` | Quit without saving |

##### Navigation (Normal Mode)
| Command | Description |
|---------|-------------|
| `h` | Move left |
| `j` | Move down |
| `k` | Move up |
| `l` | Move right |
| `w` | Move to next word |
| `b` | Move to previous word |
| `0` | Move to beginning of line |
| `$` | Move to end of line |
| `gg` | Move to top of file |
| `G` | Move to bottom of file |

##### Editing (Normal Mode)
| Command | Description |
|---------|-------------|
| `x` | Delete character under cursor |
| `dd` | Delete entire line |
| `yy` | Copy line |
| `p` | Paste after cursor |
| `u` | Undo |
| `Ctrl + R` | Redo |

#### Vim Workflow Example
```bash
# Open vim
vim myfile.txt

# Press 'i' to enter Insert mode
# Type your content
# Press 'Esc' to return to Normal mode
# Type ':wq' to save and quit
```

---

### Quick Comparison

| Feature | Nano | Vim |
|---------|------|-----|
| **Learning Curve** | Easy | Steep |
| **Speed** | Slower | Very Fast |
| **Features** | Basic | Advanced |
| **Best For** | Beginners, quick edits | Power users, programming |
| **Exit Command** | `Ctrl + X` | `:q` or `:wq` |

---

### Practical Examples

#### Creating a Simple Text File with Nano
```bash
# Open nano
nano hello.txt

# Type: Hello World!
# Press Ctrl + O (save)
# Press Ctrl + X (exit)
```

#### Creating a Simple Text File with Vim
```bash
# Open vim
vim hello.txt

# Press 'i' (insert mode)
# Type: Hello World!
# Press Esc (normal mode)
# Type ':wq' (save and quit)
```

#### Editing Existing Files
```bash
# With nano
nano existing_file.txt

# With vim
vim existing_file.txt
```

---

### Pro Tips

#### Nano Tips
- Commands are shown at the bottom of the screen
- `^` means Ctrl key
- Use `Ctrl + G` for help anytime

#### Vim Tips
- **Don't panic!** If you get stuck, press `Esc` then type `:q!` to exit
- Practice the basic commands: `i`, `Esc`, `:wq`
- Use `vimtutor` command for interactive tutorial: `vimtutor`

#### Getting Help
```bash
# Nano help
nano --help

# Vim tutorial
vimtutor

# Vim help inside vim
# Press Esc, then type :help
```

### System Information

| Command | Description | Example |
|---------|-------------|---------|
| `whoami` | Show current user | `whoami` |
| `uname` | Show system information | `uname -a` |
| `df` | Show disk space usage | `df -h` |
| `free` | Show memory usage | `free -h` |
| `ps` | Show running processes | `ps aux` |
| `top` | Show real-time processes | `top` |
| `history` | Show command history | `history` |

### File Permissions

| Command | Description | Example |
|---------|-------------|---------|
| `chmod` | Change file permissions | `chmod 755 filename` |
| `chown` | Change file ownership | `chown user:group file` |
| `ls -l` | Show detailed file info with permissions | `ls -l` |

### Search and Filter

| Command | Description | Example |
|---------|-------------|---------|
| `grep` | Search text in files | `grep "search" file.txt` |
| `find` | Find files/directories | `find . -name "*.txt"` |
| `which` | Find command location | `which docker` |
| `whereis` | Find binary, source, manual | `whereis docker` |

### Network Commands

| Command | Description | Example |
|---------|-------------|---------|
| `ping` | Test network connectivity | `ping google.com` |
| `curl` | Download/upload data | `curl -O https://example.com/file.zip` |
| `wget` | Download files | `wget https://example.com/file.zip` |
| `netstat` | Show network connections | `netstat -tuln` |

### Package Management (Ubuntu/Debian)

| Command | Description | Example |
|---------|-------------|---------|
| `apt update` | Update package list | `sudo apt update` |
| `apt install` | Install package | `sudo apt install docker` |
| `apt remove` | Remove package | `sudo apt remove package` |
| `apt list` | List installed packages | `apt list --installed` |

### Useful Shortcuts

| Shortcut | Description |
|----------|-------------|
| `Ctrl + C` | Cancel current command |
| `Ctrl + D` | Exit terminal/logout |
| `Ctrl + L` | Clear screen |
| `Tab` | Auto-complete commands/filenames |
| `↑/↓` | Navigate command history |
| `Ctrl + R` | Search command history |

### Common Options

| Option | Description | Example |
|--------|-------------|---------|
| `-h` or `--help` | Show help information | `docker --help` |
| `-v` or `--version` | Show version | `docker --version` |
| `-a` | Show all items | `ls -a` |
| `-l` | Long format listing | `ls -l` |
| `-r` | Recursive operation | `rm -r folder` |
| `-f` | Force operation | `rm -f file` |
| `-i` | Interactive mode | `rm -i file` |

### Learning Path

1. [Install Docker](https://docs.docker.com/get-docker/)
2. Learn basic Docker commands
3. Create your first Dockerfile
4. Build and run containers
5. Work with Docker Compose
6. Explore Docker networking and volumes

## Resources

- [Docker Official Documentation](https://docs.docker.com/)
- [Docker Hub](https://hub.docker.com/)
- [Dockerfile Best Practices](https://docs.docker.com/develop/dev-best-practices/)
