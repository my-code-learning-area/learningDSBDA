# map function:


    String[] lines = value.toString().split("\n");
    for (String line : lines) {
        String[] singleData = line.split(",");
        Text outputkey = new Text(singleData[1]);
        IntWritable outputValue = new IntWritable(
            Integer.parseInt(singleData[2])
        );
        con.write(outputkey, outputValue);
    }


# reduce function:


    private Text Maxword = new Text();
    private int max = 0;

            int sum = 0;
			for (IntWritable value : values) {
				sum += value.get();
			}
			if (sum > max) {
				max = sum;
				Maxword.set(key);
			}

        con.write(new Text(Maxword), new IntWritable(max));