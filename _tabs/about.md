---
# the default layout is 'page'
icon: fas fa-info-circle
order: 4
---
## Overview

This repository website provides a comprehensive tutorial for configuring and integrating **Docker** with **Ignition** by **Inductive Automation**, using **Visual Studio Code** as the primary development environment and **Git Bash** for command-line operations.

## Objectives

- Setup and configuration of Docker to run containerized Ignition Gateways.
- Git Bash command-line execution.
- Integration via Visual Studio Code and supplementary extensions.
- SCADA deployment in a containerized environment.

## Prerequisites

Listed below are all of the prerequisites that need to be installed and configured:

1. Docker Desktop
	- Version: 20.x or higher.
	- Verify Docker version using:
		`docker --version`	
		
2. Visual Studio Code
	- Extensions Required:
		- Docker.
		- Remote - Containers.
		- YAML (for editing Docker Compose files).
3. Git Bash
	- Version: 2.x or higher.
	- Verify installation with:
		`git --version`	
		
4. Ignition Installer & Docker Image
	- Ignition version: 8.x.
	- Official Docker Image:
		`inductiveautomation/ignition`	
		
5. Fundamental theory of:
	- Docker and Docker Compose.
	- Git and version control workflows.
	- SCADA systems and Ignition basics.

## Repository Structure

```
.
├── index.md               # Base index markdown file
├── Git_Bash.md            # Git Bash markdown file
├── Docker.md              # Docker markdown file
├── VS_Code.md             # Visual Studio Code markdown file
├── Gemfile                # Jekyll Bundle installer
├── Gemfile.lock           # Predefined Gemfile attributes for Ruby 3.2
├── _config.yml            # Configuration file for GitHub Action Workflow
├── .gitignore             # Ignored pushed assets.

//Other folders required for theme build (_includes, _sass, assets)
```


## Content Covered

1. **Docker Basics for Ignition**
    
    - Pulling and running the Ignition Docker image.
    - Using `docker-compose` for multi-container setups.
    
2. **Configuring Ignition in a Containerized Environment**
    
    - Managing persistent data with Docker volumes.
    - Customizing Ignition settings for optimal performance in SCADA environments.
    
3. **Using VS Code with Docker**
    
    - Editing Docker Compose and configuration files within VS Code.
    - Debugging and managing containers directly from VS Code's Docker extension.
    
4. **Git Bash for Streamlined Commands**
    
    - Automating repetitive tasks like starting, stopping, and rebuilding containers.
    - Using Git Bash to manage repository changes and execute scripts.
    
5. **Troubleshooting Common Issues**
    
    - Resolving container networking challenges for Ignition.
    - Debugging Ignition Gateway errors using logs and Docker tools.

