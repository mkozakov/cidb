# Building instructions #

This project uses [Apache Maven](http://maven.apache.org/) for the whole lifecycle management. From the project's description:

> Apache Maven is a software project management and comprehension tool.
> Based on the concept of a project object model (POM), Maven can manage
> a project's build, reporting and documentation from a central piece of information.

In short, Maven handles everything related to building the project: downloading the required dependencies, compiling the Java files, running tests, building the jars or final zips, and even more advanced goals such as performing style checks, creating releases and deploying them to a remote repository. All these steps are configured declaratively with as little custom settings as possible, since the philosophy of maven is **"convetion over configuration"**, relying on well-defined best practices and defaults, while allowing custom variations where needed.

Building the entire project is as simple as `mvn install`, but first the environment must be set-up:

* Make sure a proper JDK is installed, Oracle Java SE 1.6 or 1.7 is recommended. Just a JRE isn't enough, since the project requires compilation.
* Install maven by [downloading it](http://maven.apache.org/download.html) and following the [installation instructions](http://maven.apache.org/download.html#Installation).
* Create a `~/.m2/settings.xml` file with the following custom remote repository defined. If you are on Windows, create the directory `.m2` in your home directory, e.g. `C:\Documents and Settings\Administrator\.m2`. If you cannot do this from Windows Explorer, open a command line and use `md .m2`. Place the following content in the `settings.xml` file:

        <settings>
         <profiles>
           <profile>
             <id>xwiki</id>
             <repositories>
               <repository>
                 <id>xwiki-snapshots</id>
                 <name>XWiki Nexus Snapshot Repository Proxy</name>
                 <url>http://nexus.xwiki.org/nexus/content/groups/public-snapshots</url>
                 <releases>
                   <enabled>false</enabled>
                 </releases>
                 <snapshots>
                   <enabled>true</enabled>
                 </snapshots>
               </repository>
               <repository>
                 <id>xwiki-releases</id>
                 <name>XWiki Nexus Releases Repository Proxy</name>
                 <url>http://nexus.xwiki.org/nexus/content/groups/public</url>
                 <releases>
                   <enabled>true</enabled>
                 </releases>
                 <snapshots>
                   <enabled>false</enabled>
                 </snapshots>
               </repository>
             </repositories>
             <pluginRepositories>
               <pluginRepository>
                 <id>xwiki-plugins-snapshots</id>
                 <name>XWiki Nexus Plugin Snapshot Repository Proxy</name>
                 <url>http://nexus.xwiki.org/nexus/content/groups/public-snapshots</url>
                 <releases>
                   <enabled>false</enabled>
                 </releases>
                 <snapshots>
                   <enabled>true</enabled>
                 </snapshots>
               </pluginRepository>
               <pluginRepository>
                 <id>xwiki-plugins-releases</id>
                 <name>XWiki Nexus Plugin Releases Repository Proxy</name>
                 <url>http://nexus.xwiki.org/nexus/content/groups/public</url>
                 <releases>
                   <enabled>true</enabled>
                 </releases>
                 <snapshots>
                   <enabled>false</enabled>
                 </snapshots>
               </pluginRepository>
             </pluginRepositories>
           </profile>
         </profiles>
         <activeProfiles>
           <activeProfile>xwiki</activeProfile>
         </activeProfiles>
        </settings>

* Clone the sources of the project locally, using one of:
    * `git clone git://github.com/compbio-UofT/cidb.git` if you need a read-only clone
    * `git clone git@github.com:compbio-UofT-/cidb.git` if you also want to commit changes back to the project (and you have the right to do so)
    * download an [archive of the current code](https://github.com/compbio-UofT/cidb/downloads) if you don't want to use version control at all
* In a console (command line) go to the directory where the sources are located, and execute `mvn install` to build the project
    * the first time that the project is built is going to take a while longer, since all the required dependencies are downloaded, but all the subsequent builds should only take seconds

# Installation #

The project is split into several modules, among which `wiki-distribution` will result in a fully-working self-contained package ready to run. Running the application is as simple as:

* `mvn install`, as stated above, to get the project built
* go to the directory where the final package is located, `wiki-distribution/target`
* extract the contents of the `distribution-wiki-1.0-SNAPSHOT.zip` archive to a location of your choice (don't leave it in the `target` directory, since it will be overwriten by subsequent builds)
* launch the `start` script
* open [http://localhost:8080/](http://localhost:8080/) in a browser

That's it!

The standalone distribution comes packaged with a *Java Servlet Container*, [Jetty](http://www.eclipse.org/jetty/), a lightweight *relational database management system*, [HyperSQL](http://hsqldb.org/), and the *phenotyping application* itself.

**While good for a small to medium trial, this package isn't suited for production use and large quantities of data**.