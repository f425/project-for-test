# This is a simplification project for test

## env
* Windows 10
* JAVA_HOME: `C:\Program Files\Java\jdk1.8.0_202`
* Extension: Pack for Java v0.27.0
* Extension: Language Support for Java(TM) by Red Hat v1.33.0
* jdk1.8.0_202 from Oracle
* jdk-17.0.12+7 from Adoptium
* vscode Version: 1.92.0
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
1. after upgrade `Extension: Language Support for Java(TM) by Red Hat` to `v1.33.0`. Got error:
```
[{
	"resource": "/c:/Users/ww/work/test/project-for-test/test2/src/main/java/com/test2/Test2.java",
	"owner": "_generated_diagnostic_collection_name_#5",
	"code": "67108964",
	"severity": 8,
	"message": "The method builder() is undefined for the type Data1",
	"source": "Java",
	"startLineNumber": 9,
	"startColumn": 15,
	"endLineNumber": 9,
	"endColumn": 22
}]
```
2. Change `Extension: Language Support for Java(TM) by Red Hat` to `v1.32.0`. It's OK.
3. Keep `Extension: Language Support for Java(TM) by Red Hat` with `v1.33.0`.
    * copy `Data1.java` to `Data2.java` and delete comments.
    * `Data2.builder().build()` is ok
