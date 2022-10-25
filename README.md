# gradle-demo3

Gradleの練習

# init

```powershell
mkdir demo3 ; cd demo3
gradle init --type java-application `
 --incubating `
 --dsl groovy `
 --test-framework junit `
 --project-name demo3 `
 --package com.example.demo3
./gradlew -q run
```

# executable Jar

`build.gradle` に
```groovy
jar {
    manifest {
        attributes "Main-Class": "com.example.demo3.App"
    }
}
```

を追加して
```powershell
.\gradlew build
java -jar .\app\build\libs\app.jar
```
