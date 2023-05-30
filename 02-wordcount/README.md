# map function:


    String[] words = value.toString().split(",")
    for (String word : words) {
        con.write(new Text(word), new IntWritable(1));
    }


# reduce function:


    int sum = 0;
    for(IntWritable value : values) {
        sum += value.get();
    }
    con.write(new Text(key), new IntWritable(sum));