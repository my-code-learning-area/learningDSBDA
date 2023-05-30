# map function:


    String lines = values.toString();
    String[] line = lines.split("\n");
    for(String words: line){
        String[] data = words.split(",");
        Text outputKey = new Text(data[0]);
        IntWritable outputValue = new IntWritable(1);
        con.write(outputKey, outputValue);
    }


# reduce function:


    private Text maxword = new Text("Th number of unique listeners are: ");
    int count = 0;

            int sum = 0;
			for(IntWritable value: values){
				sum+=value.get();
			}
			count++;

        con.write(maxword, new IntWritable(count));