# AOP (Aspect Oriented Programming)

<---maven lifecycle--->
validate(validate the project is correct and all necessary information is available),compile,test,package,verify(run any checks on results of integration tests to ensure quality criteria are met),install(to local repo),deploy(publishing to remote repo)
<---maven lifecycle--->
## AOP
- The key unit of **modularity in OOP is the class**, whereas in **AOP the unit of modularity is the aspect**.
- Aspects enable the modularization of concerns such as **transaction management, logging management, security (ie Cross cuting concerns)** that cut across multiple types and objects.
- One of the **key components of Spring is the AOP framework**. While the Spring IoC container does not depend on AOP.
- Ways of writing:-
    - **schema-based approach**
    - **@AspectJ annotation style**

## Why AOP??
- If we a common thing to e.g. log() we can separate it to a separate class and calling that class (log class) object to the required class.
- But if the question arises what is the most connected object or important object than the answer comes, the most connected and imp object is the logger object and not the business object.
- So, the logger object which is not adding a business logic is more imp which is only doing a support role 
- Having Aspect for cross cutting (transaction aspect, logging aspect, security aspect) with the aspect configuration which tells which aspect applies to which method of the object.
- If want to change anything related to calling i.e. call that logging aspect for 2 more methods or don't want to call logging aspect for some other method then have to make changes in the aspect configuration only.
- can have flexibility to run some code before or after the business logic.

## Advantage Of using Aspect Oriented Programming
1. **Scenario** : 
    - I have to maintain log and send notification after calling methods that starts from a.
2. **Problem without AOP** :
    - We can call methods (that maintains log and sends notification) from the methods starting with a. In such scenario, we need to write the code in all the 4 methods.
3. **CR** 
    - But, if client says in future, I don't have to send notification, you need to change all the methods. It leads to the maintenance problem.
4. **Solution with AOP** :
    - We don't have to call methods from the method. Now we can define the additional concern like maintaining log, sending notification etc. in the method of a class. Its entry is given in the xml file

## Steps for AOP
1. Write aspects by identifying cross-cutting concerns.
2. Configure where the aspects apply. 

## Aspect jars
- **aspectjrt** jar     AspectJ: https://www.eclipse.org/aspectj/downloads.php
- **aspect-weaver** jar
- **aopalliance** jar   AOP Alliance: http://aopalliance.sourceforge.net/
- **cgilib** jar        CGILIB: http://cglib.sourceforge.net/
- **asm** jar           Spring 3 ASM: http://asm.ow2.org/

## AOP concepts
1. **Aspect**
- a modularization of a concern that cuts across multiple classes.
- This is a **module which has a set of APIs providing cross-cutting requirements**.
- aspects are **implemented using regular classes (the schema-based approach)** or **regular classes annotated with the @Aspect annotation (the @AspectJ style)**.
- For example, **a logging module would be called AOP aspect for logging**.
- An application **can have any number of aspects depending on the requirement**.

2. **Join point**: 
- a point during the execution of a program, such as the execution of a method or the handling of an exception. 
- In Spring AOP, **a join point always represents a method execution to run a advice**.
- But in AspectJ, **a joinPoint represents much more eg if the value of a instance variable aslo changes it will run the advice**
- `public void logBefore(JoinPoint joinPoint) `
    - this joinPoint will have the data about the method for which advice is going to run.
    - eg `joinPoint.getSignature().getName()`
    - `joinPoint.getTarget()`  => **return the object whoes method was called from the advice**

3. **Advice**: 
- **action taken by an aspect at a particular join point**.
- Different types of advice include **"around", "before" and "after" advice**.
- Many AOP frameworks, including Spring, model an advice as an interceptor, maintaining a chain of interceptors around the join point.
- **Advice are called for call we make not for the calls that spring container make**

4. **Pointcut**: 
- a **predicate that matches join points**. 
- Advice is **associated with a pointcut expression and runs at any join point matched by the pointcut** (for example, the execution of a method with a certain name). 
- The concept of join points as matched by pointcut expressions is central to AOP, and **Spring uses the AspectJ pointcut expression language by default**.

