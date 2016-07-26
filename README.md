# JMH_SPL Maven plugin

This project is Maven plugin for easy integration of benchmarks using JMH with our SPL evaluation engine.


## Data saver goal

Data_saver goal of this plugin handles saving measured data from `benchmarks.jar` in JSON format into configured directory. Also, custom revision identifier can be specified.

By default this goal is executed in maven _verify_ phase, because it's after _package_ phase where `benchmarks.jar` file is created.

For using this plugin goal you need to add these lines into your benchmark project `pom.xml`:

```{.xml}
<plugin>
    <groupId>cz.cuni.mff.d3s.spl</groupId>
    <artifactId>jmh_spl-maven-plugin</artifactId>
    <version>1.0-SNAPSHOT</version>
    <executions>
        <execution>
            <id>Generate data</id>
            <phase>verify</phase>
            <goals>
                <goal>data_saver</goal>
            </goals>
        </execution>
    </executions>
    <configuration>
        <jmhJar>${project.build.directory}/${uberjar.name}.jar</jmhJar>
        <resultPath>${project.basedir}/jmh_results</resultPath>
    </configuration>
</plugin>
```

There are several configuring options:

| pom.xml | property | default value | description |
| --- | --- | --- | --- |
| revisionID | data_saver.revision_id | last | Identifier of current revision. Possibly existing data will be overwriten. |
| jmhJar | data_saver.benchmarks_jar | | Path to `benchmarks.jar` generated by JMH build. |
| resultPath | data_saver.result_path | | Path to directory, where are stored measured data in Json format. Will be created if not exists. |
| additionalOpts | data_saver.additional_options | "" | Arguments which will be directly passed to `benchmarks.jar` while executing. For example "-v SILENT -foe true" |
| skip | data_saver.skip | false | Skip goal execution. |

You can specify these options from command line with property identifier:

```{.sh}
$ mvn clean install -Ddata_saver.revision_id=ver3
```


## SPL annotation goal

This goal is used for distribution of _SPLFormula_ annotation for further processing. Default execution phase is _generate-sources_. SPLFormula.java file is copied to `target/generated-sources/spl_annotations/cz/cuni/mff/d3s/spl` directory. Compilation CLASSPATH is also altered.

`pom.xml` snippet of benchmarking project:

```{.xml}
<execution>
    <id>Provide SPL annotation</id>
    <phase>generate-sources</phase>
    <goals>
        <goal>spl_annotation</goal>
    </goals>
</execution>
```

