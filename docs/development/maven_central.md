# Hosting artifacts on Maven Central

---

* Following instructions given [here](https://www.youtube.com/watch?v=bxP9IuJbcDQ)
* Official Guide [here](https://maven.apache.org/repository/guide-central-repository-upload.html)
* Source Code of the [demo module library](https://github.com/iAbhinav/maven-central-demo-library)
* Source Code of [Demo Application](https://github.com/iAbhinav/maven-central-demo-app) that uses the demo module library

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


## Create a new Multi Module Project

1. Create a new project in eclipse
  * Create a new maven project
  * Create a simple project
  * Give it a location
  * Next
  * Fill only: Groud Id, Artifact Id, Version, Packaging, Namd and Description
  * Packaging should be `POM`

2. Create a new module 

3. Create a test class in that module

4. Follow the [requirements](https://central.sonatype.org/publish/requirements/#supply-javadoc-and-sources) to build the `pom.xml` file.

5. Follow for [deployment](https://central.sonatype.org/publish/publish-maven/#distribution-management-and-authentication)

6. Add OSSRH user name and password in your global maven settings.xml
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

7. Install GPG2 on your system
* Mac using [homebrew](https://formulae.brew.sh/formula/gnupg) `brew install gnupg`
* Add the following to your ~/.bash_profile
```
GPG_TTY=$(tty)
export GPG_TTY
```
* Run `gpg -k` to list the newly made key
* Find the the public-key ifront of pub `C7347E970F093E08986641785A5BD6B1028A5CEA`

```
/Users/abhinav/.gnupg/pubring.kbx
---------------------------------
pub   ed25519 2022-06-21 [SC] [expires: 2024-06-20]
      C7347E970F093E08986641785A5BD6B1028A5CEA
```
* Send the key to server
```
gpg --keyserver hkp://keyserver.ubuntu.com --send-keys C7347E970F093E08986641785A5BD6B1028A5CEA
```

8. Deploy

```
mvn clean deploy
```

9. Set version using
```
mvn versions:set -DnewVersion=0.0.2
```

> You need to update the version before deploying any new release

10. Release
```
mvn clean deploy -P release
```

You may use `-Dmaven.javadoc.skip=true` to skip javadoc while compiling.


## Testing with a new project

1. Create a new project using [Spring starter](start.spring.io)
2. Import the newly uploaded dependency to starter's `pom.xml`
```
<dependency>
  <groupId>in.umun.framework</groupId>
  <artifactId>coree</artifactId>
  <version>0.0.1</version>
</dependency>
```
3. Use this in your Application class
```
package com.umun.starter;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

import core.CoreTest;

@SpringBootApplication
public class StarterApplication {

	public static void main(String[] args) {
		SpringApplication.run(StarterApplication.class, args);
		
		CoreTest test = new CoreTest();
		test.testCore();
	}

}
```

4. Run this class as Java Application & you shall get the following output.

```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::                (v2.7.0)

2022-06-21 16:53:42.771  INFO 20008 --- [           main] com.umun.starter.StarterApplication      : Starting StarterApplication using Java 1.8.0_231 on MacBook-Pro.local with PID 20008 (/Users/abhinav/Source/poc/umun framework/starter/target/classes started by abhinav in /Users/abhinav/Source/poc/umun framework/starter)
2022-06-21 16:53:42.774  INFO 20008 --- [           main] com.umun.starter.StarterApplication      : No active profile set, falling back to 1 default profile: "default"
2022-06-21 16:53:43.399  INFO 20008 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
2022-06-21 16:53:43.445  INFO 20008 --- [           main] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 24 ms. Found 0 JPA repository interfaces.
2022-06-21 16:53:44.275  INFO 20008 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat initialized with port(s): 8080 (http)
2022-06-21 16:53:44.286  INFO 20008 --- [           main] o.apache.catalina.core.StandardService   : Starting service [Tomcat]
2022-06-21 16:53:44.286  INFO 20008 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet engine: [Apache Tomcat/9.0.63]
2022-06-21 16:53:44.399  INFO 20008 --- [           main] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2022-06-21 16:53:44.399  INFO 20008 --- [           main] w.s.c.ServletWebServerApplicationContext : Root WebApplicationContext: initialization completed in 1579 ms
2022-06-21 16:53:44.600  INFO 20008 --- [           main] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
2022-06-21 16:53:44.642  INFO 20008 --- [           main] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 5.6.9.Final
2022-06-21 16:53:44.821  INFO 20008 --- [           main] o.hibernate.annotations.common.Version   : HCANN000001: Hibernate Commons Annotations {5.1.2.Final}
2022-06-21 16:53:44.904  INFO 20008 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2022-06-21 16:53:45.258  INFO 20008 --- [           main] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2022-06-21 16:53:45.285  INFO 20008 --- [           main] org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.MySQL8Dialect
2022-06-21 16:53:45.564  INFO 20008 --- [           main] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
2022-06-21 16:53:45.573  INFO 20008 --- [           main] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2022-06-21 16:53:45.616  WARN 20008 --- [           main] JpaBaseConfiguration$JpaWebConfiguration : spring.jpa.open-in-view is enabled by default. Therefore, database queries may be performed during view rendering. Explicitly configure spring.jpa.open-in-view to disable this warning
2022-06-21 16:53:45.974  WARN 20008 --- [           main] .s.s.UserDetailsServiceAutoConfiguration : 

Using generated security password: 5d1eaa4c-0889-414d-9644-fe6a07bcdc8d

This generated password is for development use only. Your security configuration must be updated before running your application in production.

2022-06-21 16:53:46.117  INFO 20008 --- [           main] o.s.s.web.DefaultSecurityFilterChain     : Will secure any request with [org.springframework.security.web.session.DisableEncodeUrlFilter@17ec335a, org.springframework.security.web.context.request.async.WebAsyncManagerIntegrationFilter@284990de, org.springframework.security.web.context.SecurityContextPersistenceFilter@73e4387, org.springframework.security.web.header.HeaderWriterFilter@6812c8cc, org.springframework.security.web.csrf.CsrfFilter@3487442d, org.springframework.security.web.authentication.logout.LogoutFilter@656842bc, org.springframework.security.web.authentication.UsernamePasswordAuthenticationFilter@2d4a3e13, org.springframework.security.web.authentication.ui.DefaultLoginPageGeneratingFilter@686cf8ad, org.springframework.security.web.authentication.ui.DefaultLogoutPageGeneratingFilter@565d7d2f, org.springframework.security.web.authentication.www.BasicAuthenticationFilter@3a40bb52, org.springframework.security.web.savedrequest.RequestCacheAwareFilter@4137aab0, org.springframework.security.web.servletapi.SecurityContextHolderAwareRequestFilter@682e422c, org.springframework.security.web.authentication.AnonymousAuthenticationFilter@6aea99e7, org.springframework.security.web.session.SessionManagementFilter@7a66c35a, org.springframework.security.web.access.ExceptionTranslationFilter@1b8aaeab, org.springframework.security.web.access.intercept.FilterSecurityInterceptor@703e8050]
2022-06-21 16:53:46.178  INFO 20008 --- [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port(s): 8080 (http) with context path ''
2022-06-21 16:53:46.189  INFO 20008 --- [           main] com.umun.starter.StarterApplication      : Started StarterApplication in 3.97 seconds (JVM running for 4.342)
Core is successfully imported from maven central.
```

> Core is successfully imported from maven central.
