+++
title = "Next Steps"
weight = 150
toc = true
+++

At this point you should have a working MCollective set up with your user able to do any command of the plugins we deployed.

## Confirming it is working

From the shell you set up the user in lets check the version of _puppet-agent_ installed on your nodes:

```bash
$ mco package status puppet-agent

 * [ ============================================================> ] 15 / 15

                 dev1.example.net: puppet-agent-1.8.0-1.el7.x86_64
                 dev2.example.net: puppet-agent-1.8.0-1.el7.x86_64
                 dev3.example.net: puppet-agent-1.8.0-1.el7.x86_64

Summary of Arch:

   x86_64 = 3

Summary of Ensure:

   1.8.0-1.el7 = 3


Finished processing 3 / 3 hosts in 71.63 ms
```

MCollective knows a lot about your nodes such as all their Facts and Puppet Classes deployed to them, you
can view what it knows about a certain node, this should include your classes, facts and agents deployed:

{{% notice tip %}}
MCollective identities match the certificate names from Puppet
{{% /notice %}}

```bash
$ mco inventory dev1.example.net

Inventory for dev1.example.net:

   Server Statistics:
                      Version: 2.9.1
                   Start Time: 2016-12-16 21:35:20 +0100
                  Config File: /etc/puppetlabs/mcollective/server.cfg
                  Collectives: de_collective, mcollective
              Main Collective: mcollective
                   Process ID: 13127
               Total Messages: 12
      Messages Passed Filters: 12
            Messages Filtered: 0
             Expired Messages: 0
                 Replies Sent: 11
         Total Processor Time: 35.85 seconds
                  System Time: 16.33 seconds

   Agents:
      discovery       filemgr         package
      puppet          rpcutil         service

   Data Plugins:
      agent           collective      fact
      fstat           puppet          resource
      service

   Configuration Management Classes:
    mcollective                           mcollective::config
    mcollective::facts                    mcollective::packager
    mcollective::plugin_dirs              mcollective::service
    mcollective_agent_filemgr             mcollective_agent_iptables
    mcollective_agent_package             mcollective_agent_puppet
    mcollective_agent_puppetca            mcollective_agent_service
    mcollective_choria                    mcollective_util_actionpolicy

   Facts:
      aio_agent_version => 1.8.0
      architecture => x86_64
      augeas => {"version"=>"1.4.0"}
      augeasversion => 1.4.0
      ...
```

You can get a quick report of values for some fact (add -v for node names):

```bash
$ mco facts aio_agent_version
Report for fact: aio_agent_version

        1.8.0                                    found 3 times

Finished processing 3 / 3 hosts in 18.61 ms
```

We can check that the Puppet service is up:

```bash
$ mco service status puppet

 * [ ============================================================> ] 3 / 3

   dev3.example.net: running
   dev2.example.net: running
   dev1.example.net: running

Summary of Service Status:

   running = 3


Finished processing 3 / 3 hosts in 98.98 ms
```

And when last Puppet ran on these nodes:

```bash
$ mco puppet status

 * [ ============================================================> ] 3 / 3

   dev1.example.net: Currently idling; last completed run 6 minutes 20 seconds ago
   dev2.example.net: Currently idling; last completed run 10 minutes 04 seconds ago
   dev3.example.net: Currently idling; last completed run 25 seconds ago

Summary of Applying:

   false = 3

Summary of Daemon Running:

   running = 3

Summary of Enabled:

   enabled = 3

Summary of Idling:

   true = 3

Summary of Status:

   idling = 3


Finished processing 3 / 3 hosts in 28.27 ms
```

## Further reading

There is much to learn about MCollective, at this point if all worked you'll have a setup
that is secured by using TLS on every middleware connection and every user securely identified
and authorized via the Puppet SSL certificates with auditing and logging.

Choria has a few optional features related to integration with PuppetDB and Puppet Server,
please see the [optional configuration](http://choria.io/docs/configuration/) section. You should
also read about [Choria Playbooks](http://choria.io/docs/playbooks).

The official documentation is a good resource for usage details:

  * I strongly suggest you read about [using the mcollective CLI](https://docs.puppet.com/mcollective/reference/basic/basic_cli_usage.html)
  * Read about [filtering which hosts to act on](https://docs.puppet.com/mcollective/reference/ui/filters.html)
  * Read about [managing Puppet](https://github.com/puppetlabs/mcollective-puppet-agent#usage) though note the setup steps are already completed
  * Read about [managing packages](https://github.com/puppetlabs/mcollective-package-agent#readme) though note the setup steps are already completed
  * Read about [managing services](https://github.com/puppetlabs/mcollective-service-agent#readme) though note the setup steps are already completed
  * Read about [managing files](https://github.com/puppetlabs/mcollective-filemgr-agent#readme) and this setup too is already completed

These plugins that manage Service, Package, Puppet and Files are just plugins and they make the
core feature set of Mcollective.  You can write your own plugins, deploy them and interact with
them.  There is an [official guide about this](https://docs.puppet.com/mcollective/simplerpc/agents.html).
Writing your own lets you solve your deployment problems using MCollective.

## Getting in touch

I am on the Puppet IRC as _Volcane_ and on slack as _ripienaar_, we also have a _#mcollective_ channel on Freenode.

If you wish to file a ticket about anything here or improve something the 2 GitHub project are [ripienaar/mcollective-choria](https://github.com/ripienaar/mcollective-choria)
and [ripienaar/puppet-mcollective](https://github.com/ripienaar/puppet-mcollective) where you can file issues.

