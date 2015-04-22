# Giraph Tutorial

This tutorial shows how to get started with Giraph on a Hadoop YARN cluster. We assume CDH 5.3.0.

First, download [Giraph](https://giraph.apache.org/). You'll want the source distribution, which is `giraph-dist-1.1.0-src.tar.gz`.

Unpack the tarball:

```
$ tar xvfz giraph-dist-1.1.0-src.tar.gz 
```

Build:

```
$ cde giraph-1.1.0
$ mvn -Phadoop_2 -fae -DskipTests clean package
```

Here's a sample graph:

```
[0,0,[[1,1],[3,3]]]
[1,0,[[0,1],[2,2],[3,1]]]
[2,0,[[1,2],[4,4]]]
[3,0,[[0,3],[1,1],[4,4]]]
[4,0,[[3,4],[2,4]]]
```

Copy and paste the above graph data into a file called `tiny_graph.txt`. Then put the graph into HDFS:

```
$ hadoop fs -mkdir giraph_test
$ hadoop fs -mkdir giraph_test/input
$ hadoop fs -put tiny_graph.txt giraph_test/input
```

You can then run the shortest path demo:

```
hadoop jar giraph-examples/target/giraph-examples-1.1.0-for-hadoop-2.5.1-jar-with-dependencies.jar \
  org.apache.giraph.GiraphRunner \
  -D mapred.child.java.opts="-Xms10240m -Xmx15360m" \
  org.apache.giraph.examples.SimpleShortestPathsComputation \
  -vif org.apache.giraph.io.formats.JsonLongDoubleFloatDoubleVertexInputFormat \
  -vip giraph_test/input/tiny_graph.txt \
  -vof org.apache.giraph.io.formats.IdWithValueTextOutputFormat \
  -op giraph_test/output -w 3 -ca mapred.job.tracker=localhost:5431 
```

