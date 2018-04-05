# Quick Instructions (for plain ubuntu:latest docker container from dockerhub.com):

    apt-get update
    apt-get install -y build-essential ruby ruby-dev sudo wget
    
    wget http://rock-robotics.org/autoproj_bootstrap
    ruby autoproj_bootstrap git git@gitlab.spaceapplications.com:InFuse/cdff-buildconf
    
    source env.sh
    autoproj update
    autoproj build
    

# Install instructions for the InFuse software stack

## 0. Create a folder for your rock installation and move in. 

$ mkdir rockInfuse
$ cd rockInfuse

The following commands are executed from the root folder of your Rock installation. 

## 1. Download the autoproj bootstrap script 

$ wget http://www.rock-robotics.org/autoproj_bootstrap

## 2. Run the boostrap script providing the repository address and the branch to install

$ ruby autoproj_bootstrap git git@gitlab.spaceapplications.com:InFuse/cdff-buildconf

## 3. Answer the autoproj_bootstrap questions. This example answers works currently:

Which prepackaged software (a.k.a. 'osdeps') should autoproj install automatically (all, none or a comma-separated list of: os gem pip) ?
    The software packages that autoproj will have to build may require other
    prepackaged softwares (a.k.a. OS dependencies) to be installed (RubyGems
    packages, packages from your operating system/distribution, ...). Autoproj
    is able to install those automatically for you.
    
    Advanced users may want to control this behaviour. Additionally, the
    installation of some packages require administration rights, which you may
    not have. This option is meant to allow you to control autoproj's behaviour
    while handling OS dependencies.
    
    * if you say "all", it will install all packages automatically.
      This requires root access thru 'sudo'
    * if you say "pip", only the Python packages will be installed.
      Installing these packages does not require root access.
    * if you say "gem", only the Ruby packages will be installed.
      Installing these packages does not require root access.
    * if you say "os", only the OS-provided packages will be installed.
      Installing these packages requires root access.
    * if you say "none", autoproj will not do anything related to the
      OS dependencies.
    
    Finally, you can provide a comma-separated list of pip gem and os.
    
    As any configuration value, the mode can be changed anytime by calling
      autoproj reconfigure
    
    Finally, the "autoproj osdeps" command will give you the necessary information
    about the OS packages that you will need to install manually.
    
    So, what do you want ? (all, none or a comma-separated list of: os gem pip) [all] 

The current directory is not empty, continue bootstrapping anyway ? [yes] yes


## 4. Source your rock environment

$ . env.sh

## 5. Update the installation

$ autoproj update

### These questions will be asked:

How should I interact with gitorious.org (http, ssh)
    If you give one value, it's going to be the method used for all access
    If you give multiple values, comma-separated, the first one will be
    used for pulling and the second one for pushing. An optional third value
    will be used to pull from private repositories (the same than pushing is
    used by default) [http,ssh] ssh

 How should I interact with github.com (git, http, ssh)
    If you give one value, it's going to be the method used for all access
    If you give multiple values, comma-separated, the first one will be
    used for pulling and the second one for pushing. An optional third value
    will be used to pull from private repositories (the same than pushing is
    used by default) [http,ssh] ssh

Which flavor of Rock do you want to use ?
    Use 'stable' to use the a released version of Rock that gets updated with bugfixes
    'master' for the development branch
    If you want to use a released version of rock, choose 'stable' and then call 'rock-release switch' after the initial bootstrap
    See http://rock-robotics.org/stable/documentation/installation.html for more information [stable] stable

whether C++11 should be enabled for Rock packages [no] yes

Do you need compatibility with OCL ? (yes or no)
    New Rock users that don't need backward compatibility with legacy Orocos components
    probably want to say 'no'. Otherwise, say 'yes'.
    Saying 'yes' will significantly impact compilation time and the size of the resulting binaries
    Please answer 'yes' or 'no' [no] no

  the target operating system for Orocos/RTT (gnulinux, xenomai, or macosx) [gnulinux] gnulinux

  which CORBA implementation should the RTT use ?
    Answer 'none' to disable CORBA, otherwise pick either tao or omniorb [omniorb] omniorb

## 6. Compile the installation

$ autoproj build






#Configuration of your autoproj build

- CMake
Since everything is CMake based, environment variables such as
CMAKE_PREFIX_PATH are always picked up. You can set them
in init.rb too, which will copy them to your env.sh script.

Because of cmake's aggressive caching behaviour, manual options
given to cmake will be overriden by autoproj later on. To make
such options permanent, add

  package('package_name').define "OPTION", "VALUE"

in overrides.rb. For instance, to set CMAKE_BUILD_TYPE for the rtt
package, do

  package('rtt').define "CMAKE_BUILD_TYPE", "Debug"

- Directory structure

The autoproj/ directory (this directory) contains the files and configuration
that defines the whole build. The manifest file

- Files in the autoproj/ directory

manifest:
  Simple key-value pair file in the YAML format. It lists sources for "package
  sets", other autoproj configuration directories in which packages have been
  declared for you to reuse (package_sets section). It also lists the packages
  that you actually want to build (layout section)

remotes/:
  contains a checkout of the package sets listed in the manifest. You should not
  have to go in there unless you are yourself developing a package set.

config.yml:
  Autoproj can be parametrized by build options. This file is where your
  previous choices for these options are saved. You should not change it manually.
  If you need tou change an option, run
    autoproj reconfigure

overrides.yml:
  Simple key-value pair file in the YAML format.  It allows to override branch
  information for specific packages.  Most people leave this to the default,
  unless they want to use a feature from an experimental branch. See the following
  page for a description of its contents.
    http://www.rock-robotics.org/stable/documentation/autoproj/advanced/importers.html

init.rb:
  Ruby script that contains customization code that will get executed before
  autoproj is loaded.

overrides.rb: 
  Ruby script that contains customization code that will get executed after
  autoproj is loaded, but before the build starts.


