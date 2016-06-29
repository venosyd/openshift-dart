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

## Dart Binaries

This cartridge includes dart binaries compiled for CentOS/RHEL 6.4. It is built using instructions found on [Issue 15506](https://code.google.com/p/dart/issues/detail?id=15506#c1)

An additional patch is added to enable pub build to use OPENSHIFT_DART_PUB_BUILD_IP environment instead of always binding on 127.0.0.1.

    Index: sdk/lib/_internal/pub/lib/src/command/build.dart
    ===================================================================
    --- sdk/lib/_internal/pub/lib/src/command/build.dart  (revision 31979)
    +++ sdk/lib/_internal/pub/lib/src/command/build.dart  (working copy)
    @@ -5,6 +5,7 @@
     library pub.command.build;

     import 'dart:async';
    +import 'dart:io';

     import 'package:barback/barback.dart';
     import 'package:path/path.dart' as path;
    @@ -63,7 +64,13 @@
           // user-facing, just use an IPv4 address to avoid a weird bug on the
           // OS X buildbots.
           // TODO(rnystrom): Allow specifying mode.
    -      return barback.createServer("127.0.0.1", 0, graph, mode,
    +      var env = Platform.environment;
    +      String barback_address = "127.0.0.1";
    +      if (env['OPENSHIFT_DART_PUB_BUILD_IP'] != null) {
    +        barback_address = env['OPENSHIFT_DART_PUB_BUILD_IP'];
    +      }
    +
    +      return barback.createServer(barback_address, 0, graph, mode,
               builtInTransformers: builtInTransformers,
               watcher: barback.WatcherType.NONE);
         }).then((server) {

## Version History

* 1.0: Initial release of cartridge.
