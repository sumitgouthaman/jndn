jNDN:  A Named Data Networking client library for Java
======================================================

Prerequisites
=============

* Required: Java JDK version >= 1.7
* Required: Apache Maven (for building the jar file and running unit tests)
* Optional: Android SDK (for Android examples)
* Optional: sqlite-jdbc (for key storage). This is installed by Maven.
* Optional: Protobuf (for the ProtobufTlv converter and ChronoSync). This is
  installed by Maven.

Following are the detailed steps for each platform to install the prerequisites.

## OS X 10.8.4
Install Xcode.
In Xcode Preferences > Downloads, install "Command Line Tools".

To install Apache Maven, install MacPorts from http://www.macports.org/install.php .
In a new terminal, enter:

    sudo port install maven3

Optional: To install Android SDK, install from https://developer.android.com/sdk/index.html .

## OS X 10.9.5, OS X 10.10.2
Install Xcode.  (Xcode seems to already have the Command Line Tools.)

To install Apache Maven, install MacPorts from http://www.macports.org/install.php .
In a new terminal, enter:

    sudo port install maven3
    sudo port select --set maven maven3

Optional: To install Android SDK, install from https://developer.android.com/sdk/index.html .

## Ubuntu 12.04 (64 bit and 32 bit), Ubuntu 14.04 (64 bit and 32 bit)
To install Apache Maven, in a terminal enter:

    sudo apt-get install maven

## Windows
jNDN is tested on Windows 7 64-bit.

To install Apache Maven, follow the instructions at http://maven.apache.org/download.cgi .
Also see the Windows install hints at
http://maven.apache.org/guides/getting-started/windows-prerequisites.html .

Build
=====

To build in a terminal, change directory to the jNDN root.  Enter:

    mvn package

This builds the default profile "with-protobuf" which puts jndn-<version>.jar in the 'target'
folder, and the javadoc files in 'target/apidocs'. The jar file includes ProtobufTlv and
other classes so that your deployed application needs the Google Protobuf library.
To build without this dependency, enter:

    mvn package -P without-protobuf

Building 'package' automatically runs the unit tests in the folder 'tests'. To also
run the integration tests (you must be running NFD), in a terminal enter:

    mvn verify -P with-integration-tests

To run the examples, you must first install the jar file. In the jNDN root directory, enter:

    mvn install

To run an example such as TestEncodeDecodeFibEntry (see the list below), make sure the jar file
is installed (see above). Change to the 'examples' directory. In a terminal enter:

    mvn -q test -DclassName=TestEncodeDecodeFibEntry

To run the Android samples, install the Android SDK as shown above. The samples
were tested by installing the following in the Android SDK Manager:
"Android SDK Build tools rev 21.1", all support for Android 5.0 (API 21),
"Extras / Android Support Repository" and "Extras / Android Support Library".
Also, in the Android Virtual Device Manager create an Android 5.0 device. Also,
import adt-bundle/sdk/extras/android/support/v7/appcompat into the workspace.

* samples/android/NDNPing: Ping an NDN testbed server and show the ping time.  To build, in Eclipse you need to right-click on src/net and fix the link to the jndn/src/net folder.

Files
=====
This makes the following library:

* target/jndn-<version>.jar: The jNDN library.

When you run `mvn test` in the examples directory, it compiles the following java classes
from examples/src/net/named_data/jndn/tests and puts the class files in
examples/target/classes/net/named_data/jndn/tests:

* TestGetAsync: Connect to one of the NDN testbed hubs, express an interest and display the received data.
* TestPublishAsyncNfd: Connect to the local NFD hub, accept interests with prefix /testecho and echo back a data packet. See test-echo-consumer.
* TestEchoConsumer: Prompt for a word, send the interest /testecho/word to the local hub which is echoed by test-publish-async-nfd (or test-publish-async-ndnx).
* TestEncodeDecodeInterest: Encode and decode an interest, testing interest selectors and the name URI.
* TestEncodeDecodeData: Encode and decode a data packet, including signing the data packet.
* TestEncodeDecodeFibEntry: Encode and decode a sample Protobuf message using ProtobufTlv.
* TestChronoChat: A command-line chat application using the ChronoSync2013 API, compatible with ChronoChat-js.

Running `mvn package` puts code documentation in target/apidocs.
