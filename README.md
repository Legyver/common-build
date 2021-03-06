# common-build
Common build files

## publish.gradle
Publish and upload signed artifacts.  Use this one for standalone libraries that are not modular

Applies the following plugins
- java-library
- maven-publish
- signing

### usage
Supports the following targets
- install
    - installs the signed artifacts locally
- publish
    - publishes the signed artifacts to your remote repository

Configuration
- ${gradle.user.home}/gradle.properties
```
ossrhUsername=
```
- build.properties
```groovy
description = 'A required description'

apply from: 'https://raw.githubusercontent.com/Legyver/common-build/1.5/publish.gradle' //java 9+
```

## publishLibraryModule.gradle
Same as above, but has the URLs corrected for multi-module libraries with a root library.
```groovy
description = 'A required description'

apply from: 'https://raw.githubusercontent.com/Legyver/common-build/1.5/publish.LibraryModulegradle' //java 9+ multi-module
```

### usage
Supports the following targets
- install
    - installs the signed artifacts locally
- publish
    - publishes the signed artifacts to your remote repository

Configuration
- ${gradle.user.home}/gradle.properties
```
ossrhUsername=
```
- gradle.properties
```properties
# below is where the .git resource and project url refer to the subproject urls will have '/tree/master/<artifactId>' appended subproject direct linking
parentLibraryPrefix=<parent_library_repo>
```
- build.gradle
```groovy
ext {
    publishBuildScript = 'https://raw.githubusercontent.com/Legyver/common-build/1.5/publishLibraryModule.gradle' //java 9+ multi-module
}
```
- subproject/build.gradle
```groovy
description = 'A required description'

apply from: publishBuildScript
```

## jarsigner.gradle
Signs the jar files in your buildDir/install/<AppName>/lib directory

### usage
Supports the following target
- signDist
    - signs all jars in the buildDir/install/<AppName>/lib directory

Configuration
- build.gradle
```groovy
apply from: 'https://raw.githubusercontent.com/Legyver/common-build/1.4/jarsigner.gradle'
```



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
#if using the jarsigner.gradle script set the following as well
jarsignerAlias=<alias>
jarsignerPassw=<storepass>
```
Putting these values in your gradle home allows them to be shared across projects without checking them in and helps keep your secrets safe.

##Usage
```gradle
//required properties
version = '1.0.0.0'
description = 'My required description'

//optional prefix to be prepended to the libary name ie: library-<project-name>
parentLibraryPrefix=library

//for a single library
apply from: 'https://raw.githubusercontent.com/Legyver/common-build/1.2/publish.gradle'

//for multiple modular libraries under a parent repository
//in the parent project set the hasProperty
ext {
  publishScript = 'https://raw.githubusercontent.com/Legyver/common-build/1.3/publishLibraryModule.gradle'
}
//in each subproject your want to publish add the following
apply from: publishScript

//for signing your library jars in your dist with jarsigner
apply from  'https://raw.githubusercontent.com/Legyver/common-build/1.4/jarsigner.gradle'
```
