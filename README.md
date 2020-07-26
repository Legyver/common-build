# common-build
Common build files
##Usage
```gradle
plugins {
    id 'java-library'
    id 'maven-publish'
    id 'signing'
}

//required properties
version = '1.0.0.0'
description = 'My required description'

//optional prefix to be prepended to the libary name ie: library-<project-name> 
parentLibraryPrefix=library

apply from: 'https://raw.githubusercontent.com/Legyver/common-build/master/publish.gradle'
```