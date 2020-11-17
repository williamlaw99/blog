---
layout: post
title: build.grade模板
category: H_开发糖
tags: Essay
keywords: 
---

### 1. application

```
apply plugin: 'com.android.application'
apply plugin: 'org.greenrobot.greendao'
def VersionSetFile = rootProject.file("release_version.properties")
def VersionProperties = new Properties()
VersionProperties.load(new FileInputStream(VersionSetFile))
android {
    compileSdkVersion 28

    defaultConfig {
        applicationId "com.adayo.service.upgrade"
        minSdkVersion 17
        targetSdkVersion 28

        def date = new Date().format("yyMMdd", TimeZone.getTimeZone("CMT+8"))
        versionCode date.toInteger()
        versionName VersionProperties['upgrade_service_vn']+"."+date

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }

    /**
     * 签名配置
     *
     */
    signingConfigs {
        debug {
            keyAlias 'Android_17'
            keyPassword '123456'
            storeFile file('Android_17.jks')
            storePassword '123456'
        }

        release {
            keyAlias 'Android_17'
            keyPassword '123456'
            storeFile file('Android_17.jks')
            storePassword '123456'
        }
    }

    /**
     * 用于配置多种构建类型，编译系统默认定义2种构建类型：debug和release。
     *    debug构建类型不会明确的显示在默认的编译配置中，但它包含调试工具并采用debug key进行签名；
     *    release类型会采用Proguard进行代码压缩，并且默认不会进行签名；
     */

    buildTypes {

        /**
         * 默认情况下，AS使用minifyEnabled配置发布构建类型启用代码混淆，并且指定Proguard设置文件；
         */
        debug {
            //配置签名
            signingConfig signingConfigs.debug
        }

        release {
            minifyEnabled false // 为发布构建类型启用代码压缩.
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

            //配置签名
            signingConfig signingConfigs.release
        }
    }

    android.applicationVariants.all { variant ->
        variant.outputs.all {
            def createTime = new Date().format("YYYY.MM.dd", TimeZone.getTimeZone("GMT+08:00"))
            variant.getPackageApplication().outputDirectory = new File(project.rootDir.absolutePath + "/output/${variant.buildType.name}/"+createTime)
            outputFileName = "${project.name}" + ".apk"
        }
    }

    lintOptions {
        checkReleaseBuilds false
    }

    greendao{
        schemaVersion 2
    }
}

dependencies {
    implementation fileTree(dir: 'libs',include: ['*.jar'])
    implementation 'com.android.support:appcompat-v7:28.0.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
    implementation 'com.android.support.constraint:constraint-layout:1.1.3'

    implementation project (':fota_constant')
    implementation project (':fota_proxy')
    implementation project (':fota_project')
    implementation project (':fota_presenter')
    implementation project (':sx5g_fota_proxy')
    implementation project (':sx5g_fota_presenter')
    implementation project (':upgrade-constant')
    implementation project (':upgrade-manager')
    implementation project (':upgrade-midware')
    implementation project (':upgrade-proxy')
}
```

```
// Top-level build file where you can add configuration options common to all sub-projects/modules.

buildscript {
    repositories {
        jcenter()
        google()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:3.2.1'
        classpath 'org.greenrobot:greendao-gradle-plugin:3.2.2' // add plugin
        // NOTE: Do not place your application dependencies here; they belong
        // in the individual module build.gradle files
    }
}

allprojects {
    repositories {
        jcenter()
        google()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}

```





### 2.library

```
apply plugin: 'com.android.library'
def VersionSetFile = rootProject.file("release_version.properties")
def VersionProperties = new Properties()
VersionProperties.load(new FileInputStream(VersionSetFile))
android {
    compileSdkVersion 28

    defaultConfig {
        minSdkVersion 17
        targetSdkVersion 28
        def date = new Date().format("yyMMdd", TimeZone.getTimeZone("CMT+8"))
        versionCode date.toInteger()
        versionName VersionProperties['upgrade_service_vn']+"."+date
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
        }
    }

    def date = new Date().format("YYMMdd", TimeZone.getTimeZone("GMT+08:00"))
    def sdkDestinationPath = project.rootDir.absolutePath + "/output/jar/" + date
    def SDK_BASENAME = "${project.name}" + "." + "${defaultConfig.versionName}" + "." + date
    def zipFile = file('build/intermediates/intermediate-jars/release/classes.jar')

    task deleteBuild(type: Delete) {
        delete sdkDestinationPath + SDK_BASENAME + ".jar"
    }

    task makeJar(type: Jar, dependsOn: ['compileReleaseJavaWithJavac']) {
        from zipTree(zipFile)
        baseName = SDK_BASENAME
        destinationDir = file(sdkDestinationPath)
    }

    makeJar.dependsOn(deleteBuild, build)
}

dependencies {
    compileOnly fileTree(dir: 'jars', include: ['*.jar'])

    implementation 'com.android.support:appcompat-v7:28.0.0'
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'

    compileOnly project (':fota_proxy')
    compileOnly project (':fota_constant')
}

```

