Neo4j Scala wrapper library
=======================

Notice
------

It's been a while since I worked on this project, so you might want to check [FaKod's fork](https://github.com/FaKod/neo4j-scala) which is much more active and up to date.

Presentation
------------

The Neo4j Scala wrapper library allows you the [Neo4j open source graph database](http://neo4j.org/) through a
domain-specific simplified language. It is written in Scala and is intended to be used in other Scala projects.

This wrapper is mostly based on the work done by [Martin Kleppmann](http://twitter.com/martinkl) in his [Scala implementation of RESTful JSON HTTP resources on top of the Neo4j graph database and Jersey](http://github.com/ept/neo4j-resources) project. I thought it'd be usefull to extract the Neo4j DSL into a seperate project, and Marting agreed to this.

Building
--------

You need a Java 5 (or newer) environment and Maven 2.0.9 (or newer) installed:

    $ mvn --version
    Apache Maven 3.0-alpha-5 (r883378; 2009-11-23 16:53:41+0100)
    Java version: 1.6.0_15
    Java home: /usr/lib/jvm/java-6-sun-1.6.0.15/jre
    Default locale: en_US, platform encoding: UTF-8
    OS name: "linux" version: "2.6.31-12-generic" arch: "i386" Family: "unix"

You should now be able to do a full build of `neo4j-resources`:

    $ git clone git://github.com/jawher/neo4j-scala.git
    $ cd neo4j-scala
    $ mvn clean install

To use this library in your projects, add the following to the `dependencies` section of your `pom.xml`:

    <dependency>
      <groupId>org.neo4j</groupId>
      <artifactId>neo4j-scala</artifactId>
      <version>0.9.9-SNAPSHOT</version>
    </dependency>

If you don't use Maven, take `target/neo4j-scala-0.9.9-SNAPSHOT.jar` and all of its dependencies, and add them to your classpath.


Troubleshooting
---------------

Please consider using [Github issues tracker](http://github.com/jawher/neo4j-scala/issues) to submit bug reports or feature requests.


Using this library
------------------

Using this wrapper, this is how creating two relationships can look in Scala:

    start --> "KNOWS" --> intermediary --> "KNOWS" --> end

And this is how getting and setting properties on a node or relationship looks like :

    start("foo") = "bar"
    start("foo") match {
      case Some(x) => println(x)
      case None => println("aww")
    }

Besides, the neo4j scala binding makes it possible to write stop and returnable evaluators in a functional style :

    //StopEvaluator.END_OF_GRAPH, written in a Scala idiomatic way :
    start.traverse(Traverser.Order.BREADTH_FIRST, (tp : TraversalPosition) => false, ReturnableEvaluator.ALL_BUT_START_NODE, DynamicRelationshipType.withName("foo"), Direction.OUTGOING)
    
    //ReturnableEvaluator.ALL_BUT_START_NODE, written in a Scala idiomatic way :
    start.traverse(Traverser.Order.BREADTH_FIRST, StopEvaluator.END_OF_GRAPH, (tp : TraversalPosition) => tp.notStartNode(), DynamicRelationshipType.withName("foo"), Direction.OUTGOING)


License
-------

See `LICENSE` for details.

