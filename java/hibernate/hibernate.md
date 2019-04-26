# hibernate #

----------
1. hibernate 注解字段默认值的设置:
     
    @Column(name="ISPUBLIC" ,nullable=false,
	columnDefinition="INT default 0")
	private int isPublic;

	> 注意字段的类型必须指定，因为hibernate 会把columnDefinition 的内容直接写在生成标的ddl中，因此语法必须正确。小记一下，以防忘记。nullable 表示定义的字段可以为空.