# Palladio-Build-MavenParent

Using the `eclipse-parent-updatesite` base POM, you can define Maven Tycho-based Palladio builds with little effort. This parent POM covers:
* Compilation of Java and Xtend code
* Building of plugins and features
* Building of source plugins and source features
* Test execution and coverage reports
* API documentation using JavaDoc
* Checkstyle generation

## Requirements
* You have to activate the extension `org.eclipse.tycho:tycho-build:2.7.5` in the `.mvn/extensions.xml` file by adding:

  ```xml
  <extensions>
      <extension>
          <groupId>org.eclipse.tycho</groupId>
          <artifactId>tycho-build</artifactId>
          <version>2.7.5</version>
      </extension>
  </extensions>
  ```

* Your project must have a root POM that serves as an aggregating POM for all your project modules (e.g., releng, bundles, features, tests). This root POM must refer to the `eclipse-parent-updatesite` as its parent POM by adding:  

  ```xml
  <parent>
      <groupId>org.palladiosimulator</groupId>
      <artifactId>eclipse-parent-updatesite</artifactId>
      <version>0.10.0</version>
  </parent>
  ```

* If you need a module-specific POM (e.g., for a bundle), it must refer to your project's root POM as its parent.

## Target Platform
You need to define your target platform explicitly by creating a `.target` file (the recommended name is `tp.target`) that references one of the shared Palladio base target platforms. For detailed instructions on how to do this and see which target platforms are available, please follow the [Usage section of Palladio-Build-TargetPlatforms](https://github.com/PalladioSimulator/Palladio-Build-TargetPlatforms?tab=readme-ov-file#usage). To use the newest available version of units, add `0.0.0` as the version number. Starting with Tycho 4.0.10, it is also possible to omit the version entirely, which is equivalent to `0.0.0`.  
Once you have created your `.target` file, you must tell the build system where to find it. This is done by defining a property `targetPlatform.relativePath` in your root POM and setting its value to the relative path from your project's root directory to your `.target` file. For example, given the following project structure:

```
Palladio-Example-Project/
├── pom.xml                       <-- Your root POM
├── releng/                       (contains helper projects and the update site project)
│   ├── org.palladiosimulator.example.targetplatform/
│   │   └── tp.target             <-- Your .target file
│   ├── org.palladiosimulator.example.updatesite/
│   │   └── category.xml
│   └── ...
├── bundles/                      (contains all plugin projects)
├── features/                     (contains all feature projects)
├── tests/                        (contains all test projects)
├── .mvn/
│   └── extensions.xml
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
