# spring-code-learn #

----------
1.读取请求流中的数据

	> InputStream in = request.getInputStream();
	ByteArrayOutputStream outputStream = new ByteArrayOutStream();
	byte[] byte = new byte[10240];
	int count = -1;
	while(-1 != (count = in.read.(data,0,data.length))){
	outputStream.write(data,0,count);
	}
	json = new String(outputStream.toByteArray(),"UTF-8")