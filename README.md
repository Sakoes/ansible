# Raspberry Pi Setup w/ ansible
The goal of this repository is to keep track of the installation and configuration of my Raspberry Pi home server and everything what is running on it. By automating this process with Ansible there is a clear and reproduceable overview of the steps taken.
The playbooks themselves and the tools and services they install and run are used to experiment for my own personal development and in my free time. Although I do my best to keep everything working, it is not guaranteed to be bug free or up-to-date as these are a constant work in progress.
Feel free to contact me in case you have questions, want to have a discussion or have any feedback, I'm always happy to talk about technologies which interest me.

More detailed README's can be found in the corresponding directories.

## clean-install
The clean-install playbook can be run on a fresh raspbian install. It does some general configurations on the Raspberry Pi.  
It also configures, downloads and runs diffenrent tools and packages like python, pip, Docker, homer-dashboard, portainer. This playbook uses docker-compose to run most services.

## k3s-install
This playbook downloads k3s and configures the rpi to run k3s. It runs the same tools and services from the _clean-install_ playbook, but uses k3s instead of docker-compose.
