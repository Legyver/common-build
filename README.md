# common-build
Common build files

## publish.gradle
Publish and upload signed artifacts.

Applies the following plugins
- java-library
- maven-publish
- signing

##Prerequisites
### Gradle version 6.X
### GPG
On Windows, you can use gpg4Win https://www.gpg4win.org/
### {gradle.home}/gradle.properties
```properties
signing.gnupg.executable=gpg
signing.gnupg.useLegacyGpg=true
signing.gnupg.homeDir=<userhome>/.gnupg
signing.gnupg.optionsFile=<userhome>/.gnupg/gpg.conf
signing.gnupg.keyName=<key_name>
signing.gnupg.passphrase=<key_password>
ossrhUsername=<oss_username>
ossrhPassword=<oss_password>
```
Putting these values in your gradle home allows them to be shared across projects without checking them in and helps keep your secrets safe.

##Usage
```gradle
//required properties
version = '1.0.0.0'
description = 'My required description'

//optional prefix to be prepended to the libary name ie: library-<project-name> 
parentLibraryPrefix=library

apply from: 'https://raw.githubusercontent.com/Legyver/common-build/1.1/publish.gradle'
```
### Targets
#### printProperties
Prints the OSSRH username and password.  Useful for debugging signing issues.
#### install
Installs artifacts to your mavenLocal without publishing to central
#### publish
Publishes to your staging repository 
