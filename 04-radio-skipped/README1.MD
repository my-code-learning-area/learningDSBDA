# map function:


    String lines = values.toString();
    String[] line = lines.split("\n");
    for(String words: line){
        String[] data = words.split(",");
        Text outputKey = new Text(data[1]);
        IntWritable outputValue = new IntWritable(Integer.parseInt(data[4]));
        con.write(outputKey, outputValue);
    }


# reduce function:


    int sum = 0;
    for(IntWritable value: values){
        sum+=value.get();
    }
    con.write(word, new IntWritable(sum));