5. **Introduction**: 
- declaring additional methods or fields on behalf of a type. Spring AOP allows you to introduce new interfaces (and a corresponding implementation) to any advised object. 
- For example, **you could use an introduction to make a bean implement an IsModified interface, to simplify caching. (An introduction is known as an inter-type declaration in the AspectJ community**.)

6. **Target object**: 
- object being advised by one or more aspects. 
- Also referred to as the advised object. Since Spring AOP is implemented using runtime proxies, this object will always be a proxied object.

7. **AOP proxy**: 
- an object created by the AOP framework in order to implement the aspect contracts (advise method executions and so on). 
- In the Spring Framework, an AOP proxy will be a JDK dynamic proxy or a CGLIB proxy.

8. **Weaving**: 
- **linking aspects with other application types or objects to create an advised object**. 
- This can be done at compile time (using the AspectJ compiler, for example), load time, or at runtime. 
- Spring AOP, like other pure Java AOP frameworks, performs weaving at runtime.

## Types of advice:
- **Before advice**: `@Before()`
    - Advice that **executes before a join point**, but which does not have the ability to prevent execution flow proceeding to the join point (unless it throws an exception).
- **After returning advice**: `@AfterReturning(pointcut="args(name)", returning="retrnValue")`
    - Advice to be **executed after a join point completes normally**: 
    - for example, if a method **returns without throwing an exception**.
- **After throwing advice**: `@AfterThrowing(pointcut="args(name)", throwing="exception")`
    - Advice to be **executed if a method exits by throwing an exception**.
- **After (finally) advice**: `@After()`
    - Advice to be **executed regardless of the means by which a join point exits (normal or exceptional return)**.
- **Around advice**: `@Around()`
    - Advice that **surrounds a join point such as a method invocation**. 
    - This is the most powerful kind of advice. Around advice **can perform custom behavior before and after the method invocation**. 
    - It is also responsible for choosing whether to proceed to the join point or to shortcut the advised method execution by returning its own return value or throwing an exception.
    - `ProceedingJoinPoint` is the compulsory argument which has to be taken for around advice.
    - `proceedingJoinPoint.proceed()` has to be written when want to execute the target method.
    - **This advice give more control to the return value as one can change the value that is being return**.


## Pointcut
- A join point is a **step of the program execution, such as the execution of a method or the handling of an exception**. 
- In Spring AOP, a join point always represents a method execution. 
- A **pointcut is a predicate that matches the join points** and a pointcut expression language is a way of describing pointcuts programmatically.
- Spring **AOP only supports method execution join points** for Spring beans.
- A pointcut declaration has two parts: 
    - a **signature comprising a name and any parameters**, 
    - a **pointcut expression that determines exactly which method executions** we are interested in
    -       @Pointcut("execution(* transfer(..))")                  // the pointcut expression
            private void anyOldTransfer() {}                        // the pointcut signature
    - the method serving as the `pointcut signature must have a void return type`
- Spring AOP supports the following AspectJ pointcut designators (PCD) for use in pointcut expressions:
    - call
    - get
    - set
    - preinitialization
    - staticinitialization
    - initialization
    - handler
    - adviceexecution
    - withincode
    - cflow
    - cflowbelow
    - if
    - @this
    - @withincode


### **1.Usage**
- A pointcut expression can appear as a value of the @Pointcut annotation:
-           @Pointcut("within(@org.springframework.stereotype.Repository *)")
            public void repositoryClassMethods() {}
- The method declaration is called the **pointcut signature**. 
- It **provides a name that can be used by advice annotations to refer to that pointcut**.
-           @Around("repositoryClassMethods()")
            public Object measureMethodExecutionTime(ProceedingJoinPoint pjp) throws Throwable {
                ...
            }
- A pointcut expression could also appear as the value of the expression property of an `aop:pointcut` tag:
-           <aop:config>
                <aop:pointcut id="anyDaoMethod"
                expression="@target(org.springframework.stereotype.Repository)"/>
            </aop:config>>

