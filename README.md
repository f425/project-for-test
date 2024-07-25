# This is a simplification project for test

## env
* Windows 10
* JAVA_HOME: `C:\Program Files\Java\jdk1.8.0_202`
* Extension Pack for Java v0.27.0
* jdk1.8.0_202 from Oracle
* jdk-17.0.12+7 from Adoptium
* vscode Version: 1.91.1
* vscode config `setting.json` about java and gradle:
    1. `C:\\Program Files\\Java\\jdk1.8.0_202` (Oracle jdk8 for code project)
    2. `C:\\Program Files\\Eclipse Adoptium\\jdk-17.0.12+7` (for vscode language server)
    ```json
        {
            "java.jdt.ls.java.home": "C:\\Program Files\\Eclipse Adoptium\\jdk-17.0.12+7",
            "java.import.gradle.java.home": "C:\\Program Files\\Java\\jdk1.8.0_202",
            "java.configuration.runtimes": [
                {
                    "name": "JavaSE-1.8",
                    "path": "C:\\Program Files\\Java\\jdk1.8.0_202",
                    "default": true
                }
            ]
        }
    ```

## problem
1. library `TestProcessor` used to customize the processor (with `JavacProcessingEnvironment` what maybe only used with java8)
2. project `test1`, depends on this `TestProcessor` for compilation
3. project `test2`, which depends on `test1`
4. the `test2` is ERROR with red color
5. the `PROBLEMS` show the prompt.
```
[{
	"resource": "/C:/Users/ww/Desktop/test/test/test2/",
	"owner": "_generated_diagnostic_collection_name_#3",
	"code": "0",
	"severity": 8,
	"message": "The project cannot be built until its prerequisite test1 is built. Cleaning and building all projects is recommended",
	"source": "Java",
	"startLineNumber": 1,
	"startColumn": 1,
	"endLineNumber": 1,
	"endColumn": 1
}]
```
6. open the java icon in the status bar and then click `Open Log`: (file in `log` floder)
```
java.lang.IllegalAccessError: class com.ttt.TestProcessor (in unnamed module @0x303bfd10) cannot access class com.sun.tools.javac.processing.JavacProcessingEnvironment (in module jdk.compiler) because module jdk.compiler does not export com.sun.tools.javac.processing to unnamed module @0x303bfd10
	at com.ttt.TestProcessor.init(TestProcessor.java:23)
	at org.eclipse.jdt.internal.apt.pluggable.core.dispatch.
```
8. debug Test1.main show: `Error: Cannot find or load main class com.test1.`
9. debug Test2.main show:
```
Exception in thread "main" java.lang.NoClassDefFoundError: com/test1/Test1
        at com.test2.Test2.main(Test2.java:7)
Caused by: java.lang.ClassNotFoundException: com.test1.Test1
        at java.net.URLClassLoader.findClass(URLClassLoader.java:382)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:424)
        at sun.misc.Launcher$AppClassLoader.loadClass(Launcher.java:349)
        at java.lang.ClassLoader.loadClass(ClassLoader.java:357)
        ... 1 more
```
7. so is there something wrong about `Extension Pack for Java` or other config i can hava a try?

### some test
1. publish projcet in `TERMINAL` powershell with `./gradlew processor:publishToMavenLocal` (publish the project `TestProcessor` first before compile other project)
2. run `./gradlew jar` in `TERMINAL` with `JAVA_HOME`, got `BUILD SUCESS`
3. set jdk path in gradle.properties with jdk8, run `./gradlew jar`, got `BUILD SUCESS`
4. set jdk path in gradle.properties with jdk17, run `./gradlew jar`, got ERROR most same to ERROR in `OpenLog`
```
> Task :test1:compileJava FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':test1:compileJava'.
> java.lang.IllegalAccessError: class com.ttt.TestProcessor (in unnamed module @0xf263622) cannot access class com.sun.tools.javac.processing.JavacProcessingEnvironment (in module jdk.compiler) because module jdk.compiler does not export com.sun.tools.javac.processing to unnamed module @0xf263622
```

### abnout idea
1. Other peopel on my team use `idea`. they got nothing.
2. I also open this project with idea. it can run success. Also debug with Test1.main and Test2.main
3. the idea `Gradle JVM` with JAVA_HOME: jdk8 (C:\Program Files\Java\jdk1.8.0_202)