# MapReduce

This repo provides two examples and a script to easily define and run mapper and reducer functions against Hadoop MapReduce.

Generally speaking, on the top level there are folders that must contain:

* mapper.py
* reducer.py
* sample directory with data to be copied in hdfs within docker container

__ENSURE THAT .py FILES HAVE `chmod +x` PERMISSIONS__
(This is hadoop requirement)

Of course, the idea is to add more folders that demonstrate different aggregations that can be achieved with MapReduce over different datasaets (that are available in the sample folder). The aspiration was to make something reusable quickly and cheaply.

## Usage

1. Download this repo
2. CD into this repo
3. Run the following

```
docker run -v $(pwd):/usr/local/hadoop/py -it sequenceiq/hadoop-docker:2.7.1 /usr/local/hadoop/py/py_runner.sh basic_grep
```
(notice the **grep** keyword at the end - corresponds to the folder **basic_grep**!)

expected output:

```
foo	6
quux	4
```
Another example:

```
docker run \
  -v $(pwd):/usr/local/hadoop/py \
  -it sequenceiq/hadoop-docker:2.7.1 \
  /usr/local/hadoop/py/py_runner.sh count
```
(notice the **count** keyword at the end  - corresponds to the folder **count**!)

expected output:

```
bar	0
foo	6
labs	0
quux	4
```

## Additional Exercises

### A) Using Regex:
The sample files were changed from the original one in basic_grep/ count to add some words to test for the given conditions.

#### 1. Check for all the words that have "oo" as part of the string
* ```Command```
```
docker run -v $(pwd):/usr/local/hadoop/py -it sequenceiq/hadoop-docker:2.7.1 /usr/local/hadoop/py/py_runner.sh exercise "oo"
```
* ```Output```
```
foo  6
```

#### 2. Check for all words that have 'k' in the beginning and 'e' at the end of the string
* ```Command```
```
docker run -v $(pwd):/usr/local/hadoop/py -it sequenceiq/hadoop-docker:2.7.1 /usr/local/hadoop/py/py_runner.sh exercise "^k[a-zA-Z]*e$"
```
* ```Output```
```
kettle  1
kite    1
```

#### 3. Check for all words that have only 'one' capital letters positioned anywhere in the string
* ```Command```
```
docker run -v $(pwd):/usr/local/hadoop/py -it sequenceiq/hadoop-docker:2.7.1 /usr/local/hadoop/py/py_runner.sh exercise "^[a-z]*[A-Z][a-z]*$"
```
* ```Output```
```
Hello   1
Tanay   1
World   1
hEllo   1
wOrld   1
```

### B) Using SQL Query:
A sample database was created and we inserted some values to test the given conditions by running some queries.

#### 1. Check for all the words that have "oo" as part of the string
* ```Query```
```
SELECT str, count(*) FROM word WHERE str LIKE '%oo%'
```
* ```Output```
```
[('FooBar',), ('foobar',), ('bookworm',),(3,)]
```

#### 2. Check for all words that starts with f/F
* ```Query```
```
SELECT Count(*) FROM word WHERE str LIKE 'f%' OR str LIKE 'F%'
```
* ```Output```
```
[(2,)]
```

#### 3. Check for all words that have capital letters positioned anywhere in the string
* ```Command```
```
SELECT str, Count(*) FROM word WHERE LOWER(str) != str
```
* ```Output```
```
[('FooBar', 1)]
```

## References
- [Regex in Python](https://docs.python.org/3/library/re.html)
- [Online Regex checker](https://regex101.com/)
- [Regex reference for only one capital letter](https://stackoverflow.com/questions/2602993/a-regular-expression-that-will-allow-a-string-with-only-one-capital-letter)
