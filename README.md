# test-bundle

This is a simple bundle to test deployments in Central Portal.

Before you can use it make sure to:
  - update the groupId, package name and class path to match you namespace
  - install [gpg and create a key ring](https://central.sonatype.org/publish/requirements/gpg/#sign-files-with-gpgpgp)
  - update the key name in [pom.xml](pom.xml)


### Manual bundle deployment

Follow these steps to build the bundle and then deploy the zip artifact in https://central.sonatype.com.
```bash
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=x.y.z
mvn clean install package
```


### Central maven plugin deployment

To deploy using central-publishing-maven-plugin, update _pom.xml_ to:
  - add publishing server configuration:
```xml
<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <publishing.server.id>portal-staging</publishing.server.id>
  <tokenAuth>true</tokenAuth>
  <centralBaseUrl>https://staging.portal.central.sonatype.dev</centralBaseUrl>
  <waitForPublishCompletion>false</waitForPublishCompletion>
</properties>
```
  - disable the _distributionManagement_ section in the POM file
  - disable _nexus-staging-maven-plugin_
  - disable _checksum-maven-plugin_
  - disable _maven-assembly-plugin_
  - enable _central-publishing-maven-plugin_

then follow these steps to build and publish the bundle:
```bash
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=x.y.z
mvn clean deploy
```


### OSSRH deployment

To deploy to OSSRH using Nexus staging maven plugin follow the instructions at
https://central.sonatype.org/publish/publish-maven/#deployment. To summarize these instructions:
  - configure distribution management by adding the OSSRH repository to the POM
  - configure deployment using nexus-staging-maven-plugin
  - configure the OSSRH authentication in local _settings.xml_
  - set the property autoReleaseAfterClose to *false* you can manually inspect the staging repository
    in the Nexus Repository Manager
  - disable _maven-assembly-plugin_
  - disable _central-publishing-maven-plugin_

then follow these steps to build and publish the bundle:
```bash
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=x.y.z
mvn clean deploy
# release from the staging repository
# fails with XPP3 pull parser library not present
mvn nexus-staging:release 
# or drop the staging repository
# also fails with XPP3 pull parser library not present
mvn nexus-staging:drop
```
