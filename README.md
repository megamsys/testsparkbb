# testsparkbuilder

### Gradle tests for our [sparkbuilder](https://github.com/megamsys/sparkbuilder)

```

gradle build


```

This will generate the jar testsparkbuild/build/libs/testsparkbuild.jar

### Setup jobserver

```

sudo systemctl start docker

sudo docker pull velvia/spark-jobserver:0.6.1.mesos-0.25.0.spark-1.5.2

sudo docker run -d -p 8090:8090 velvia/spark-jobserver:0.6.1.mesos-0.25.0.spark-1.5.2


```

### Jobserver running

To check if jobserver is running [http://localhost:8090](http://localhost:8090)

### Upload the testsparkbb.jar

Its weird that you'll have be one level higer that then project directory.

eg: If we have `yanpi` directory, to use curl and successfuly upload the testsparkbb.jar

```
cd  yonpis

curl --data-binary @testsparkbb/build/libs/testsparkbb.jalocalhost:8090/jars/haa01

```
### Cool, you should be done..

This is a simple WorkCountExample

## Run it

### Valid

```
curl -d "input.string = a b c a b see" 'localhost:8090/job?appName=haa01&classPath=io.megam.sparkbb.WordCountExample'
```

```json
{
  "status": "STARTED",
  "result": {
    "jobId": "5507e06e-ceb7-4e05-b905-f59aa62542a2",
    "context": "1cf49600-io.megam.sparkbb.WordCountExample"
  }
}
```

```
curl localhost:8090/jobs/5507e06e-ceb7-4e05-b905-f59aa62542a2
```

```json
{
  "duration": "1.516 secs",
  "classPath": "io.megam.sparkbb.WordCountExample",
  "startTime": "2015-12-10T12:19:58.936Z",
  "context": "1cf49600-io.megam.sparkbb.WordCountExample",
  "result": {
    "a": 2,
    "b": 2,
    "see": 1,
    "c": 1
  },
  "status": "FINISHED",
  "jobId": "5507e06e-ceb7-4e05-b905-f59aa62542a2"
}
```


### Invalid

```
curl -i -d "bad.input=abc" 'localhost:8090/jobs?appName=haa01&classPath=io.megam.sparkbb.WordCountExample'
HTTP/1.1 400 Bad Request
Server: spray-can/1.3.3
Date: Thu, 10 Dec 2015 12:13:36 GMT
Access-Control-Allow-Origin: *
Content-Type: application/json; charset=UTF-8
Content-Length: 656

```

```json
{
  "status": "VALIDATION FAILED",
  "result": {
    "message": "No input.string config param",
    "errorClass": "java.lang.Throwable",
    "stack": ["spark.jobserver.JobManagerActor$$anonfun$spark$jobserver$JobManagerActor$$getJobFuture$4.apply(JobManagerActor.scala:246)", "scala.concurrent.impl.Future$PromiseCompletingRunnable.liftedTree1$1(Future.scala:24)", "scala.concurrent.impl.Future$PromiseCompletingRunnable.run(Future.scala:24)", "java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1145)", "java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:615)", "java.lang.Thread.run(Thread.java:745)"]
  }
}
```json
