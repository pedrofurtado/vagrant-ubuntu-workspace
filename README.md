# Vagrant ubuntu workspace

[![Vagrant Box](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/actions/workflows/vagrant.yml/badge.svg)](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/actions/workflows/vagrant.yml)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/pedrofurtado/vagrant-ubuntu-workspace)
[![license](https://img.shields.io/github/license/pedrofurtado/vagrant-ubuntu-workspace.svg)]()

My personal Ubuntu vagrant workspace with VirtualBox provider.

Used only for development purposes.

## Platforms support

This workspace supports Windows as host.

## Installation

The first step is to install the following softwares in your PC:

- VirtualBox
- Vagrant

## Usage

Vagrantfile

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "pedrofurtado/vagrant-ubuntu-workspace"
end
```

Shell

```bash
vagrant init pedrofurtado/vagrant-ubuntu-workspace
vagrant up
```

Build from scratch

```bash
git clone https://github.com/pedrofurtado/vagrant-ubuntu-workspace
cd vagrant-ubuntu-workspace/
vagrant up
```

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/pedrofurtado/vagrant-ubuntu-workspace. This project is intended to be a safe, welcoming space for collaboration, and contributors are expected to adhere to the [code of conduct](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/blob/master/CODE_OF_CONDUCT.md).

## License

The repository is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Code of Conduct

Everyone interacting in the vagrant-ubuntu-workspace project's codebases, issue trackers, chat rooms and mailing lists is expected to follow the [code of conduct](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/blob/master/CODE_OF_CONDUCT.md).
