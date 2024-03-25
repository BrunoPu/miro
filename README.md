
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("MySparkApp") \
    .config("spark.sql.broadcastTimeout", "-1") \
    .getOrCreate()
