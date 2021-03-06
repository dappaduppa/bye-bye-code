---
layout: post
title:  "Java Annotation!"
date:   2017-08-22 11:42:46 +0900
categories: Java Annotation
---


**Java Annotation**

Builtin annotations:  1

@Override  
@Deprecated    
@SuppressWarnings  

Java annotation package:  

    @Documented  
    @Target(ElementType.TYPE / ElementType.CONSTRUCTOR)
    @Retention(RetentionPolicy.SOURCE / RetentionPolicy.CLASS / RetentionPolicy.RUNTIME)  
    @Inherited  

**RealTimeUsage: **

```java

public enum DBTablesEnum {
    ORDERS("orders"),
    ORDER_ITEMS("order_items");

    private final String tableName;

    // why private : because enum constructor should be private
    private DBTablesEnum(String tableName) {
        this.tableName = tableName;
    }
    
    public String getValue() {
    	return this.tableName;
    }
}

@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.CLASS)
public @interface DBTablesAnn {
    DBTablesEnum dbTable();
}

// Usage
@DBTablesAnn(dbTable = DBTablesEnum.ORDER_ITEMS) 
// if it is defined "value()" instead of "dbTable()" in DBTableAnn
// then instead of 
//      @DBTablesAnn(dbTable = DBTablesEnum.ORDER_ITEMS) 
//  ==>    @DBTablesAnn(DBTablesEnum.ORDER_ITEMS) is enough
public class OrderItemsDao {

}

```
**Repeatable annotation**

Old way:

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
//@Repeatable(Hints.class)
public @interface Hint {
	String value();
}

@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface Hints {
	Hint[] value();
}


//old way: 
@Hints({ @Hint("fun"), @Hint("fun1") })

//new way: 
//@Hint("fun")
//@Hint("fun1")
public class SampleRepeatableAnn1 {

}

```   


**New way**

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
// this is the addition , but Hints.class is required anyway. 
// No change in Hints.class
// but Hints.class implicity setup 
@Repeatable(Hints.class) 
public @interface Hint {
	String value();
}

//old way: 
//@Hints({ @Hint("fun"), @Hint("fun1") })

//new way: 
@Hint("fun")
@Hint("fun1")
public class SampleRepeatableAnn1 {
		System.out.println(SampleRepeatableAnn1.class.getAnnotation(Hint.class)); // null
		
		// @effjava.Hints(value=[@effjava.Hint(value=fun), @effjava.Hint(value=fun1)])
		System.out.println(SampleRepeatableAnn1.class.getAnnotation(Hints.class)); 
		
		// [@effjava.Hint(value=fun), @effjava.Hint(value=fun1)]
		System.out.println(Arrays
				.asList(SampleRepeatableAnn1.class.getAnnotationsByType(Hint.class)
				//.stream()
			//	.collect(Collectors.toList()
				)); 
}
```

**Template for Repeating annotation**

```java
import java.lang.annotation.Repeatable;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;

public class RepeatingAnnotations {

    //TODO rename the xxxsss , ( plural annotation)
    //TODO rename yyy ( repeated annotation in  xxxsss)
    @Retention( RetentionPolicy.RUNTIME )
    public @interface xxxsss {
        yyy[] value() default{};
    }


    @Repeatable(value = xxxsss.class )
    public @interface yyy {
        String value();
    };

    //TODO sample usage
    //TODO set value in ""
    //TODO rename "TobeUsed"
    //TODO expand the list {} as much as you desire
    @xxxsss({@yyy(""), @yyy("")})
    public interface TobeUsed {

    }
}
```


* parameterless annotation:
    @Override

* parameter with a name called "value"
    @SuppressWarnings(value = "unchecked")

* parameter name can be omitted if there is only one parameter
    @SuppressWarnings("unchecked")


* repeating annotation ( allowed from Java SE 8 )

    @Author(name = "dhanveer")
    @Author(name = "dhanasekaran")
    class MyClass {
        ...
        ...
    }


things you already know :
    predefined annotations

    @Deprecated --> field, method, class
    @deprecated --> javadoc
    @Override
    @SuppressWarnings  --> @SuppressWarnings({"deprecation, "unchecked"})

@SafeVarargs --> method, and constructor level

@FunctionalInterface --> Java SE 8
    type declaration is intended to be a functional interface.

Annotation applies to other annotation:
---------------------------------------
    --> called meta-annotation

    @Retention -->
        * RetentionPolicy.SOURCE    --> source                      level
        * RetentionPolicy.CLASS     --> source + compiler           level
        * RetentionPolicy.RUNTIME   --> source + compiler + runtime level


    @Documented

    @Target
        ElementType.ANNOTATION_TYPE
        ElementType.CONSTRUCTOR
        ElementType.FIELD
        ElementType.LOCAL_VARIABLE
        ElementType.METHOD
        ElementType.PACKAGE
        ElementType.PARAMETER
        ElementType.TYPE

    @Inherited

    @Repetable

Java SE 8 --> annotation can be applied to the use of types.


Checker framework ( University of Washington )
----------------------------------------------


---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/
