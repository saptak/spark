


There is a :sh command in the Spark shell that lets you submit linux cmd line commands:

```
scala> :sh sudo jps
res4: scala.tools.nsc.interpreter.ProcessResult = sudo jps (7 lines, exit 0)
```

The res4 output that you see stands for ‘result #4’.

Now, print the output of result 2:

```
scala> res4.show
12509 jar
30843 SparkSubmit
22616 Jps
14703 DseSparkMaster
22541 CoarseGrainedExecutorBackend
7777 DseDaemon
14759 DseSparkWorker</p>
```

Now that we’ve launched the Spark shell, more JVMs have instantiated to support the Shell, namely the SparkSubmit and CoarseGrainedExecutorBackend. The SparkSubmit is the driver for our ‘Spark shell” application and the CoarseGrainedExecutorBackend is the sole Executor running to support our application.
