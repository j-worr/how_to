## How to deploy Rails on a VM

Just a quick rails server with sqllite db for some basic testing.

For Ubuntu 22.04

export DEBIAN_FRONTEND=noninteractive
export NEEDRESTART_SUSPEND=1
sudo apt -y remove needrestart
sudo apt update && sudo apt upgrade -y
sudo apt install ruby-full -y
sudo apt install cmdtest -y
DEBIAN_FRONTEND=noninteractive sudo apt-get install libsqlite3-dev -y
sudo apt install nodejs -y
NEEDRESTART_SUSPEND=1  sudo apt install npm -y
sudo gem install rails
sudo gem install sqlite3 -v '1.4.2' --source 'https://rubygems.org/' 
sudo rails new app
cd app
sudo rails s -b 0.0.0.0

At this point, you should be able to reach the rails default on port 3000

Best to reboot before using.

Need restart was removed get everything above to proceed without prompts
suapt apt -y install needrestart
