# Palladio-Build-MavenParent

Using the `eclipse-parent-updatesite` base POM, you can define Maven Tycho-based Palladio builds with little effort. This parent POM covers
* compilation of java and xtend code
* building of plugins and features
* building of source plugins and source features
* test execution and coverage reports
* API documentation using javadoc
* checkstyle generation

## Requirements
* your project must have a root POM that is an aggregating POM as well
* you have to activate the extension `org.eclipse.tycho:tycho-build:2.7.5` in the `extensions.xml` in the `.mvn` folder
* you have to refer to this parent POM in your root POM

## Target Platform
You can define your target platform explicitly or by just giving repository URLs to resolve bundles. If you want to use the latter, you have to use the `eclipse-parent-updatesite-notp` POM. If you want to omit checkstyle checks and source feature generation, you should use `eclipse-parent-product`.

To define your target platform explicitly, you need to create a `.target` file that references one of the shared Palladio base target platforms. For detailed instructions on how to do this and see which target platforms are available, please follow the [Usage section of Palladio-Build-TargetPlatforms](https://github.com/PalladioSimulator/Palladio-Build-TargetPlatforms?tab=readme-ov-file#usage). To use the newest available version of units, add `0.0.0` as the version number. Starting with Tycho 4.0.10, it is also possible to omit the version entirely, which is equivalent to `0.0.0`.  
Once you have created your `.target` file, you must tell the build system where to find it. This is done by defining a property `targetPlatform.relativePath` in your root POM and setting its value to the relative path from your project's root directory to your `.target` file. For example, given the following project structure:

```
Palladio-Example-Project/
├── .mvn/
│   └── extensions.xml
├── releng/
│   ├── org.palladiosimulator.example.targetplatform/
│   │   └── tp.target             <-- Your .target file
│   └── ...
├── pom.xml                       <-- Your root POM
└── ...
```

Then you would configure the `targetPlatform.relativePath` property in your root POM as follows:

```xml
<properties>
    <targetPlatform.relativePath>releng/org.palladiosimulator.example.targetplatform/tp.target</targetPlatform.relativePath>
</properties>
```

## Update site / JavaDoc
To define an update site, you have to create a POM with packaging type `eclipse-repository` and place a `category.xml` besides it. You can edit this file using the default Eclipse editors. The resulting update site will be placed in `target/repository`. The JavaDoc files will be placed in a `javadoc` folder inside the repository.
