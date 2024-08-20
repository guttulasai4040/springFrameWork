# springFrameWork

package com.example.ioc;

public interface GreetingService {
    String greet(String name);
}


package com.example.ioc;

public class EnglishGreetingService implements GreetingService {
    @Override
    public String greet(String name) {
        return "Hello, " + name;
    }
}

package com.example.ioc;

public class SpanishGreetingService implements GreetingService {
    @Override
    public String greet(String name) {
        return "Hola, " + name;
    }
}

package com.example.ioc;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class GreetingController {

    private final GreetingService greetingService;

    @Autowired
    public GreetingController(GreetingService greetingService) {
        this.greetingService = greetingService;
    }

    public void sayHello(String name) {
        String message = greetingService.greet(name);
        System.out.println(message);
    }
}

package com.example.ioc;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class AppConfig {

    @Bean
    public GreetingService greetingService() {
        return new EnglishGreetingService();
    }

    @Bean
    public GreetingController greetingController() {
        return new GreetingController(greetingService());
    }
}

package com.example.ioc;

import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class IoCApplication {
    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        GreetingController controller = context.getBean(GreetingController.class);
        controller.sayHello("John");
    }
}


<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="englishGreetingService" class="com.example.ioc.EnglishGreetingService" />

    <bean id="greetingController" class="com.example.ioc.GreetingController">
        <constructor-arg ref="englishGreetingService" />
    </bean>

</beans>
