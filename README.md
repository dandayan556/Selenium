# Selenium
Selenium
Python workflow Ruby workflow JavaScript workflow Java workflow

Selenium

Selenium is an umbrella project encapsulating a variety of tools and libraries enabling web browser automation. Selenium specifically provides an infrastructure for the W3C WebDriver specification — a platform and language-neutral coding interface compatible with all major web browsers.

The project is made possible by volunteer contributors who've generously donated thousands of hours in code development and upkeep.

Selenium's source code is made available under the Apache 2.0 license.

Documentation
Narrative documentation:

User Manual
API documentation:

C#
JavaScript
Java
Python
Ruby
Pull Requests
Please read CONTRIBUTING.md before submitting your pull requests.

Requirements
Bazelisk, a Bazel wrapper that automatically downloads the version of Bazel specified in .bazelversion file and transparently passes through all command-line arguments to the real Bazel binary.
The latest version of the Java 11 OpenJDK
java and jar on the PATH (make sure you use java executable from JDK but not JRE).
To test this, try running the command javac. This command won't exist if you only have the JRE installed. If you're met with a list of command-line options, you're referencing the JDK properly.
Python 3.7+
python on the PATH
The tox automation project for Python: pip install tox
MacOS users should have the latest version of Xcode installed, including the command-line tools. The following command should work:
xcode-select --install
Users of Apple Silicon Macs should add build --host_platform=//:rosetta to their .bazelrc.local file. We are working to make sure this isn't required in the long run.

Windows users should have the latest version of Visual Studio command line tools and build tools installed

BAZEL_VS environment variable should point to the location of the build tools, e.g. C:\Program Files (x86)\Microsoft Visual Studio\2019\BuildTools
BAZEL_VC environment variable should point to the location of the command line tools, e.g. C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\VC
BAZEL_VC_FULL_VERSION environment variable should contain the version of the installed command line tools, e.g. 14.27.29110
Optional Requirements
Ruby 2.0
Internet Explorer Driver
If you plan to compile the IE driver, you also need:

Visual Studio 2008
32 and 64-bit cross compilers
The build will work on any platform, but the tests for IE will be skipped silently if you are not building on Windows.

Building
Bazel
Bazel was built by the fine folks at Google. Bazel manages dependency downloads, generates the Selenium binaries, executes tests, and does it all rather quickly.

More detailed instructions for getting Bazel running are below, but if you can successfully get the java and javascript folders to build without errors, you should be confident that you have the correct binaries on your system.

Before Building
Ensure that you have Firefox installed and the latest geckodriver on your $PATH. You may have to update this from time to time.

Common Build Targets
Java
Click to see Java Build Steps
JavaScript
Click to see JavaScript Build Steps
Python
Click to see Python Build Steps
Ruby
Click to see Ruby Build Steps
.NET
Click to see .NET Build Steps
Build Details
Bazel files are called BUILD.bazel
crazyfun build files are called build.desc. This is an older build system, still in use in the project for Ruby bindings mostly.
The order the modules are built is determined by the build system. If you want to build an individual module (assuming all dependent modules have previously been built), try the following:

bazel test javascript/atoms:test
In this case, javascript/atoms is the module directory, test is a target in that directory's BUILD.bazel file.

As you see build targets scroll past in the log, you may want to run them individually.

Common Tasks (Bazel)
To build the bulk of the Selenium binaries from source, run the following command from the root folder:

bazel build java/... javascript/...
To build the grid deployment jar, run this command:

bazel build grid
To run tests within a particular area of the project, use the "test" command, followed by the folder or target. Tests are tagged with "small", "medium", or "large", and can be filtered with the --test_size_filters option:

bazel test --test_size_filters=small,medium java/...
Bazel's "test" command will run all tests in the package, including integration tests. Expect the test java/... to launch browsers and consume a considerable amount of time and resources.

To bump the versions of the pinned browsers to their latest stable versions:

bazel run scripts:pinned_browsers > temp.bzl && mv temp.bzl common/repositories.bzl
Editing Code
Most of the team use either Intellij IDEA or VS.Code for their day-to-day editing. If you're working in IntelliJ, then we highly recommend installing the Bazel IJ plugin which is documented on its own site.

If you do use IntelliJ and the Bazel plugin, there is a project view checked into the tree in scripts/ij.bazelproject which will make it easier to get up running, and editing code :)

Tour
The codebase is generally segmented around the languages used to write the component. Selenium makes extensive use of JavaScript, so let's start there. Working on the JavaScript is easy. First of all, start the development server:

bazel run debug-server
Now, navigate to http://localhost:2310/javascript. You'll find the contents of the javascript/ directory being shown. We use the Closure Library for developing much of the JavaScript, so now navigate to http://localhost:2310/javascript/atoms/test.

The tests in this directory are normal HTML files with names ending with _test.html. Click on one to load the page and run the test.

