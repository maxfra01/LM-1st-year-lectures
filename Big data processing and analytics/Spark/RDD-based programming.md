[[Spark]]

# Create RDDs 

We always start from a **SparkContext** object `sc`.
### RDD from files

```java
JavaRDD<String> lines = sc.textFile(inputfile); 
```

Each line of the input file is associated with a string inside the created RDD.
If the input file is stored inside HDFS the partition number is equal to the number of blocks of HDFS used to store input data.
If the `inputfile` is a folder, then Spark will read all files inside that folder.

Number of partitions can also be specified manually:

```java
JavaRDD<String> lines = sc.textFile(inputfile, 5);
```

### RDD from collections

Starting from a list in Java we can use the following method:
Number of partitions can be specified.

```java
List<String> lista = Array.asList("first", "second");
JavaRDD<String> rdd = sc.parallelize(lista, 10);
```

# Save RDDs

We can use the method `saveAsTextFile`.

```java
JavaRDD<String> result;
result.saveAsTextFile( String output_path);
```

Similar to Hadoop, Spark cannot overwrite an existing folder inside the file system.

# Retrieve Data from RDDs

It is based on `collect()`.
PAY ATTENTION TO THE SIZE OF RDDS, DO NOT USE AT THE EXAM.

```java
JavaRDD<String> lines;
List<String> temp = lines.collect();
```

# RDDs operations

**Transformations** are **lazily computed**, the new RDD is not generated until an action is executed on that new RDD.

An **action** is **immediately executed** and they return local variables, so we need to pay attention to variable size.

# Functions

### Named classes

When we execute transformations we need to pass a function (or Lambda functions): we can define user-defined function using Spark interface:

![[Pasted image 20231107145717.png]]

So, if we need to create a function returning a Boolean value:

```
class filterClass implement Function<String,Boolean>{
	public Boolean call(String s){
		return s.contains("error");
	}
}
```

Then we can use that class inside Spark driver program:

```
lines.filter( new filterClass());
```

### Anonymous classes

Same approach but we define the function each time we invoke filter:

```
JavaRDD<String> errorsRDD = inputRDD.filter(
	new Function<String, Boolean>() {
		public Boolean call(String x) {
			return x.contains("error");
		}
	}
);
```

### Lambda functions

```
JavaRDD<String> errorsRDD = inputRDD.filter(
	line -> line.contains("error");
);
```

# Transformation

![[Pasted image 20231107153002.png]]
![[Pasted image 20231107153017.png]]
![[Pasted image 20231107153027.png]]
![[Pasted image 20231107153039.png]]
![[Pasted image 20231107153051.png]]

# Actions

![[Pasted image 20231107153504.png]]
![[Pasted image 20231107153517.png]]
![[Pasted image 20231107153527.png]]
![[Pasted image 20231107153541.png]]
