# jenkins-puppet 
### Environment Setup
Assumes linux or Windows with Cygwin or other GNU tools. This project was built on Windows 10 with Cygwin.
1. Install VirtualBox
2. Install Vagrant
3. Install vagrant scp plugin
```
vagrant plugin install vagrant-scp
```
4. Clone Git repo
```
git clone https://github.com/ericsopa/jenkins-puppet.git
```

### Installation
#### Terms
 * host = Computer running Virtualbox
 * vagrant = user to login to VMs
 * master = VM with Puppet Server
 * agent = VM with Puppet Agent

```
[host:/~/src/jenkins-puppet/vms] $ vagrant up
[host:/~/src/jenkins-puppet/vms] $ make all
[host:/~/src/jenkins-puppet/vms] $ vagrant ssh master
                      ------- switch host -------
                [vagrant@master] $ cd jenkins-puppet/share
                [vagrant@master] $ make master-all
                      ------- switch host -------
[host:/~/src/jenkins-puppet/vms] $ vagrant ssh agent
                      ------- switch host -------
                 [vagrant@agent] $ cd jenkins-puppet/share
                 [vagrant@agent] $ make agent-all
                      ------- switch host -------
                [vagrant@master] $ make gen-certs
                      ------- switch host -------
                 [vagrant@agent] $ make gen-csr
                      ------- switch host -------
                [vagrant@master] $ make master-cert-sign
                      ------- switch host -------
                 [vagrant@agent] $ make agent-fingerprint
                      ------- switch host -------
                [vagrant@master] $ make java-module
                [vagrant@master] $ make jenkins-module
                [vagrant@master] $ make manifest
                      ------- switch host -------
                 [vagrant@agent] $ sudo puppet agent -t
```
### Test
On host, open a web browser and connection to:

 * http://10.0.0.10:8000

Jenkins should be listening on port 8000.
