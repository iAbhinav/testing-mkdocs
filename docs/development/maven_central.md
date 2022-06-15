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
