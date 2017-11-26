---
layout: post
title:  "Effective Java - Part 1!"
date:   2016-03-13 9:30:46 +0900
categories: Effective Java, Part1
---

# Creating and Destroying Objects

## Item 1: Consider static factory methods instead of constructors

// static factory method #(NE) Factory method pattern  
// no equivalent design pattern ( it is not a design pattern)  
```java
public static Boolean valueOf(boolean b) {
    return b ? Boolean.TRUE : Boolean.FALSE;
}
```

// Advantages:
1. Name of method ( compared to constructors)  
2. Not required to create New object (Flyweight pattern)  
           == operator instead of the equals(Object) method, which may  
            result in improved performance  
3. return an object of any subtype (unlike constructors).  
            ex 1: Collections.static methods  
            ex 2: EnumSet  
                    64 or fewer ( RegularEnumSet)  
                    65 or more (JumboEnumSet)  
4. reduce verbosity ( by type inference)  

```java
    Map<String, List<String>> m =
        new HashMap<String, List<String>>();

    public static <K, V> HashMap<K, V> newInstance() {
        return new HashMap<K, V>();
    }

    Map<String, List<String>> m = HashMap.newInstance();

    // java 7 :
        // diamond operator
        Map<String, List<String>> m = new HashMap<>();

```

// Disadvantages:  
1. Without public / protected constructors, classes can NOT be subclassed.
Blessing in disguise : enforce composition instead of inheritance.  
2. naming convention required.
    * valueOf
    * of (concise alternative of valueOf, used in EnumSet)
    * getInstance
    * newInstance
    * getType
    * newType

---

## Item 2: Consider a builder when faced with many constructor parameters

* avoid telescoping constructor pattern
* use **builder** pattern

