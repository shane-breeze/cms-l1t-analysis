# cms-l1t-analysis
Software package to analyse L1TNtuples

[![Build Status](https://travis-ci.org/cms-l1t-offline/cms-l1t-analysis.svg?branch=master)](https://travis-ci.org/cms-l1t-offline/cms-l1t-analysis) [![DOI](https://zenodo.org/badge/80877637.svg)](https://zenodo.org/badge/latestdoi/80877637) [![Code Health](https://landscape.io/github/cms-l1t-offline/cms-l1t-analysis/master/landscape.svg?style=flat)](https://landscape.io/github/cms-l1t-offline/cms-l1t-analysis/master) [![docs](https://readthedocs.org/projects/cms-l1t-analysis/badge/?version=latest)](http://cms-l1t-analysis.readthedocs.io/en/latest/)


## DEV
 1. Read [CONTRIBUTING.md](https://github.com/cms-l1t-offline/cms-l1t-analysis/blob/master/CONTRIBUTING.md)
 2. Follow the instructions below

### On Scientific Linux 6 with CVMFS available
This includes nodes lxplus.cern.ch & private clusters
```bash
git clone https://github.com/<your github user name>/cms-l1t-analysis.git
cd cms-l1t-analysis
git remote add upstream https://github.com/cms-l1t-offline/cms-l1t-analysis.git
source bin/env.sh
# you will need your grid cert
voms-proxy-init --voms cms
make setup
```

### On OS X/other Linux/Windows
 1. Install [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
 2. Install [Vagrant](https://www.vagrantup.com/downloads.html)
   - You might need to also install `vagrant-libvirt` on some systems if you see "The provider 'libvirt' could not be found, but was requested to
back the machine 'default'. Please use a provider that exists."
 3. Follow instructions below
```bash
git clone https://github.com/<your github user name>/cms-l1t-analysis.git
cd cms-l1t-analysis
git remote add upstream https://github.com/cms-l1t-offline/cms-l1t-analysis.git
# only on non Scientific Linux machines (e.g. OS X, Windows, Ubuntu, etc)
vagrant up
vagrant ssh
cd /vagrant
source bin/env.sh
# you will need your grid cert
voms-proxy-init --voms cms
make setup
```

### running tests
Tests can be run either on an SL 6 machine or in the Vagrant box:
```
make test
# if a grid proxy is provided (e.g. via voms-proxy-init --voms cms)
# you can also run tests that require grid access:
make test-all
```

### running benchmark
```
# install python requirements
pip install -r requirements.txt --user
make benchmark
```

### Generating changelog
```bash
make changelog
```

### Generating documentation (locally)
Documentation is automatically updated on http://cms-l1t-analysis.readthedocs.io/en/latest/
whenever a the master branch is updated. If you want to test documentation locally
execute
```bash
# HTML version
make docs-html # produces output in docs/_build/html
make docs-latex # produces output in docs/_build/latex
# you might need to
# export PATH:/cvmfs/sft.cern.ch/lcg/external/texlive/2014/bin/x86_64-linux:$PATH
# for docs-latex

#### Prerequisites
```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable --ruby
source ~/.rvm/scripts/rvmsource ~/.rvm/scripts/rvm
gem install github_changelog_generator
```

### Implementing and running an analysis script
"Analyzers" are the parts of the code that receive events from the input tuples, extracts the relevant data and puts this into the histograms.

You can see an example of an analyzer at: `cmsl1t/analyzers/demo_analyzer.py`.  
To implement your own analyzer, all you need to do is make a new class in a file under `cmsl1t/analyzers/` which inherits from `cmsl1t.analyzers.BaseAnalyzer.BaseAnalyzer`.  You then need to implement two or three methods: `prepare_for_event`, ` fill_histograms`, `write_histograms`, and `make_plots`.  See the BaseAnalyzer class and the demo_analyzer for examples and documentation of these methods.

Once you have implemented an analyzer and written a simple configuration for it, you can run it with `cmsl1t` command:
```
cmsl1t -c config/demo.yaml -n 1000
```

Get help on the command line options by doing:
```
cmsl1t --help
```
