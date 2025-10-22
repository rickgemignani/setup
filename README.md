# Setup Repositories

This repository contains all my development environment setup scripts and configurations organized as git submodules.

## Repository Structure

This repository uses git submodules to organize the different setup repositories:

- **[MacOs-Setup](MacOs-Setup/)** - macOS development environment setup scripts
- **[Windows-Setup](Windows-Setup/)** - Windows development environment setup scripts  
- **[dotfiles](dotfiles/)** - Personal dotfiles and shell configurations

## Quick Start

### Clone the repository with all submodules:

```bash
git clone --recursive https://github.com/rickgemignani/setup.git
cd setup
```

### If you already cloned without submodules:

```bash
git submodule update --init --recursive
```

## Individual Setup Repositories

### macOS Setup
The macOS setup includes:
- Homebrew package management
- Xcode Command Line Tools
- GUI applications (VS Code, Chrome, Docker, etc.)
- Development tools and CLI packages
- macOS system defaults configuration
- Dock and Finder preferences

**Usage:**
```bash
cd MacOs-Setup
./install.sh
```

### Windows Setup  
The Windows setup includes:
- Chocolatey package management
- PowerShell 7 installation
- Windows Update configuration
- Development tools and applications
- User account setup scripts

**Usage:**
```powershell
cd Windows-Setup
.\install.ps1
```

### Dotfiles
Personal shell configurations and CLI tools:
- Zsh and Bash configurations
- Git configurations
- Tmux configuration
- Vim configuration
- CLI tool installations

**Usage:**
```bash
cd dotfiles
./install.sh
```

## Working with Submodules

### Updating all submodules to latest versions:
```bash
git submodule update --remote --merge
```

### Updating a specific submodule:
```bash
git submodule update --remote MacOs-Setup
```

### Making changes to a submodule:
```bash
cd MacOs-Setup
# Make your changes
git add .
git commit -m "Your changes"
git push origin main
cd ..
git add MacOs-Setup
git commit -m "Update MacOs-Setup submodule"
git push origin main
```

### Pulling latest changes for all submodules:
```bash
git pull --recurse-submodules
```

## Repository Information

- **Main Repository**: [https://github.com/rickgemignani/setup](https://github.com/rickgemignani/setup)
- **macOS Setup**: [https://github.com/rickgemignani/MacOs-Setup](https://github.com/rickgemignani/MacOs-Setup)
- **Windows Setup**: [https://github.com/rickgemignani/Windows-Setup](https://github.com/rickgemignani/Windows-Setup)
- **Dotfiles**: [https://github.com/rickgemignani/dotfiles](https://github.com/rickgemignani/dotfiles)

## Contributing

Each submodule is a separate repository with its own contribution guidelines. Please refer to the individual README files in each submodule for specific contribution instructions.

## License

Each submodule has its own license. Please check the individual LICENSE files in each repository.
