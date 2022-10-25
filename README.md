# gradle-demo3

Gradleの練習 on Windows (PowerShell)

# init

PowerShell
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

`-Dfoo=bar`式でないから楽。

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

```
> jar tvf .\app\build\libs\app.jar  
     0 Tue Oct 25 16:15:56 JST 2022 META-INF/
    60 Tue Oct 25 16:15:56 JST 2022 META-INF/MANIFEST.MF
     0 Tue Oct 25 16:15:56 JST 2022 com/
     0 Tue Oct 25 16:15:56 JST 2022 com/example/
     0 Tue Oct 25 16:15:56 JST 2022 com/example/demo3/
   668 Tue Oct 25 16:15:56 JST 2022 com/example/demo3/App.class
```

MANIFEST.MFは
```
Manifest-Version: 1.0
Main-Class: com.example.demo3.App


```

# with dependency

md5sumを計算してみる(意味はない。テスト)。

[Maven Repository: commons-codec » commons-codec » 1.15](https://mvnrepository.com/artifact/commons-codec/commons-codec/1.15)

com.google.guavaが入ってたのをコメントアウトして...

App.javaを書き加えて。
vscodeだとクラスの補完してくれない。mavenのプロジェクトとは違う。何か拡張機能要るのか?

`build.gradle` に
```groovy
jar {
    manifest {
        attributes "Main-Class": "com.example.demo3.App"
    }
    from {
        configurations.runtimeClasspath.collect { it.isDirectory() ? it : zipTree(it) }
    }
}
```

参照: [Using the Jar Task From the Java Plugin](https://www.baeldung.com/gradle-fat-jar#using-the-jar-task-from-the-java-plugin)


とりあえず同じようにして動かす。
```powershell
.\gradlew build
java -jar .\app\build\libs\app.jar
```


# 環境

gradleは `choco install gradle` で入れた。

```
demo3> gradle -v

------------------------------------------------------------
Gradle 7.5.1
------------------------------------------------------------

Build time:   2022-08-05 21:17:56 UTC
Revision:     d1daa0cbf1a0103000b71484e1dbfe096e095918

Kotlin:       1.6.21
Groovy:       3.0.10
Ant:          Apache Ant(TM) version 1.10.11 compiled on July 10 2021
JVM:          17.0.4.1 (Eclipse Adoptium 17.0.4.1+1)
OS:           Windows 11 10.0 amd64
```

# TODO

vscodeでimportの補完してくれないのは困るなあ。IntelliJ使えってか。
[Maven and Gradle support for Java in Visual Studio Code](https://code.visualstudio.com/docs/java/java-build#_gradle) 読む。

あとでDSL=Kotlinで試す。
