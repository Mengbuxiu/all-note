# java email #

----------
1. JavaMailSenderImpl 配置

	> <!-- 配置MailSender -->
                <bean id="mailSender" class="org.springframework.mail.javamail.JavaMailSenderImpl">
                        <property name="host" value="smtp.163.com" />
                        <property name="port" value="25" />
                        <property name="username" value="网易用户名（邮箱没有@后面的）" />
                        <property name="password" value="密码" />
                        <property name="javaMailProperties">
                                <props>
                                        <prop key="mail.smtp.auth">true</prop>
                                        <!--关于smtp发送邮件的时长-->
                                        <prop key="mail.smtp.timeout">5000</prop>
                                        <!--关于smtp连接的时长-->
                                        <prop key="mail.smtp.connectiontimeout">3000</prop>
                                </props>
                        </property>
                </bean>
