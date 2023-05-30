# learningDSBDA


## TO RUN PROJECT:

### put file to hadoop file system:

    hadoop fs -put inputFileName inputFileName

### run the har file

    hadoop jar [file_name.jar] [package.ClassName] inputFileName outputDirName

### see the output dir (OPTIONAL)
    
    hadoop fs -ls outputDirName

### Display the output
    
    hadoop fs -cat outputDirName/part-r-00000




# Other (not required)

    su
    [password=cloudera]


    mount -t vboxsf [shared_folder_name] [location_to_copy]