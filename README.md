# test-bundle

This is a simple bundle to test deployments in Central Portal.

Before you can use it make sure to:
  - update the groupId and paths to match you namespace
  - install [gpg and create a key ring](https://central.sonatype.org/publish/requirements/gpg/#sign-files-with-gpgpgp)
  - update the key name in [pom.xml](pom.xml)

Follow these steps to build the bundle.
```bash
mvn versions:set -DgenerateBackupPoms=false -DnewVersion=x.y.z
mvn clean install package
```

Deploy the zip artifact in Portal. 
