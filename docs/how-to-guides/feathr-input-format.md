---
layout: default
title: Input File for Feathr
parent: How-to Guides
---

# Input File Format for Feathr

Feathr supports multiple file formats, including Parquet, ORC, Avro, JSON, Delta Lake, and CSV. The formats are recognized in the following order:

1. If the input path has a suffix, that will be honored. For example, `wasb://demodata@demodata/user_profile.csv` will be recognized as csv, while `wasb://demodata@demodata/product_id.parquet` will be recognized as parquet. Note that this is a per file behavior.
2. If the input file doesn't have a name, say `wasb://demodata@demodata/user_click_stream`, users can optionally set a parameter to let Feathr know which format to read those files. Refer to the `spark.feathr.inputFormat` setting in [Feathr Job Configuration](./feathr-job-configuration.md) for more details on how to set those, as well as for code examples. Note that this is a global setting that will apply to every input which the format is not recognized.
3. If all the above conditions are not recognized, Feathr will use `avro` as the default format.

## Special note for spark outputs

Many Spark users will use delta lake format to store the results. In those cases, the result folder will be something like this:
![Spark Output](../images/spark-output.png)

Please note that although the results are shown as "parquet", you should use the path of the parent folder and use `delta` format to read the folder.

# TimePartitionPattern for input files
When data sources are defined by 'HdfsSource', feathr supports 'time_partition_pattern' to match paths of input files. For example, given time_partition_pattern = 'yyyy/MM/dd' and a 'base_path', all available input files under paths 'base_path'/yyyy/MM/dd will be visited and used as data sources.

More reference on the APIs:

- [MaterializationSettings API doc](https://feathr.readthedocs.io/en/latest/feathr.html#feathr.MaterializationSettings)