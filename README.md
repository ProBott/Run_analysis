Data Cleaning and Transformation

The data transformation steps are as follows:

1.Test and training data is downloaded as UCI HAR Dataset in the working directory ‘~/R/data/‘ from the following url:
https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip

2. Each data set is read in using read.table

3. For each measurement, including subjects and activities, test and training data sets are merged by row using mergefile function. The resulting merged files are stored in the global environment for future use.

4. Original test and training files are removed from the workspace.

5. Mean and standard deviation are calculated for each measurement by row using apply function, resulting in a new data frame, meansd.

6. Activity descriptions are added as factor to the merged label file.

7. Aggregate the means for each measurement by groups of subject ID and activity time.

8. Write .txt file to the working directory.
