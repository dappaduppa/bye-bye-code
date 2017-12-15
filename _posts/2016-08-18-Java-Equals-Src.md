---
layout: post
title:  "Java Equals Check"
date:   2016-08-18 10:45:46 +0900
categories: Java, Equals
---

```java
-----

ch11 item 74

NonSerializableSuperClass dir

Foo.java
----
package ds;

import java.io.IOException;
import java.io.ObjectInputStream;
import java.io.ObjectOutputStream;
import java.io.Serializable;

public class Foo extends AbstractFoo implements Serializable {
	private void readObject(ObjectInputStream s) throws IOException,
			ClassNotFoundException {
		s.defaultReadObject();
		// Manually deserialize and initialize superclass state
		int x = s.readInt();
		int y = s.readInt();
		initialize(x, y);
	}

	private void writeObject(ObjectOutputStream s) throws IOException {
		s.defaultWriteObject();
		// Manually serialize superclass state
		s.writeInt(getX());
		s.writeInt(getY());
	}

	// Constructor does not use the fancy mechanism
	public Foo(int x, int y) {
		super(x, y);
	}

	private static final long serialVersionUID = 1856835860954L;
}

AbstractFoo.java

package ds;

import java.util.concurrent.atomic.AtomicReference;

//Nonserializable stateful class allowing serializable subclass
public abstract class AbstractFoo {
	private int x, y; // Our state

	// This enum and field are used to track initialization

	private enum State {
		NEW, INITIALIZING, INITIALIZED
	};

	private final AtomicReference<State> init = new AtomicReference<State>(
			State.NEW);

	public AbstractFoo(int x, int y) {
		initialize(x, y);
	}

	// This constructor and the following method allow
	// subclass's readObject method to initialize our state.
	protected AbstractFoo() {
	}

	protected final void initialize(int x, int y) {
		if (!init.compareAndSet(State.NEW, State.INITIALIZING))
			throw new IllegalStateException("Already initialized");
		this.x = x;
		this.y = y;
		// Do anything else the original constructor did
		init.set(State.INITIALIZED);
	}

	// These methods provide access to internal state so it can
	// be manually serialized by subclass's writeObject method.
	protected final int getX() {
		checkInit();
		return x;
	}

	protected final int getY() {
		checkInit();
		return y;
	}

	// Must call from all public and protected instance methods
	private void checkInit() {
		if (init.get() != State.INITIALIZED)
			throw new IllegalStateException("Uninitialized");
	}
	// Remainder omitted
}

InvalidClassException dir
-------------------------
readme_Serialize_DeSerialize.txt
---------


Steps to reproduce InvalidClassException

1) Dont define the serialVersionUID

2) Serialize the object

3) modify the original class ( add one more field )

4) deserialize the object ,
  you get InvalidClassException since the serialVersionUID is automatically created.

5) to overcome, from the exception note down the original serialVersionUID, and define
  the same in original class.



DeSerializeSampleBean.java
----------------
package ds;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import java.io.ObjectInputStream;

public class DeSerializeSampleBean {

	public static void main(String[] args) throws ClassNotFoundException, IOException {
		final File file = new File("ssb.out");
		final FileInputStream fis = new FileInputStream(file);
		final ObjectInputStream ois = new ObjectInputStream(fis);
		final SampleBean sb = (SampleBean)ois.readObject();
		ois.close();
		System.out.println(sb.getName());
	}

}


SampleBean.java
-----

package ds;

import java.io.Serializable;

public class SampleBean implements Serializable {
	
　// initially dont define.	
	private static final long serialVersionUID = -16810750484858286L;

	private String name;
   
   // initially don't define.
	private String id;
	
		
	public final String getName() {
		return name;
	}

	public final void setName(String name) {
		this.name = name;
	}


   // initially don't define.
	public final String getId() {
		return id;
	}

   // initially don't define.
	public final void setId(String id) {
		this.id = id;
	}

	
	
	
}

SerializeSampleBean.java
---------------

package ds;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;
import java.io.ObjectOutputStream;

public class SerializeSampleBean {

	public static void main(String[] args) throws IOException {

		final SampleBean sb = new SampleBean();
		sb.setName("mandaya");

		final File file = new File("ssb.out");
		final FileOutputStream fos = new FileOutputStream(file);
		final ObjectOutputStream oos = new ObjectOutputStream(fos);
		oos.writeObject(sb);
		oos.close();
		fos.close();
	}

}

---------------------------------------------------------
Equals
---------------------------------------------------------

import org.junit.Test;


public abstract class AbstractEqualsTest {
	
	protected static IEquals iEquals;
	
	@Test
	public void equalsTest() {
		iEquals.testAll();
	}
}
import org.junit.Test;


public abstract class AbstractNotEqualsTest {
	protected static INotEquals iNotEquals;
	
	@Test
	public void notEqualsTest() {
		iNotEquals.testAll();
	}
}

----------------
EqualsImpl
----------------
import org.junit.Assert;


public class EqualsImpl<X extends Object, Y extends Object, Z extends Object> implements IEquals {

	private X x;
	private Y y;
	private Z z;
	
	
	public EqualsImpl(X x, Y y, Z z) {
		this.x = x;
		this.y = y;
		this.z = z;
		
	}
	
	public void testAll() {
		reflexive();
		symmetric();
		transitive();
		consistent();
		nonull();
	}
	
	// For any non-null reference value x, x.equals(x) must return true
	public void reflexive() {
		Assert.assertTrue(x.equals(x));
	}

	// For any non-null reference values x and y, x.equals(y) must return true
	// if and only if y.equals(x) returns true
	public void symmetric() {
		Assert.assertTrue(x.equals(y) && y.equals(x));
	}

	// For any non-null reference values x, y, z, if x.equals(y) returns true
	// and y.equals(z) returns true, then x.equals(z) must return true.
	public void transitive() {
		Assert.assertTrue(x.equals(y) && y.equals(z) && x.equals(z));
	}

	// For any non-null reference values x and y, multiple invocations of
	// x.equals(y) consistently return true or consistently return false,
	// provided no information used in equals comparisons on the objects is
	// modified.
	@SuppressWarnings("unused")
	public void consistent() {
		for (int i : new int[] { 1, 2, 3 }) {
			Assert.assertTrue(x.equals(y));
		}
	}

	// For any non-null reference value x, x.equals(null) must return false.
	public void nonull() {
		Assert.assertFalse(x.equals(null));
	}

}

----------
IEquals
----------


public interface IEquals extends IEqualsNotEqualsCommon {

}


-------------
IEqualsNotEqualsCommon
-------------
public interface IEqualsNotEqualsCommon {
	// For any non-null reference value x, x.equals(x) must return true
	public void reflexive();

	// For any non-null reference values x and y, x.equals(y) must return true
	// if and only if y.equals(x) returns true
	public void symmetric();

	// For any non-null reference values x, y, z, if x.equals(y) returns true
	// and y.equals(z) returns true, then x.equals(z) must return true.
	public void transitive();

	// For any non-null reference values x and y, multiple invocations of
	// x.equals(y) consistently return true or consistently return false,
	// provided no information used in equals comparisons on the objects is
	// modified.
	public void consistent();

	// For any non-null reference value x, x.equals(null) must return false.
	public void nonull();

	public void testAll();
}


---------
INotEquals
--------

public interface INotEquals extends IEqualsNotEqualsCommon {

}


---------
NotEqualsImpl
---------

import org.junit.Assert;
import org.junit.Test;

public class NotEqualsImpl<X extends Object, Y extends Object, Z extends Object>
		implements INotEquals {

	private X x;
	private Y y;
	private Z z;

	public NotEqualsImpl(X x, Y y, Z z) {
		this.x = x;
		this.y = y;
		this.z = z;

	}

	@Test
	public void testAll() {
		reflexive();
		symmetric();
		transitive();
		consistent();
		nonull();
	}

	// For any non-null reference value x, x.equals(x) must return true
	public void reflexive() {
		Assert.assertTrue(x.equals(x));
	}

	// For any non-null reference values x and y, x.equals(y) must return true
	// if and only if y.equals(x) returns true
	public void symmetric() {
		Assert.assertFalse(x.equals(y) && y.equals(x));
	}

	// For any non-null reference values x, y, z, if x.equals(y) returns true
	// and y.equals(z) returns true, then x.equals(z) must return true.
	public void transitive() {
		Assert.assertFalse(x.equals(y) && y.equals(z) && x.equals(z));
	}

	// For any non-null reference values x and y, multiple invocations of
	// x.equals(y) consistently return true or consistently return false,
	// provided no information used in equals comparisons on the objects is
	// modified.
	@SuppressWarnings("unused")
	public void consistent() {
		for (int i : new int[] { 1, 2, 3 }) {
			Assert.assertFalse(x.equals(y));
		}
	}

	// For any non-null reference value x, x.equals(null) must return false.
	public void nonull() {
		Assert.assertFalse(x.equals(null));
	}

}

--------
PersonEqualsTest
---------

import org.junit.BeforeClass;

public class PersonEqualsTest extends AbstractEqualsTest {

	private static Person x;
	private static Person y;
	private static Person z;
	

	@BeforeClass
	public static void createInstance() {
		x = new Person("ds");
		y = new Person("ds");
		z = new Person("ds");
		iEquals = new EqualsImpl<>(x, y, z);
	}

}

-------------
PersonNOTEqualsTest
-------------

import org.junit.BeforeClass;

public class PersonNOTEqualsTest extends AbstractNotEqualsTest {

	private static Person x;
	private static Person y;
	private static Person z;

	@BeforeClass
	public static void createInstance() {
		x = new Person("ab");
		y = new Person("cd");
		z = new Person("ef");
		iNotEquals = new NotEqualsImpl<>(x, y, z);

	}

}
```
--------------------
