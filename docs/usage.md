---
post_title: Usage
menu_order: 10
enterprise: 'no'
---

# Spark

Zeppelin is bundled with the Spark distribution specified by the
configuration variable `spark.uri`.  DC/OS Zeppelin fetches this
distribution and sets the `SPARK_HOME` of its Spark client to this
location.  Executors will run in the docker image specified by
`spark.executor_docker_image`.  By default, these are configured to be
a recent DC/OS Spark distribution.

## Testing

To test that Spark support in Zeppelin is working properly, run the
following minimal Spark job in a new Zeppelin note:

```
val rdd = sc.parallelize(1 to 5)
rdd.sum()
```

After a short period, "15" should be printed.

**NOTE**: The first Spark job you run on a new DC/OS cluster may take
some time as the Docker images are pulled down.
