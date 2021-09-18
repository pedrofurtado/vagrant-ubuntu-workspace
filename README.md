# Vagrant ubuntu workspace

[![Vagrant Box](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/actions/workflows/vagrant.yml/badge.svg)](https://github.com/pedrofurtado/vagrant-ubuntu-workspace/actions/workflows/vagrant.yml)
![GitHub release (latest by date)](https://img.shields.io/github/v/release/pedrofurtado/vagrant-ubuntu-workspace)
[![license](https://img.shields.io/github/license/pedrofurtado/vagrant-ubuntu-workspace.svg)]()

My personal Ubuntu vagrant workspace with VirtualBox provider. Used only for development purposes.

## Installation

The first step is to install the following softwares in your PC:

- VirtualBox
- Vagrant

## Usage

How to use this box with Vagrant:

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
