## ServiceComb

### Release Guide

### Service-Center

Step 1: Clone the code

```
git clone https://github.com/apache/incubator-servicecomb-service-center.git $GOPATH/src/github.com/apache/incubator-servicecomb-service-center
cd $GOPATH/src/github.com/apache/incubator-servicecomb-service-center
```

Step 2: Download all the dependencies

```
go get github.com/FiloSottile/gvt
gvt restore
```

Step 3: Make the release

```
bash -x scripts/release/make_release.sh linux 1.0.0 1.0.0-m1
bash -x scripts/release/make_release.sh windows 1.0.0 1.0.0-m1
```

Step 4: Make a Tag
```
git tag -a 1.0.0-m1 -m "Service-Center 1.0.0-m1 Release"
```

Step 5: Push the Tag
```
git push origin 1.0.0-m1
```

Step 6: Test all the release

Step 7: Download the source code from git tag and rename it to apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip  
  
Step 8: Sign the release  
```
gpg2 -ab apache-incubator-servicecomb-service-center-1.0.0-m1-linux-amd64.tar.gz
gpg2 -ab apache-incubator-servicecomb-service-center-1.0.0-m1-windows-amd64.tar.gz
gpg2 -ab apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip

gpg2 --print-md SHA1 apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz > apache-servicecomb-incubating-service-center-1.0.0-m1-linux-amd64.tar.gz.sha1
gpg2 --print-md SHA1 apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz > apache-servicecomb-incubating-service-center-1.0.0-m1-windows-amd64.tar.gz.sha1
gpg2 --print-md SHA1 apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip > apache-servicecomb-incubating-service-center-1.0.0-m1-src.zip.sha1
```
Step 9: Push it to Apache SVN.

### Java-Chassis

Step 1: Clone the code
```
git clone https://github.com/apache/incubator-servicecomb-java-chassis
cd incubator-servicecomb-java-chassis
```

Step 2: Cut the release

```
perl -pi -e 's/1.0.0-m1-SNAPSHOT/1.0.0-m1/g' $(find . | grep *.xml)
```

Step 3: Create a tag and push the tag to origin

```
git tag -a 1.0.0-m1 -m "Java-Chassis 1.0.0-m1 Release"

git push origin 1.0.0-m1
```

Step 4: Configure the .travis.settings.xml file to change the values of apache username/password , keypath and passphrase.
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>apache.snapshots.https</id>
            <username>asifdxtreme</username>
            <password>*****************************************</password>
        </server>
        <server>
            <id>apache.releases.https</id>
            <username>asifdxtreme</username>
            <password>**********************************</password>
        </server>
    </servers>
    <profiles>
        <profile>
          <id>passphrase</id> <!-- give it the name of your project -->
          <properties>
            <gpg.homedir>/home/travis/build/apache/incubator-servicecomb-java-chassis</gpg.homedir>
            <gpg.keyname>2DE9D2F9</gpg.keyname>
            <gpg.passphrase>*******************************************</gpg.passphrase>
          </properties>
        </profile>
    </profiles>          
</settings>
```

Step 5: Run the below command to start deploying it to nexus repo
```
mvn deploy -DskipTests -Prelease -Pdistribution -Ppassphrase --settings .travis.settings.xml
```

Step 6: Once deployed successfully then go to nexus repo to close the staging repo https://repository.apache.org 

Step 7: Download the java-chassis-distribution from staging repo and upload it to Apache SVN. 

### Saga

Step 1: Clone the code
```
git clone https://github.com/apache/incubator-servicecomb-saga
cd incubator-servicecomb-saga
```

Step 2: Cut the release

```
perl -pi -e 's/0.1.0-SNAPSHOT/0.1.0/g' $(find . | grep *.xml)
```

Step 3: Create a tag and push the tag to origin

```
git tag -a 0.1.0 -m "Saga 0.1.0 Release"

git push origin 0.1.0
```

Step 4: Configure the .travis.settings.xml file to change the values of apache username/password , keypath and passphrase.
```
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 http://maven.apache.org/xsd/settings-1.0.0.xsd">
    <servers>
        <server>
            <id>apache.snapshots.https</id>
            <username>asifdxtreme</username>
            <password>*****************************************</password>
        </server>
        <server>
            <id>apache.releases.https</id>
            <username>asifdxtreme</username>
            <password>**********************************</password>
        </server>
    </servers>
    <profiles>
        <profile>
          <id>passphrase</id> <!-- give it the name of your project -->
          <properties>
            <gpg.homedir>/home/travis/build/apache/incubator-servicecomb-java-chassis</gpg.homedir>
            <gpg.keyname>2DE9D2F9</gpg.keyname>
            <gpg.passphrase>*******************************************</gpg.passphrase>
          </properties>
        </profile>
    </profiles>          
</settings>
```

Step 5: Run the below command to start deploying it to nexus repo
```
mvn deploy -DskipTests --settings .travis.settings.xml -Prelease -Ppassphrase
```

Step 6: Once deployed successfully then go to nexus repo to close the staging repo https://repository.apache.org 

Step 7: Download the saga-distribution from staging repo and upload it to Apache SVN. 