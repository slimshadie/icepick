Icepick
============

[![Build Status](https://secure.travis-ci.org/frankiesardo/icepick.png)](http://travis-ci.org/frankiesardo/icepick)

Icepick is an Android library that eliminates the boilerplate of saving and restoring instance state.
It uses annotation processing to generate code that does bundle manipulation and key generation, so that you don't have to write it yourself.

```java
class ExampleActivity extends Activity {
  @Icicle String username; // This will be automatically saved and restored

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Icepick.restoreInstanceState(this, savedInstanceState);
  }

  @Override public void onSaveInstanceState(Bundle outState) {
    super.onSaveInstanceState(outState);
    Icepick.saveInstanceState(this, outState);
  }

  // You can put the calls to Icepick into a BaseActivity
  // All Activities extending BaseActivity automatically have state saved/restored
}
```

Icepick can also generate the instance state code for your Custom Views:

```java
class CustomView extends View {
  @Icicle int selectedPosition; // This will be automatically saved and restored

  @Override public Parcelable onSaveInstanceState() {
    return Icepick.saveInstanceState(this, super.onSaveInstanceState());
  }

  @Override public void onRestoreInstanceState(Parcelable state) {
    super.onRestoreInstanceState(Icepick.restoreInstanceState(this, state));
  }

  // You can put the calls to Bundles into a BaseCustomView and inherit from it
  // All Views extending this CustomView automatically have state saved/restored
}
```


Proguard
--------

If Proguard is enabled make sure you add these rules to your configuration:

```
-dontwarn icepick.**
-keep class **$$Icicle { *; }
-keepnames class * { @icepick.Icicle *;}
```

Download
--------

Icepick needs two libraries: `icepick` and `icepick-processor`.

[![Clojars Project](http://clojars.org/frankiesardo/icepick/latest-version.svg)](http://clojars.org/frankiesardo/icepick)

[![Clojars Project](http://clojars.org/frankiesardo/icepick-processor/latest-version.svg)](http://clojars.org/frankiesardo/icepick-processor)

Gradle:

```groovy
repositories {
  maven {url "https://clojars.org/repo/"}
}
dependencies {
  compile 'frankiesardo:icepick:{{latest-version}}'
  provided 'frankiesardo:icepick-processor:{{latest-version}}'
}
```

Maven:

```xml
<repositories>
  <repository>
    <id>clojars</id>
    <url>https://clojars.org/repo/</url>
    <snapshots>
      <enabled>true</enabled>
    </snapshots>
    <releases>
      <enabled>true</enabled>
    </releases>
  </repository>
</repositories>
<dependencies>
  <dependency>
    <groupId>frankiesardo</groupId>
    <artifactId>icepick</artifactId>
    <version>{{latest-version}}</version>
  </dependency>
  <dependency>
    <groupId>frankiesardo</groupId>
    <artifactId>icepick-processor</artifactId>
    <version>{{latest-version}}</version>
    <optional>true</optional>
  </dependency>
</dependencies>
```

License
-------

    Copyright 2013 Frankie Sardo

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.
