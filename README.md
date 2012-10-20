FailFileHandler
===============

A simple [Chef Handler](http://wiki.opscode.com/display/chef/Exception+and+Report+Handlers) that drops a file upon a failed run and deletes it on success.

We use this as a simple method to communicate to other automation that Chef has failed and to take action.

### Installation:

There are a [few ways to distribute](http://wiki.opscode.com/display/chef/Distributing+Chef+Handlers) Chef handlers, I opt for using the super handy [chef_handler cookbook](https://github.com/opscode-cookbooks/chef_handler).

Grab the cookbook and drop the `FailFileHandler.rb` script into `chef_handler/files/default/handlers`.

In the `chef_handler/recipes/default.rb` file use the chef_handler resource to setup the handler thus:

```ruby
failFile = [
  :failFile => "/var/tmp/chef_run_failed"
]

chef_handler "FailFileHandler" do
  source "#{node['chef_handler']['handler_path']}/FailFileHandler.rb"
  arguments failFile
  action :enable
end
```

### Usage:

It's pretty easy from here and you can use your imagination as to what to do. I use it for NRPE checks, because I'm too lazy to setup NSCA.

```bash
#!/bin/bash
if [ ! -f /var/tmp/chef_run_failed ]; then
	echo "OK"
else
	echo "Chef has failed."
fi
```