### **2.Pointcut Designators**
- A pointcut expression **starts with a pointcut designator (PCD), which is a keyword telling Spring AOP what to match**.
- Spring AOP supports the following AspectJ pointcut designators (PCD) for use in pointcut expressions:
1. **execution**
    - syntax `execution(modifiers-pattern? ret-type-pattern declaring-type-pattern?name-pattern(param-pattern) throws-pattern?)`
    - which **matches method execution join points**
    - ` @Pointcut(execution(public !static * *(..))")`
    - `@Pointcut("execution(public String com.deepak.dao.FooDao.findById(Long))")`
    - will match exactly the execution of findById method of the FooDao class but not flexible.
    - `@Pointcut("execution(* com.deepak.dao.FooDao.*(..))")`
    - match all the methods of the FooDao class, which may have different signatures, return types, and arguments
    - first wildcard matches any return value, the second matches any method name, and the (..) pattern matches any number of parameters (zero or more).
    - some examples

        | **pointcut expressions** | **Description** |                      
        | ------------------------ | --------------- |        
        | execution(public * *(..)) | execution of any public method |
        | execution(* set*(..)) | execution of any method with a name beginning with "set" |
        | execution(* com.deepak.service.AccountService.*(..)) | execution of any method defined by the AccountService interface | 
        | execution(* com.deepak.service.*.*(..)) | execution of any method defined in the service package |
        | execution(* com.deepak.service..*.*(..)) | execution of any method defined in the service package or a sub-package | 

      
2. **within**
    - which **matches class execution join points**
    - Another way to achieve the same result from the previous section is by using the within PCD, which limits matching to join points of certain types.
    - `@Pointcut("within(com.deepak.dao.FooDao)")`
    - We could also match any type within the com.deepak package or a sub-package.
    - `@Pointcut("within(com.deepak..*)")`
    - some examples

        | **pointcut expressions** | **Description** |                      
        | ------------------------ | --------------- |  
        | within(com.deepak.service.*) | any join point (method execution only in Spring AOP) within the service package |
        | within(com.deepak.service..*) | any join point (method execution only in Spring AOP) within the service package or a sub-package | 
        
3. **args** 
    - limits matching to join points where the arguments are instances of the given types
    - i.e. **it calls all the method where the arg matches to the no an dtype of the argument**
    -       //Advice arguments, will be applied to bean methods with single String argument
            @Before("args(name)")
            public void logStringArguments(String name){
                System.out.println("String argument passed="+name);
            }
    -       //Advice arguments, will be applied to bean methods with single String argument
            @Before("args(String)")
            public void logStringArguments(){
                System.out.println("String argument passed=");
            }
4. **@annotation(fullyQualifiedName)**
    - `@After("@annotation(com.deepak.aspect.Loggable)")`
    - `public @interface Loggable {}` : is the custom pointcut annotation and will be applied to all the method which uses has this annoation without worring for the pointcut expression.


## Enabling @AspectJ Support
### 1. **Using Spring Annotation**
- add the @EnableAspectJAutoProxy annotation
-           @Configuration
            @EnableAspectJAutoProxy                     // Enabling @AspectJ Support
            public class AppConfig {
                ...
            }

### 2. **Using XML configuration**
- use the aop:aspectj-autoproxy element
- `<aop:aspectj-autoproxy/>`
-           <?xml version="1.0" encoding="UTF-8"?>
            <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xmlns:aop="http://www.springframework.org/schema/aop"                   // aop schema
                xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> 
                <!-- bean definitions here -->
            </beans>

## Declaring an aspect
### 1. **Using Spring Annotation**
- **@Aspect and @Component**   
-           package com.deepak;
            import org.aspectj.lang.annotation.Aspect;
            import org.springframework.stereotype.Component; 

            @Aspect                     // For aspect
            @Component                  // For Bean
            public class EmployeeServiceAspect { }

### 2. **Using XML configuration**
- **for aspect**
-           package com.deepak;
            import org.aspectj.lang.annotation.Aspect;

            @Aspect
            public class NotVeryUsefulAspect {

            }
- **for bean declaration**            
-           <bean id="myAspect" class="com.deepak.NotVeryUsefulAspect">
                <!-- configure properties of aspect here as normal -->
            </bean>
**Note** :- @Aspect annotation on a class marks it as an aspect, and hence excludes it from auto-proxying.


