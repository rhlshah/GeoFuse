# GeoFuse : Spatial Joining Made Simple with PostgreSQL and Apache Spark

The required task is to understand and implement two tasks: i) Parallel spatial joining technique on-top of an open-source relational database management system (i.e., PostgreSQL) 
using PostGIS spatial extension and ii) Spatial joining with a map reduce program on top of Apache Spark using the Apache Sedona spatial extension.

Input Data: Input datasets consist of two spatial datasets: a point dataset and a rectangle dataset. The point dataset (points.csv) consists of latitudes and longitudes of various points while the rectangle dataset (rectangles.csv) consists of latitudes and longitudes of two diagonal points of rectangles.
Each row in points.csv file has the format longitude,latitude while the same for the rectangles.csv file is longitude1,latitude1,longitude2,latitude2.
Our goal is to perform spatial join between points dataset and rectangles dataset and return the number of points inside each rectangle (including points on rectangle boundary).

Part A (Parallel Spatial Join with PostGIS)
We load the points and rectangles datasets into two PostgreSQL tables: points and rectangles respectively. The points table has a geometry type column geom which denotes Point geometry. The rectangles table has a geometry type column geom which denotes Envelop geometry (rectangle). You are given a Python file Assignment2_Interface.py with an incomplete function parallelJoin. You need to complete this function. It has following attributes: pointsTable, rectsTable, outputTable, outputPath, and openConnection. The high-level objective is to find the number of points inside each rectangle save it to the outputTable and outputPath. Perform the following tasks:
• Create four partitions/fragments of both pointsTable and rectsTable. You should consider space partitioning such that all points or rectangles of a partition should lie within the corresponding fragment. Fragments doesn’t need to satisfy disjointness property.
• Run four parallel threads. Each thread should perform a spatial join between a fragment of pointsTable and a fragment of rectsTable. The purpose of the join is to find the number of points (pointsTable.geom) inside each rectangle or Envelop (rectsTable.geom) within the corresponding fragment. You must make use of ST_Contains method supported by PostGIS.
• Sort the output of each fragment in the ascending order of counts of points inside the parallel threads.
• Merge the outputs of four parallel joins into outputTable in the ascending order of counts of points. You can design the structure of the outputTable as you wish, but it should have a column named points_count containing counts of points.
• Write the counts of points into the outputPath in the ascending order. You don’t need to write rectangle coordinates.


Part B (Spatial Join with Map Reduce on Apache Sedona)
Similar to Part A, you need to perform a spatial join between the points dataset and rectangles dataset in order to count the number of points located within each rectangle. 
This time, you will perform the task with the help of map and reduce functionalities supported by Spark.
Task: You are given a Apache Sedona project named Map-Reduce-Apache-Sedona. Find the Scala file SparkMapReduce.scala. Complete the incomplete function runMapReduce(). 
Instead of using group by operation, you must make use of Spark map and reducebyKey operations to complete the task. Covert the resultant RDD back to a DataFrame before returning the result. 
The returned DataFrame should contain the number of points within each rectangle in an ascending order of count values. You don’t need to return rectangles.
