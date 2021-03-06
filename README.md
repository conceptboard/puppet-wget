[![Build Status](https://travis-ci.org/maestrodev/puppet-wget.svg?branch=master)](https://travis-ci.org/maestrodev/puppet-wget)
[![Puppet Forge](https://img.shields.io/puppetforge/v/maestrodev/wget.svg)](https://forge.puppetlabs.com/maestrodev/wget)
[![Puppet Forge](https://img.shields.io/puppetforge/f/maestrodev/wget.svg)](https://forge.puppetlabs.com/maestrodev/wget)
[![Puppet Forge](https://img.shields.io/puppetforge/e/maestrodev/wget.svg)](https://forge.puppetlabs.com/maestrodev/wget)



A Puppet module to download files with wget, supporting authentication.

# Example

install wget:

```puppet
    include wget
```

```puppet
    wget::fetch { "download Google's index":
      source      => 'http://www.google.com/index.html',
      destination => '/tmp/index.html',
      timeout     => 0,
      verbose     => false,
    }
```
or alternatively: 

```puppet
    wget::fetch { 'http://www.google.com/index.html':
      destination => '/tmp/index.html',
      timeout     => 0,
      verbose     => false,
    }
```
This fetches a document which requires authentication:

```puppet
    wget::fetch { 'Fetch secret PDF':
      source      => 'https://confidential.example.com/secret.pdf',
      destination => '/tmp/secret.pdf',
      user        => 'user',
      password    => 'p$ssw0rd',
      timeout     => 0,
      verbose     => false,
    }
```

This caches the downloaded file in an intermediate directory to avoid
repeatedly downloading it. This uses the timestamping (-N) and prefix (-P)
wget options to only re-download if the source file has been updated.

```puppet
    wget::fetch { 'https://tool.com/downloads/tool-1.0.tgz':
      destination => '/tmp/tool-1.0.tgz',
      cache_dir   => '/var/cache/wget',
    }
```

It's assumed that the cached file will be named after the source's URL
basename but this assumption can be broken if wget follows some redirects. In
this case you must inform the correct filename in the cache like this:

```puppet
    wget::fetch { 'https://tool.com/downloads/tool-latest.tgz':
      destination => '/tmp/tool-1.0.tgz',
      cache_dir   => '/var/cache/wget',
      cache_file  => 'tool-1.1.tgz',
    }
```

# Building

Testing is done with rspec, [Beaker-rspec](https://github.com/puppetlabs/beaker-rspec), [Beaker](https://github.com/puppetlabs/beaker))

To test and build the module

    bundle install
    # run specs
    rake

    # run Beaker system tests with vagrant vms
    rake beaker
    # to use other vm from the list spec/acceptance/nodesets and not destroy the vm after the tests
    BEAKER_destroy=no BEAKER_set=centos-65-x64-docker bundle exec rake beaker

    # Release the Puppet module to the Forge, doing a clean, build, tag, push, bump_commit and git push
    rake module:release

# License

Copyright 2011-2013 MaestroDev

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
