# Configuration Settings
TuxLab has two different sets of settings; server, and application.  Server settings define the overall behavior of the set of applications (like domain, email and mongodb server settings, etc).  Application settings define the function of the web application.  Both can be edited through a series of configuration files in the tuxlab-infra and tuxlab-app repositories respectively.

## Server Configuration
All of the server settings are defined in `config.yml` in the root of the `tuxlab-infra` repository. As Ansible sets up each server, it downloads a copy of this file from the GIT_URL (defined below), so if you want these variable changes to propogate across servers (Required for AWS Installation, but not Development Enviornments), it is imperative that you fork the original tuxlab-infra repository (https://github.com/learnlinux/tuxlab-infra), and include this link as the GIT_URL.

Because the server configuration variables are critical to operation, you will likely need to change the majority of them before proceeding.  A guide to the various settings and options is printed below:

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRR4mCglkjOeJ_enHmKDxy3OgKvKqUYf91fJyflmM5NUys27f7HsTlVpaHH83u7oYsRyOF75djv3qNn/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" style="width:48%; height:1200px; margin-left:10px;"></iframe>

## Application Configuration
The application-specific settings are defined in a number of .json files in the `tuxlab-app/private` folder.  Unlike the server settings, in order for these to take effect, you need to create a forked repository for both Development and Production environments.  Be sure that in doing this, you update the APP_REPO_GIT or APP_REPO_BIN options in the server settings.

<iframe src="https://docs.google.com/spreadsheets/d/e/2PACX-1vRFna9Fy_ZXhZS0ZqJb3CM7zy17KBQvszEFFgC2vufTvb2F97DHFzQ1VnOEdzdQHe7UsXPPmdWujuYo/pubhtml?gid=0&amp;single=true&amp;widget=true&amp;headers=false" style="width:48%; height:500px; margin-left:10px;"></iframe>
