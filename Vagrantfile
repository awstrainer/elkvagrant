

Vagrant.configure(2) do |config|
  config.hostmanager.enabled = true

  config.vm.box = "geerlingguy/centos7"

  config.vm.define "elkmaster1", primary: true do |h|
    h.vm.network "private_network", ip: "192.168.136.101"
    h.vm.provision :shell, :inline => <<'EOF'
if [ ! -f "/home/vagrant/.ssh/id_rsa" ]; then
  ssh-keygen -t rsa -N "" -f /home/vagrant/.ssh/id_rsa
fi
cp /home/vagrant/.ssh/id_rsa.pub /vagrant/control.pub

cat << 'SSHEOF' > /home/vagrant/.ssh/config
Host *
  StrictHostKeyChecking no
  UserKnownHostsFile=/dev/null
SSHEOF

chown -R vagrant:vagrant /home/vagrant/.ssh/
EOF
  end
   config.vm.define "elkslave1" do |h|
    h.vm.network "private_network", ip: "192.168.136.102"
    h.vm.provision :shell, inline: 'cat /vagrant/control.pub >> /home/vagrant/.ssh/authorized_keys'
  end
end
