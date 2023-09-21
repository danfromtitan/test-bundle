# test-bundle

This is a simple bundle to test deployments in Central Portal.

Before you can use it make sure to:
  - update the groupId, package name and class path to match you namespace
  - install [gpg and create a key ring](https://central.sonatype.org/publish/requirements/gpg/#sign-files-with-gpgpgp)
  - update the key name in [pom.xml](pom.xml)

Follow these steps to build the bundle and then deploy the zip artifact in Portal's UI.
```bash
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=x.y.z
mvn clean install package
```


Alternativelly, to deploy using central-publishing-maven-plugin, update _pom.xml_ to:
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
  - disable _checksum-maven-plugin_
  - disable _maven-assembly-plugin_
  - enable _central-publishing-maven-plugin_

then follow these steps to build the bundle:
```bash
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=x.y.z
mvn clean deploy
```