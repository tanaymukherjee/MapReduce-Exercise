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
docker run \
  -v $(pwd):/usr/local/hadoop/py \
  -it sequenceiq/hadoop-docker:2.7.1 \
  /usr/local/hadoop/py/py_runner.sh grep
```
(notice the **grep** keyword at the end - corresponds to the folder **grep**!)

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
