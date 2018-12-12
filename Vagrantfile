# Sets environment variables from a file
env_file = File.dirname(__FILE__) + '/.env'
env_example_file = File.dirname(__FILE__) + '/.env.example'
FileUtils.cp(env_example_file, env_file) unless File.exist?(env_example_file)
File.open(env_file).each do |line|
  /^(?![#\s])^(.+)=(.*)$/.match(line) do |m|
    ENV[m[1]] = m[2]
  end
end

# Shell provision data
once_as_root_script = <<-SH
  echo "--> Run the provisioning script by the user: `whoami`"

  echo "--> Install Docker"
  curl -sSL get.docker.com -o docker-setup.sh
  sh docker-setup.sh
  rm docker-setup.sh

  echo "--> Install Docker Compose"
  curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
  chmod +x /usr/local/bin/docker-compose
SH
once_as_vagrant_script = <<-SH
  echo "--> Run the provisioning script by the user: `whoami`"

  echo "--> Set the default folder after start shell sessions"
  { echo; echo "cd /vagrant/docker"; } | tee -a ~/.bashrc
SH
always_as_root_script = <<-SH
  echo "--> Run the provisioning script by the user: `whoami`"

  echo "--> Run the project with Docker Compose"
  cd /vagrant/docker
  docker system prune -f
  docker-compose up --build
SH

# Vagrant configuration
Vagrant.configure(2) do |config|
  # Required box of every environment
  config.vm.box = 'bento/ubuntu-16.04'

  # The machine IP
  config.vm.network 'private_network', ip: ENV['VM_IP']

  # General configuration
  config.vm.provider 'virtualbox' do |vb|
    vb.gui = ENV['VM_GUI'].downcase == 'true'
    vb.memory = ENV['VM_MEMORY']
    vb.cpus = ENV['VM_CPUS']
  end

  config.vm.boot_timeout = 600

  # Sets a default timezone
  timezone = ENV['TZ']

  # Shell provision
  config.vm.provision 'shell', inline: once_as_root_script
  config.vm.provision 'shell', inline: once_as_vagrant_script, privileged: false
  config.vm.provision 'shell', inline: always_as_root_script, run: 'always'
end
