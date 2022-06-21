# Hosting artifacts on Maven Central

---

* Following instructions given [here](https://www.youtube.com/watch?v=bxP9IuJbcDQ)
* Official Guide [here](https://maven.apache.org/repository/guide-central-repository-upload.html)

## Create OSS Repository Hosting(OSSRH) Account

1. Create a new [Sonatype Jira Account](https://issues.sonatype.org/secure/Signup!default.jspa)
2. Create a new [Ticket](https://issues.sonatype.org/secure/CreateIssue.jspa?pid=10134&issuetype=21)
3. If you have provided your own domain name. In order to prove its ownership you need to add a code like `OSSRH-81760` to your DNS TXT.
> You may use [MxToolBox](https://mxtoolbox.com/SuperTool.aspx) to verify if the record has been added or not.

Once all requirements are cleared your ticket will be resolved/fixed. And you shall receive such a message.

```
Congratulations! Welcome to the Central Repository!
in.umun.framework has been prepared, now user(s) umunbeing can:
Publish snapshot and release artifacts to s01.oss.sonatype.org
Have a look at this section of our official guide for deployment instructions:
https://central.sonatype.org/publish/publish-guide/#deployment
```

> Getting your account validated could take up to two business days. Though usually it takes no more than an hour or so.

4. Create a new project in eclipse
  * Create a new maven project
  * Create a simple project
  * Give it a location
  * Next
  * Fill only: Groud Id, Artifact Id, Version, Packaging, Namd and Description
  * Packaging should be `POM`

5. Create a new module 

6. Create a test class in that module

7. Follow the [requirements](https://central.sonatype.org/publish/requirements/#supply-javadoc-and-sources) to build the `pom.xml` file.

8. Follow for [deployment](https://central.sonatype.org/publish/publish-maven/#distribution-management-and-authentication)

9. Add OSSRH user name and password in your global maven settings.xml
  * On Mac OS
  * Go to your home `cd ~` 
  * Go to `~/.m2`
  * Create a new settings.xml file if not already present

```
<settings>
  <servers>
    <server>
      <id>ossrh</id>
      <username>your-jira-id</username>
      <password>your-jira-pwd</password>
    </server>
  </servers>
  
  <profiles>
    <profile>
      <id>ossrh</id>
      <activation>
        <activeByDefault>true</activeByDefault>
      </activation>
      <properties>
        <gpg.executable>gpg</gpg.executable>
        <gpg.passphrase>the_pass_phrase</gpg.passphrase>
      </properties>
    </profile>
  </profiles>
 
</settings>
```
> GPG Passphase could be any passphrase of your liking

> Replace `<gpg.executable>gpg2</gpg.executable>` with `<gpg.executable>gpg</gpg.executable>` if copying from the OG documentation.

10. Install GPG2 on your system
* Mac using [homebrew](https://formulae.brew.sh/formula/gnupg) `brew install gnupg`
* Add the following to your ~/.bash_profile
```
GPG_TTY=$(tty)
export GPG_TTY
```

11. Deploy

```
mvn clean deploy
```
