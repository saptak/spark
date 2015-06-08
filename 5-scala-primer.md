#A short primer on Scala

Scala is relatively new language based on the JVM. The main difference between other “Object Oriented Languages” and Scala is that everything in Scala is an object. The primitive types that are defined in Java, such as int or boolean, are objects in Scala. Functions are treated as objects, too. As objects, they can be passed as arguments, allowing a functional programming approach to writing applications for Apache Spark.

If you have programmed in Java or C#, you should feel right at home with Scala with very little effort.

###Values

Values can be either mutable or immutable. Mutable values are expressed by the `var` keyword.

```
var a: Int = 5
a = a + 1
println(a)
```
![](https://www.dropbox.com/s/89ly7aj7yg6ssdb/Screenshot%202015-06-08%2012.26.39.png?dl=1)

Immutable values are expressed by the `val` keyword.

```
val b: Int = 7
b = b + 1 //Error
println(b)
```
![](https://www.dropbox.com/s/48flcm9q1zlg165/Screenshot%202015-06-08%2012.29.13.png?dl=1)