Maven POM files
Here is the public Selenium Maven repository.

Build Output
bazel makes a top-level group of directories with the bazel- prefix on each directory.

Help with go
More general, but basic, help for go…

./go --help
go is just a wrapper around Rake, so you can use the standard commands such as rake -T to get more information about available targets.

Maven per se
If it is not clear already, Selenium is not built with Maven. It is built with bazel, though that is invoked with go as outlined above, so you do not have to learn too much about that.

That said, it is possible to relatively quickly build Selenium pieces for Maven to use. You are only really going to want to do this when you are testing the cutting-edge of Selenium development (which we welcome) against your application. Here is the quickest way to build and deploy into your local maven repository (~/.m2/repository), while skipping Selenium's own tests.

./go maven-install
The maven jars should now be in your local ~/.m2/repository.

Useful Resources
Refer to the Build Instructions wiki page for the last word on building the bits and pieces of Selenium.

Running Browser Tests on Linux
In order to run Browser tests, you first need to install the browser-specific drivers, such as geckodriver, chromedriver, or edgedriver. These need to be on your PATH.

By default, Bazel runs these tests in your current X-server UI. If you prefer, you can alternatively run them in a virtual or nested X-server.

Run the X server Xvfb :99 or Xnest :99
Run a window manager, for example, DISPLAY=:99 jwm
Run the tests you are interested in:
bazel test --test_env=DISPLAY=:99 //java/... --test_tag_filters=chrome
An easy way to run tests in a virtual X-server is to use Bazel's --run_under functionality:

bazel test --run_under="xvfb-run -a" //java/... --test_tag_filters=chrome
Bazel Installation/Troubleshooting
Selenium Build Docker Image
If you're finding it hard to set up a development environment using bazel and you have access to Docker, then you can build a Docker image suitable for building and testing Selenium in from the Dockerfile in the dev image directory.

MacOS
bazelisk
Bazelisk is a Mac-friendly launcher for Bazel. To install, follow these steps:

brew tap bazelbuild/tap && \
brew uninstall bazel; \
brew install bazelbuild/tap/bazelisk
Xcode
If you're getting errors that mention Xcode, you'll need to install the command-line tools.

Bazel for Mac requires some additional steps to configure properly. First things first: use the Bazelisk project (courtesy of philwo), a pure golang implementation of Bazel. In order to install Bazelisk, first verify that your Xcode will cooperate: execute the following command:

xcode-select -p

If the value is /Applications/Xcode.app/Contents/Developer/, you can proceed with bazelisk installation. If, however, the return value is /Library/Developer/CommandLineTools/, you'll need to redirect the Xcode system to the correct value.

sudo xcode-select -s /Applications/Xcode.app/Contents/Developer/
sudo xcodebuild -license
The first command will prompt you for a password. The second step requires you to read a new Xcode license, and then accept it by typing "agree".

(Thanks to this thread for these steps)

Releasing
Begin by tagging the revision you're about to release, and push that tag to GitHub.

Before running a release build, you must ensure that the --stamp flag is used by the build. The easiest way to do this is:

echo build --stamp >>.bazelrc.local
GitHub Release Page
Draft a new (perhaps pre-release)
Make sure this release is for the tag you created earlier
Set the title to be whatever the release is.
Use git log $PREV_RELEASE..$NEW_TAG --format=format:'* [%h](https://github.com/seleniumhq/selenium/commit/%H) - %s :: %an' | pbcopy to generate the list of changes. Make sure you've set $PREV_RELEASE and $NEW_TAG!
The release notes are:
### Changelog

For each component's detailed changelog, please check:
* [Ruby](https://github.com/SeleniumHQ/selenium/blob/trunk/rb/CHANGES)
* [Python](https://github.com/SeleniumHQ/selenium/blob/trunk/py/CHANGES)
* [JavaScript](https://github.com/SeleniumHQ/selenium/blob/trunk/javascript/node/selenium-webdriver/CHANGES.md)
* [Java](https://github.com/SeleniumHQ/selenium/blob/trunk/java/CHANGELOG)
* [DotNet](https://github.com/SeleniumHQ/selenium/blob/trunk/dotnet/CHANGELOG)
* [IEDriverServer](https://github.com/SeleniumHQ/selenium/blob/trunk/cpp/iedriverserver/CHANGELOG)

### Commits in this release
<details>
<summary>Click to see all the commits included in this release</summary>

   INSERT LIST OF CHANGES HERE!

</details>
Now publish the release.
Java
To release the Java components, make sure you have permission to push to the OSS Sonatype repo. You will need these credentials when pushing the maven release.

Make sure that the java CHANGELOG is up to date, then just run:

./go release-java
This will do two things:

Build the publishable artifacts and push them to a staging repo on the OSS Sonatype server.
Create zip files to upload in build/dist
You will need to manually release the maven artifacts, and also upload the artifacts from build/dist to the GitHub release.
