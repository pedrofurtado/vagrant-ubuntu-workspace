# Vagrant ubuntu workspace

[![Vagrant Box](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/actions/workflows/vagrant.yml/badge.svg)](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/actions/workflows/vagrant.yml)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/pedrofurtado/vagrant-ubuntu-workspace)
[![license](https://img.shields.io/github/license/pedrofurtado/vagrant-ubuntu-workspace.svg)]()

My personal Ubuntu vagrant workspace with VirtualBox provider.

Used only for development purposes.

## Platforms support

This workspace supports Windows as host.

## Installation

Install the following softwares in your PC:

- VirtualBox
- Vagrant

Important note: You will need to run Vagrant as Administrator.

## Usage (example)

Vagrantfile

```ruby
Vagrant.configure('2') do |config|
  config.vm.box          = 'pedrofurtado/vagrant-ubuntu-workspace'
  config.vagrant.plugins = ['vagrant-disksize']
  config.disksize.size   = '100GB'
  config.vm.box_version  = 'x.y.z'
  config.vm.network 'forwarded_port', guest: 9999, host: 9999, id: 'portainer'
  config.vm.network 'forwarded_port', guest: 9998, host: 9998, id: 'k8s_dashboard'
  config.vm.network 'forwarded_port', guest: 1234, host: 1234, id: 'my_app'
  config.vm.provider 'virtualbox' do |v|
    v.memory    = '8192'
    v.cpus      = '2'
    v.customize ['setextradata', :id, 'VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root', '1']
    v.customize ['modifyvm',     :id, '--uartmode1', 'disconnected']
  end
  
  config.vm.provision 'shell', inline: <<-SHELL
    # Your shell script here
  SHELL
end
```

CMD

```bash
vagrant up (run as Administrator)
```

Enjoy!

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/pedrofurtado/vagrant-ubuntu-workspace. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/blob/master/CODE_OF_CONDUCT.md).

## License

The repository is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the vagrant-ubuntu-workspace project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/blob/master/CODE_OF_CONDUCT.md).
