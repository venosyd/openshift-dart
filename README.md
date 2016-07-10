# Dart cartridge for OpenShift

This repository provides a [Dart](https://www.dartlang.org/) based downloadable cartridge for OpenShift.
This cartridge will work on OpenShift Online, and Origin (CentOS).

## Creating an application with this downloadable cartridge

You can create an application using RHC tools by running:

    rhc app create mydartapp https://raw.githubusercontent.com/venosyd/openshift-dart/master/metadata/manifest.yml

## Installing locally on OpenShift Origin

Visit https://github.com/kraman/openshift-dart for instructions


## Writing Dart applications

Use the pub app model, from Dart Documentation:

**bin/** -> for serverside code  
**lib/** -> for common code between server and clients  
**web/** -> for, obviously, web code  

The cartridge expects a ```appserver.dart``` in the folder ```bin``` which listens on the IP/port specified  
in ```OPENSHIFT_DART_IP``` and ```OPENSHIFT_DART_PORT``` environment variables. A sample ```appserver.dart```  
has been provided. Log files should be stored in ```OPENSHIFT_DART_LOG_DIR```.

### Dart Application Builds

Upon a git push the cartridge tries to build your application. I will execute the following:

    # download dependencies based on pubspec.yaml
    pub get && pub build