## Determining argument names
- The **parameter binding in advice invocations relies on matching names** used in pointcut expressions to declared parameter names in (advice and pointcut) method signatures. Parameter names are not available through Java reflection, so Spring AOP uses the following strategies to determine parameter names:
1. If the parameter names have been **specified by the user explicitly**, then the specified parameter names are used: **both the advice and the pointcut annotations have an optional "argNames" attribute which can be used to specify the argument names** of the annotated method - these argument names are available at runtime. For example:
    -       @Before(value="com.deepak.lib.Pointcuts.anyPublicMethod() && target(bean) && @annotation(auditable)",
                argNames="bean,auditable")
            public void audit(Object bean, Auditable auditable) {
                AuditCode code = auditable.value();
                // ... use code and bean
            }
2. **If the first parameter is of the JoinPoint, ProceedingJoinPoint**, or JoinPoint.StaticPart type, you may **leave out the name of the parameter from the value of the "argNames" attribute**. 
    - For example, if you **modify the preceding advice to receive the join point object, the "argNames" attribute need not include it**:
    -       @Before(value="com.deepak.lib.Pointcuts.anyPublicMethod() && target(bean) && @annotation(auditable)",
                    argNames="bean,auditable")
            public void audit(JoinPoint jp, Object bean, Auditable auditable) {
                AuditCode code = auditable.value();
                // ... use code, bean, and jp
            }

## Advice ordering
1. What happens when multiple pieces of advice all want to run at the same join point?
    - Spring AOP follows the same precedence rules as AspectJ to determine the order of advice execution.   
    - The **highest precedence advice runs first "on the way in"** (so given two pieces of **before advice, the one with highest precedence runs first**). 
    - **"On the way out" from a join point, the highest precedence advice runs last** (so given two pieces of a**fter advice, the one with the highest precedence will run second**). 
2. When two pieces of **advice defined in different aspects both need to run at the same join point**, 
    - unless you specify otherwise the **order of execution is undefined**. 
    - This is done in the normal Spring way **by either implementing the org.springframework.core.Ordered interface in the aspect** class or **annotating it with the Order annotation**.
    - Given two aspects, **the aspect returning the lower value from Ordered.getValue() (or the annotation value) has the higher precedence**.           
3. When **two pieces of advice defined in the same aspect** both need to run at the same join point, 
    - the ordering is undefined (since there is no way to retrieve the declaration order via reflection for javac-compiled classes). 
    - **Consider collapsing such advice methods into one advice method per join point in each aspect class**, or refactor the pieces of advice into separate aspect classes - which can be ordered at the aspect level.    

## Introduction
1. Introductions (known as **inter-type declarations** in AspectJ) enable an aspect to declare that **advised objects implement a given interface**, and to **provide an implementation of that interface on behalf of those objects**.
2. An introduction is made **using the @DeclareParents** annotation. 
3. This annotation is **used to declare that matching types have a new parent** (hence the name). 
    - For example, given an interface UsageTracked, and an implementation of that interface DefaultUsageTracked, the following aspect declares that all implementors of service interfaces also implement the UsageTracked interface.
4.          @Aspect
            public class UsageTracking {

                @DeclareParents(value="com.deepak.myapp.service.*+", defaultImpl=DefaultUsageTracked.class)
                public static UsageTracked mixin;

                @Before("com.deepak.myapp.SystemArchitecture.businessService() && this(usageTracked)")
                public void recordUsage(UsageTracked usageTracked) {
                    usageTracked.incrementUseCount();
                }
            }
5. The interface to be implemented is determined by the type of the annotated field.
6. The value attribute of the @DeclareParents annotation is an AspectJ type pattern :- any bean of a matching type will implement the UsageTracked interface.

## Schema-based AOP
1. need to import the spring-aop schema          
    -       <beans xmlns="http://www.springframework.org/schema/beans"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xmlns:aop="http://www.springframework.org/schema/aop"                   // aop schema
                xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                    http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd"> 
                <!-- bean definitions here -->
            </beans>
2. Within your Spring configurations, **all aspect and advisor elements must be placed within** an `<aop:config>` element (you can have more than one <aop:config> element in an application context configuration). An <aop:config> element can contain pointcut, advisor, and aspect elements

