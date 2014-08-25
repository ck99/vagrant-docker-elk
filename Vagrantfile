# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "phusion-open-ubuntu-14.04-amd64"
  config.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box"
  config.vm.network :forwarded_port, host: 8080, guest: 80
  config.vm.network :forwarded_port, host: 8500, guest: 5000
  config.vm.network :forwarded_port, host: 8333, guest: 3333

  config.vm.provider :vmware_fusion do |f, override|
    override.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vmwarefusion.box"
  end

  if Dir.glob("#{File.dirname(__FILE__)}/.vagrant/machines/default/*/id").empty?
    # Install Docker
    pkg_cmd = "wget -q -O - https://get.docker.io/gpg | apt-key add -;" \
      "echo deb http://get.docker.io/ubuntu docker main > /etc/apt/sources.list.d/docker.list;" \
      "apt-get update -qq; apt-get install -q -y --force-yes lxc-docker; "
    # Add vagrant user to the docker group
    pkg_cmd << "usermod -a -G docker vagrant; "
    
    config.vm.provision :shell, :inline => pkg_cmd
    
    elk_stack_init =  "docker pull ck99/elasticsearch ; docker pull ck99/logstash; docker pull ck99/kibana; "
    elk_stack_init << "docker run --name elasticsearch -d ck99/elasticsearch;"
    elk_stack_init << "docker run --name logstash --link elasticsearch:elasticsearch -p 5000:5000 -p 3333:3333 -d ck99/logstash; "
    elk_stack_init << "docker run --name kibana --link elasticsearch:elasticsearch -p 80:80 -d ck99/kibana;"
    
    config.vm.provision :shell, :inline => elk_stack_init
  end
end
