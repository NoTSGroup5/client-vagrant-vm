Vagrant.configure(2) do |config|
    provisioner = Vagrant::Util::Platform.windows? ? :guest_ansible : :ansible

    config.vm.network "forwarded_port", guest: 3000, host: 3000, protocol: "tcp"

    config.vm.define "hyperledger" do |hyperledger|
        hyperledger.vm.box = "ubuntu/xenial64"
        hyperledger.vm.hostname  = "hyperledger.dev"
        hyperledger.vm.network :private_network, ip: "192.168.33.151"

        # Tell vagrant to run ansible as a provisioner
        hyperledger.vm.provision provisioner do |ansible|
            # where the playbook is located
            ansible.playbook = "provisioning/playbook.yml"
        end
    end

    # Access the shared vagrant directory via NFS, otherwise slow on mac and windows
    config.vm.synced_folder ".", "/vagrant", type: "nfs"

    config.vm.provider "virtualbox" do |v|
        v.memory = 2048
        v.cpus = 2
    end
end