```java
package effjava;

//@formatter:off
// Sample Usage : java SampleBuilderPattern 2 3 
//@formatter:on
public class SampleBuilderPattern {

	public static void main(String[] args) {
		if (args.length < 2) {
			throw new IllegalArgumentException(
					"required and optional parameters missing. Sample Usage: java SampleBuilderPattern 2 3 ");
		}

		final int REQ_COUNT = Integer.valueOf(args[0]);
		final int OPT_COUNT = Integer.valueOf(args[1]);

		int tempReqCount = REQ_COUNT;
		int tempOptCount = OPT_COUNT;
//@formatter:off
// public class SampleBuilderBean { 
//@formatter:on
		System.out.println("public class SampleBuilderBean {");
		System.out.println();

//@formatter:off
//	    // Required
//	    private final reqType1 reqProperty1;
//	    private final reqType2 reqProperty2;
//
//	    // Optional
//	    private final opType1 opProperty1;
//	    private final opType2 opProperty2;
//	    private final opType3 opProperty3;
//	    
//@formatter:on
		System.out.println("    // Required");
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("    private final reqType" + i + " reqProperty"
					+ i + ";");
		}
		System.out.println();
		System.out.println("    // Optional");
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("    private final opType" + i + " opProperty"
					+ i + ";");
		}
//@formatter:off
// public static class Builder {
//@formatter:on
		System.out.println();
		System.out.println("    public static class Builder {");
		System.out.println();

//@formatter:off
//        // Required parameters
//        private final reqType1 reqProperty1;
//        private final reqType2 reqProperty2;
//
//        // Optional parameters - initialized to default values
//        // TODO : initialize to default
//        private opType1 opProperty1 = ;
//        private opType2 opProperty2 = ;
//        private opType3 opProperty3 = ;
//   
//@formatter:on
		tempReqCount = REQ_COUNT;
		tempOptCount = OPT_COUNT;
		System.out.println("        // Required parameters");
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("        private final reqType" + i
					+ " reqProperty" + i + ";");
		}
		System.out.println();
		System.out
				.println("        // Optional parameters - initialized to default values");
		System.out.println("        // TODO : initialize to default");
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("        private opType" + i + " opProperty" + i
					+ " = " + ";");
		}
		System.out.println();

//@formatter:off
// public Builder(reqType1 reqProperty1, reqType2 reqProperty2) {
//@formatter:on

		System.out.print("        public Builder(");
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.print("reqType" + i + " reqProperty" + i);
			if (i < tempReqCount) {
				System.out.print(", ");
			}
		}
		System.out.println(") {");
//@formatter:off
//    this.reqProperty1 = reqProperty1;
//    this.reqProperty2 = reqProperty2;
//  }
//@formatter:on
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("            this.reqProperty" + i + " = "
					+ "reqProperty" + i + ";");
		}
		System.out.println("        }");
//@formatter:off
//    public Builder opProperty1(opType1 val) {
//        opProperty1 = val;
//        return this;
//    }
//    public Builder opProperty2(opType2 val) {
//        opProperty2 = val;
//        return this;
//    }
//    public Builder opProperty3(opType3 val) {
//        opProperty3 = val;
//        return this;
//    }
//@formatter:on
		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("        public Builder opProperty" + i
					+ "(opType" + i + " val) {");
			System.out.println("            " + "opProperty" + i + " = "
					+ "val;");
			System.out.println("            " + "return this;");
			System.out.println("        }");
		}
//@formatter:off
//    public SampleBuilderBean build() {
//        return new SampleBuilderBean(this);
//    }
// }		
//@formatter:on
		System.out.println("        public SampleBuilderBean build() {");
		System.out.println("            return new SampleBuilderBean(this);");
		System.out.println("        }");
		System.out.println("    }");

		System.out.println();
//@formatter:off
//    private SampleBuilderBean(Builder builder) {
//        reqProperty1 = builder.reqProperty1;
//        reqProperty2 = builder.reqProperty2;
//        opProperty1 = builder.opProperty1;
//        opProperty2 = builder.opProperty2;
//        opProperty3 = builder.opProperty3;
//    }
//@formatter:on
		System.out.println("    private SampleBuilderBean(Builder builder) {");
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("        reqProperty" + i + " = "
					+ "builder.reqProperty" + i + ";");
		}

		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("        opProperty" + i + " = "
					+ "builder.opProperty" + i + ";");
		}
		System.out.println("    }");
//@formatter:off
//    public reqType1 reqProperty1() {
//        return reqProperty1;
//    }
//@formatter:on
		System.out.println();
		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.println("    public reqType" + i + " reqProperty" + i
					+ "() {");
			System.out.println("        return reqProperty" + i + ";");
			System.out.println("    }");
		}
		System.out.println();
		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out.println("    public opType" + i + " opProperty" + i
					+ "() {");
			System.out.println("        return opProperty" + i + ";");
			System.out.println("    }");
		}

//@formatter:off
//	    public static void main(String[] args) {
//	        final SampleBuilderBean sbb = new SampleBuilderBean.Builder(reqactval1, reqactval2)
//	                                                           .opProperty1(opVal1)
//	                                                           .opProperty2(opVal2)
//	                                                           .opProperty3(opVal3)
//	                                                           .build();
//	    }
//@formatter:on
		System.out.println("    public static void main(String[] args) {");
		System.out
				.print("        final SampleBuilderBean sbb = new SampleBuilderBean.Builder(");

		tempReqCount = REQ_COUNT;
		for (int i = 1; i <= tempReqCount; i++) {
			System.out.print("reqactval" + i);
			if (i < tempReqCount) {
				System.out.print(", ");
			}
		}
		System.out.println(")");

		tempOptCount = OPT_COUNT;
		for (int i = 1; i <= tempOptCount; i++) {
			System.out
					.println("                                                           .opProperty"
							+ i + "(opVal" + i + ")");
		}
		System.out
				.println("                                                           .build();");

		System.out.println("    }");

//@formatter:off
//	}
//@formatter:on
		System.out.println("}");
	}

//@formatter:off
/* Sample Output
public class SampleBuilderBean {

    // Required
    private final reqType1 reqProperty1;
    private final reqType2 reqProperty2;

    // Optional
    private final opType1 opProperty1;
    private final opType2 opProperty2;
    private final opType3 opProperty3;

    public static class Builder {

        // Required parameters
        private final reqType1 reqProperty1;
        private final reqType2 reqProperty2;

        // Optional parameters - initialized to default values
        // TODO : initialize to default
        private opType1 opProperty1 = ;
        private opType2 opProperty2 = ;
        private opType3 opProperty3 = ;

        public Builder(reqType1 reqProperty1, reqType2 reqProperty2) {
            this.reqProperty1 = reqProperty1;
            this.reqProperty2 = reqProperty2;
        }
        public Builder opProperty1(opType1 val) {
            opProperty1 = val;
            return this;
        }
        public Builder opProperty2(opType2 val) {
            opProperty2 = val;
            return this;
        }
        public Builder opProperty3(opType3 val) {
            opProperty3 = val;
            return this;
        }
        public SampleBuilderBean build() {
            return new SampleBuilderBean(this);
        }
    }

    private SampleBuilderBean(Builder builder) {
        reqProperty1 = builder.reqProperty1;
        reqProperty2 = builder.reqProperty2;
        opProperty1 = builder.opProperty1;
        opProperty2 = builder.opProperty2;
        opProperty3 = builder.opProperty3;
    }

    public reqType1 reqProperty1() {
        return reqProperty1;
    }
    public reqType2 reqProperty2() {
        return reqProperty2;
    }

    public opType1 opProperty1() {
        return opProperty1;
    }
    public opType2 opProperty2() {
        return opProperty2;
    }
    public opType3 opProperty3() {
        return opProperty3;
    }
    public static void main(String[] args) {
        final SampleBuilderBean sbb = new SampleBuilderBean.Builder(reqactval1, reqactval2)
                                                           .opProperty1(opVal1)
                                                           .opProperty2(opVal2)
                                                           .opProperty3(opVal3)
                                                           .build();
    }
}
 */
//@formatter:on	

}


```

---

## Item 3: Enforce the singleton property with a private constructor or an enum type

**Singleton with public final field**
```java

// Serializable Singleton
public class Singleton implements Serializable {
    private static final Singleton INSTANCE = new Singleton();
    private Singleton() { 
        //... 
        // TODO throw exception, on second instance creation
        // when using AccessibleObject.setAccssible
    }
    public static Singleton getInstance() { return INSTANCE; }

    public void leaveTheBuilding() { 
        //... 
    }
    
    // readResolve method to preserve singleton property
    private Object readResolve() {
        // Return the one true Elvis and let the garbage collector
        // take care of the Elvis impersonator.
        return INSTANCE;
    }

}

// TODO implement AccessibleObject.setAccessible
```

### Enum singleton - the preferred approach  

1. guarantee against multiple instantiation, 
2. even in the face of sophisticated serialization or 
3. reflection attacks

```java
// java 1.5 using enum for creating Singleton
public enum Singleton {
    INSTANCE;
    public void leaveTheBuilding() { ... }
}

```

---

## Item 4: Enforce non-instantiability with private constructor  

### utility class

**examples**

* java.lang.Math
* java.util.Arrays
* java.util.Collections

1. Make it abstract ...hmm no: since it can be subclassed and can be instantiated.
2. Make noninstantiable by including a private constructor

```java
// Noninstantiable utility class
public class UtilityClass {
    // Suppress default constructor for noninstantiability
    private UtilityClass() {
        throw new AssertionError();
    }
    // Remainder omitted
}
```

---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/