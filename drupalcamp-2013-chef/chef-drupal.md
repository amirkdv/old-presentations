<h2 style="margin:100px auto;font-size:2.2em; font-weight:bold;">
  Cooking Drupal with Chef
</h2>
<div style="position: absolute; width:900px; bottom:1em;right:1em;">
  <img src="resources/img/ew-good.png" style="height:130px; float:right;">
  <code style="float:right; font-size:0.7em;">
    <div style="font-size:1.2em; font-weight:bold;">Amir Kadivar </div>
    <div> amir@evolvingweb.ca </div>
    <div> github.com/amirkdv </div>
  </code>
</div>

--end--

## Outline

* Automated Infrastructure: Problem Definition 
* Virtualization
* Configuration Management
* Chef
* Chef for Drupal

--end--

## Problem Definition

### development environments
* consistency (with production and other devs)
* choice of tools (limiting developers)
* multiple platforms

### testing and integration
* is every commit tested?
* test environment (testing a web application leaves a lot of cruft)

### deployment
* how do you go from zero to live site?
* how frequent are changes pushed to production?

--end--

## Enters Virtualization

* processes sharing hardware → O/S s sharing hardware
* A VM is an _environment_ that a VM Monitor creates
* These _environments_ should, ideally, be:
  * isolated from each other
  * equivalent to a real machine
  * as efficient as a real machine
* We have achieved all of these!
--end--

## Virtualization Technologies

* Types of VM Monitors
* Methods of Virtualization
* why is <img src="resources/img/vagrant-w-name.png" style="display:inline; vertical-align:middle;" height="100px;"/> relevant?

--end---

## How does Virtualization Help?

<!-- * VMs are cheap to build and to throw away! -->

### development:
* VM for every project
* VM for each developer

### testing & integration:
* spin up throwaway VM for tests, as frequently as you wish

### deployment:
* crufty prod? start from fresh VM
* switch platform? start from fresh VM

--end--

## Missing Piece: Provisioning

### VMs are cheap but:
* how much work is it to configure a VM from _scratch_?
* <div style="font-size:0.9em;">
  how much of this work is reusable for a similar
  _but not identical_ use case?</div>
* <div style="font-size:0.9em;">
  how do you maintain _consistent_ configuration
  accross different machines?
  </div>


--end--

## Config Management tools

* Config described in a programming (instead of human) language:
  * version control
  * reusable
* High level programming language instead of shell scripts
  * cross-platform configuration
  * more readable/maintainable

--end--

### <img src="resources/img/chef.png" width="400px"/> 
* What is it exactly?
* Concepts:
  * State
  * Resources
  * Idempotence
--end--

## How does <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/> work?
* Recipes and Cookbooks
* Run Lists
* Attributes
* Assets: Files and Templates
* Nodes and C/S Chef
---end---

## Ruby

    #ruby
    # a function
    def double (x)
      return 2*x
    end 
    
    puts double 12 # => 24
    
--end----
## Ruby 
    #ruby 
    if File.exists? '/foo/bar.php'
      puts "found it"
    end
     
    puts "found it" if File.exists? 'foo'

---end---
## Ruby
    #ruby
    ['John','Mary', 'Bob'].each do |guest|
      puts 'Pleased to meet you #{guest}!'
    end
----end---

## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/>Resources
    
    #ruby
    directory "/path/to/some/directory" do
      owner "user1"
      group "group1"
    end
----end---

## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/>Resources

    #ruby
    execute "some-task" do
      cwd "/path/to/some/directory"
      command "command --with-some options"
      creates "/path/to/some/file"
    end
---end---
## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/>an idempotent resource
<div style="font-size:0.6em">

    #ruby
    execute "my-important-file" do
      command "cp -Rf /vagrant/. #{node['drupal-camp']['the-directory']}/"
      creates node['drupal-camp']['the-directory'] + "/foo.txt"
    end

---end---
## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/>Attributes
    
    #json
      {
        "mysql": {
          "server_root_password": "root",
        },
        "apache2":{
          "listen_ports": ["80","8000"]
        }
      }
---end---
## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/>Templates
<div style="font-size:0.6em">

    #ruby
    template "/path/to/install/foo.sh" do
      source "foo.sh.erb"
      variables({
        :user => node['apache']['user'], # => www-data
        :group => node['deploy']['dev_group'], # => sysadmin
      })
    end
----end----
## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle; margin:0;" height="150px;"/>Templates
`my_template.sh.erb`:
    
    #bash
    # script generated by Chef
    #! /bin/bash
    chown <%= @user %>:<%= @group %> /var/www

`/path/to/install/foo.sh`:

    #bash
    # script generated by Chef
    #! /bin/bash
    chown www-data:sysadmin /var/www

--end---
## Let's see a simple Chef Run

-----end---
## <img src="resources/img/chef-logo.png" style="display:inline; vertical-align:middle;"height="250px;"/> + <img src="resources/img/drupal.png" style="vertical-align:middle;display:inline;"height="250px;"/>
### Platform configuration
* HTTP Server(s)
* Database
* PHP
* APC, memcached, Search, Reverse Proxy, Firewall, ...

### Drupal configuration
* download Drupal and connect to database
* deploy existing project (codebase + database data)
* manage modules
--end---

## Case Studies
### deploy-drupal
[github.com/evolvingweb/chef-deploy-drupal](http://github.com/evolvingweb/chef-deploy-drupal)
### drupal-solr
[github.com/evolvingweb/chef-drupal-solr](http://github.com/evolvingweb/chef-drupal-solr)

--end----
# Thank You!
--end---
