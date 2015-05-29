
Vagrant.configure(2) do |config|

config.vm.provider :digital_ocean do |provider, override|
	override.ssh.private_key_path = '~/.ssh/id_rsa'
	override.vm.box = 'digital_ocean'
	override.vm.box_url = "https://github.com/smdahlen/vagrant-digitalocean/raw/master/box/digital_ocean.box"

	provider.token = ${DIGITAL_OCEAN_TOKEN_CHRIS}
	provider.image = 'ubuntu-14-04-x64'
	provider.region = 'nyc2'
	provider.size = '512mb'
	end

config.vm.box = "ubuntu/trusty64"
config.vm.network "forwarded_port", guest: 3838, host: 3838
config.vm.provider "virtualbox" do |vb|
vb.customize ["modifyvm", :id, "--memory", "2048"]
vb.customize ["modifyvm", :id, "--cpus", "2"]
end

$script = <<BOOTSTRAP
sudo apt-get update
sudo apt-get -y install git gcc
sudo apt-get -y install libxml2-dev libcurl4-openssl-dev libssl0.9.8 libcairo2-dev
sudo add-apt-repository ppa:marutter/rrutter
sudo apt-get update
sudo apt-get -y install r-base r-base-dev
sudo R -e "install.packages('shiny', repos = 'http://cran.rstudio.com/', dep = TRUE)"
sudo R -e "install.packages('rmarkdown', repos = 'http://cran.rstudio.com/', dep = TRUE)"
sudo apt-get -y install gdebi-core
wget http://download3.rstudio.org/ubuntu-12.04/x86_64/shiny-server-1.2.3.368-amd64.deb
sudo gdebi shiny-server-1.2.3.368-amd64.deb
sudo dpkg -i *.deb
rm *.deb
sudo ln -s /vagrant/apps /srv/shiny-server
BOOTSTRAP

config.vm.provision :shell, :inline => $script
end