### 1.**Declaring an aspect**
            <aop:config>
                <aop:aspect id="myAspect" ref="aBean">
                    ...
                </aop:aspect>
            </aop:config>

            <bean id="aBean" class="...">
                ...
            </bean>       
- The bean backing the aspect ("aBean" in this case) can of course be configured and dependency injected just like any other Spring bean.

### 2.**Declaring a pointcut**
-           <aop:config>

                <aop:aspect id="myAspect" ref="aBean">

                    <aop:pointcut id="businessService"
                        expression="execution(* com.deepak.myapp.service.*.*(..))"/>

                    ...

                </aop:aspect>

            </aop:config>
- combining pointcut
-           <aop:config>

                <aop:aspect id="myAspect" ref="aBean">

                    <aop:pointcut id="businessService"
                        expression="execution(* com.deepak.myapp.service.*.*(..)) &amp;&amp; this(service)"/>

                    <aop:before pointcut-ref="businessService" method="monitor"/>

                    ...

                </aop:aspect>

            </aop:config>   

            // method
            public void monitor(Object service) {
                ...
            } 
- When combining pointcut sub-expressions, **&& is awkward within an XML document**, and so the keywords **and, or, and not can be used in place of &&, ||, and !** respectively         
-           <aop:config>

                <aop:aspect id="myAspect" ref="aBean">

                    <aop:pointcut id="businessService"
                        expression="execution(* com.deepak.myapp.service..(..)) and this(service)"/>

                    <aop:before pointcut-ref="businessService" method="monitor"/>

                    ...
                </aop:aspect>
            </aop:config>           

### 3.**Declaring advice**
1. **Before advice**
- Before advice runs before a matched method execution. It is declared inside an <aop:aspect> using the <aop:before> element.
-           <aop:aspect id="beforeExample" ref="aBean">
                <aop:pointcut id="dataAccessOperation"
                        expression="execution(* com.deepak.myapp.service..(..)) "/>

                <aop:before
                    pointcut-ref="dataAccessOperation"
                    method="doAccessCheck"/>
                <aop:before
                    pointcut="execution(* com.deepak.myapp.dao.*.*(..))"
                    method="doAccessCheckAnother"/>
                ...

            </aop:aspect>

2. **After returning advice**
- it is **possible to get hold of the return value within the advice body**. Use the **returning attribute to specify the name of the parameter** to which the return value should be passed:
-               <aop:aspect id="afterReturningExample" ref="aBean">

                    <aop:after-returning
                        pointcut-ref="dataAccessOperation"
                        returning="retVal"
                        method="doAccessCheck"/>

                    ...

                </aop:aspect>


                // advice method
                // The doAccessCheck method must declare a parameter named retVal
                public void doAccessCheck(Object retVal) {... }

3. **After throwing advice**
- it is **possible to get hold of the thrown exception within the advice body**. Use the **throwing attribute to specify the name of the parameter** to which the exception should be passed.
-               <aop:aspect id="afterThrowingExample" ref="aBean">

                    <aop:after-throwing
                        pointcut-ref="dataAccessOperation"
                        throwing="dataAccessEx"
                        method="doRecoveryActions"/>

                    ...

                </aop:aspect>


                // advice method
                // doRecoveryActions method must declare a parameter named dataAccessEx 
                public void doRecoveryActions(DataAccessException dataAccessEx) {...}

4. **After (finally) advice**
-               <aop:aspect id="afterFinallyExample" ref="aBean">

                    <aop:after
                        pointcut-ref="dataAccessOperation"
                        method="doReleaseLock"/>

                    ...

                </aop:aspect>

5. **Around advice**
-  It has the opportunity to do **work both before and after the method executes**, and to **determine when, how, and even if, the method actually gets to execute at all**. 
- Around advice is often **used if you need to share state before and after a method execution in a thread-safe manner** 
- eg.(**starting and stopping a timer**).     
-               <aop:aspect id="aroundExample" ref="aBean">

                    <aop:around
                        pointcut-ref="businessService"
                        method="doBasicProfiling"/>

                    ...

                </aop:aspect>

                // advice method
                public Object doBasicProfiling(ProceedingJoinPoint pjp) throws Throwable {
                    // start stopwatch
                    Object retVal = pjp.proceed();
                    // stop stopwatch
                    return retVal;
                }