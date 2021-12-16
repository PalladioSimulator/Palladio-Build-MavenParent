# Palladio-Build-MavenParent

Using the `eclipse-parent-updatesite` base POM, you can define Maven Tycho based Palladio builds with little effort. The parent POM covers
* compilation of java and xtend code
* building of plugins and features
* building of source plugins and source features
* test execution and coverage reports
* API documentation using javadoc
* checkstyle generation
* updating, filtering, and merging of target platform definitions

## Requirements
* your project has to have a root POM that is an aggregating POM as well
* you have to activate the following extensions in the `extensions.xml` in the `.mvn` folder
  * `org.eclipse.tycho.extras:tycho-pomless:2.5.0`
  * `org.palladiosimulator:tycho-tp-refresh-maven-plugin:0.2.5`
* you have to refer to the parent POM in your root POM

## Target Platform
You can define your target platform explicitly or by just giving repository URLs to resolve bundles. If you want to use the latter, you have to use the `eclipse-parent-updatesite-notp` POM. If you want to omit checkstyle checks and source feature generation, you should use `eclipse-parent-product`.

To define your target platform explicitly, you have to create target platform definitions. The parent POM will take care of merging multiple target definitions. You can refer to target files by defining a property `org.palladiosimulator.maven.tychotprefresh.tplocation.n` with the path to the file. You can use project coordinates (`groupdId:artifactId:version:classifier:fileExtension`) as well if your target definition resides inside a maven artifact. Please note that you have to use different numbers for `n` starting with 2.

You can use additional attributes in the target definition file to filter entries or update version numbers. Please note that you cannot use the Eclipse editor anymore to edit files containing additional attributes.

To select only a subset of your target definition during a build, you have to add the attribute `filter` to a `location` element and set the value to any name you like. Afterwards, you have to add a property `org.palladiosimulator.maven.tychotprefresh.filter.n` with the name you chose to your root POM. Use different numbers `n` for different filters starting with 2. The parent POM already defines the filters `nightly` and `release` that are activated by activating the `nightly` or `release` profile. The former is activated if the latter is not activated.

To update versions of units mentioned in the target platform, you have to add the attribute `refresh` and set it to `true`. Before the build takes place, the latest versions available in the given location will be used.

You can always have a look at the generated target platform definition in the `target` folder of your root project.

## Update site / JavaDoc
To define an update site, you have to create a POM with packaging type `eclipse-repository` and place a `category.xml` besides it. You can edit this file using the default Eclipse editors. The resulting update site will be placed in `target/repository`. The JavaDoc files will be placed in a `javadoc` folder inside the repository.
