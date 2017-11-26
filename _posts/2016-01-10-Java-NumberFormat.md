---
layout: post
title:  "Java - NumberFormat"
date:   2016-01-10 14:30:16 +0900
categories: Java, NumberFormat, Currency
---

## total 10 chars 

```java
    // ABC1 => ABC0000001 ( total 10 chars)

    for(int i=1; i<11; i++){ 
        // % - start of format 
        // 0 - 0-pad the result 
        // 7 - set result width to 7 characters wide 
        // d - display as decimal integer 
        String id = String.format("ABC%07d", i); 
        System.out.println(id); 
    } 

    Ans:

    ABC0000001
    ABC0000002
    ABC0000003
    ABC0000004
    ABC0000005
    ABC0000006
    ABC0000007
    ABC0000008
    ABC0000009
    ABC0000010
```

## DecimalSeparator , GroupingSeparator

```java

    // 83000 => $83.000
    
    DecimalFormatSymbols symbols = new DecimalFormatSymbols(new Locale("en", "US")); 
    symbols.setDecimalSeparator(','); 
    symbols.setGroupingSeparator('.'); 
    DecimalFormat df = new DecimalFormat("$#,##0", symbols); 
    System.out.println(df.format(83000L)); //$83.000 

```

## rounding, NO rounding technique

```java
    String str = String.format("%.2f", 3.99999);
    
    ==> 4.00
    
    
    // NO rounding : for two digits => x - 0.005
    String str = String.format("%.2f", 3.99999 - 0.005);
    ==> 3.99 ( no rounding )

    // TODO : why NO rounding ???

    final DecimalFormatSymbols syms = new DecimalFormatSymbols(); 
    syms.setDecimalSeparator(','); 
    syms.setGroupingSeparator('.'); 
    DecimalFormat myFormatter = new DecimalFormat("###,###.00", syms); 
    System.out.println(myFormatter.format(1234.126)); => 1.234,13
    System.out.println(myFormatter.format(1234.125)); => 1.234,12 (? why no rounding)
```		 

## more rounding

```java
    // 1 -> 1.00 1.1 -> 1.10 1.12 -> 1.12
    // 1.123 -> 1.123 1.1235 -> 1.124

    final DecimalFormat df = new DecimalFormat("#,##0.00#"); 
    // 0 means if digit avl/NOt avl, display	
    // # means if digit avl only display ( note rounding also happens )

    // df.setRoundingMode(RoundingMode.FLOOR); // disable rounding

    // use of RoundingMode.UNNECESSARY to find fractions ( intelligent debugging )
    // since it throws ArithmeticException

    double[] d1s = { 1, 1.1, 1.12 };

    for (double d : d1s) {
        System.out.println(df.format(d));
    }

    d1s = new double[] { 1.123, 1.1235 };

    for (double d : d1s) {
        System.out.println(df.format(d));
    }


        output:
        ------
        1.00
        1.10
        1.12
        1.123
        1.124
```

## \u00a5 => Japanese yen 

```java
    pattern 		value
    ("###,###.###", 123456.789);
    ("###.##", 123456.786);
    ("000000.000", 123.78);
    ("$###,###.###", 12345.67);
    ("\u00a5###,###.###", 12345.67);

    final DecimalFormat df = new DecimalFormat();
    df.applyPattern(pattern);
    
    df.format(value) => output
    
    => value       pattern      output
    123456.789  ###,###.###  123,456.789
    123456.786  ###.##  123456.79
    123.78  000000.000  000123.780
    12345.67  $###,###.###  $12,345.67
    12345.67  \###,###.###  \12,345.67
                
```


## 	using locale:

```java					 
		
    NumberFormat nf = NumberFormat.getNumberInstance(loc);
    DecimalFormat df = (DecimalFormat)nf;
    df.applyPattern(pattern);
    
    
    loc could be any one of the following
    
        new Locale("en", "US") 
        new Locale("de", "DE")
        new Locale("fr", "FR")
```

## display currency

```java	         

    static public void displayCurrency( Locale currentLocale) {

        Double currencyAmount = new Double(9876543.21);
        Currency currentCurrency = Currency.getInstance(currentLocale);
        NumberFormat currencyFormatter = 
            NumberFormat.getCurrencyInstance(currentLocale);

        System.out.println(
            currentLocale.getDisplayName() + ", " +
            currentCurrency.getDisplayName() + ": " +
            currencyFormatter.format(currencyAmount));
    }


    // The output generated by the preceding lines of code is as follows:

    French (France), Euro: 9 876 543,21 
    German (Germany), Euro: 9.876.543,21 
    English (United States), US Dollar: $9,876,543.21
```

## Locale ( set lang and region)

```java 
	Locale enGBLocale =  new Locale.Builder().setLanguage("en").setRegion("GB").build();
``` 

-------------
//TODO
https://docs.oracle.com/javase/tutorial/i18n/format/dateintro.html

	DateTime ??? (already taken notes)
	
## MessageFormat

```java

    // following also ok, as long as ds_ab_CD.properties file is present in the classpath.

    ResourceBundle rb = ResourceBundle.getBundle("ds", new Locale("ab", "CD")); 

    // properties file.
    /*
    ds_ab_CD.properties

    template = At {2,time,short} on {2,date,long}, \
        we detected {1,number,integer} spaceships on \
        the planet {0}.
    planet = Mars

    */

    // code

    rb.getString("planet") => "Mars"

    {2,time,short} => time portion in Date. 
                    short => DateFormat.SHORT 
                    
                    
    MessageFormatter mf = new MessageFormatter("");
    mf.setLocale(loc);

    mf.applyPattern(rb.getString("template"));
    String output = mf.format(messageArguments);

    Object[] messageArguments = {
        messages.getString("planet"),
        new Integer(7),
        new Date()
    };

```

## handling plurals in message.

```java

    Choice format:(handling plurals)

    ResourceBundle bundle = ResourceBundle.getBundle(
        "ChoiceBundle", currentLocale);

    /*
    ChoiceBundle_en_US.properties
    -----------------------------
    pattern = There {0} on {1}.
    noFiles = are no files
    oneFile = is one file
    multipleFiles = are {2} files

    ChoiceBundle_fr_FR.properties
    -----------------------------
    pattern = Il {0} sur {1}.
    noFiles = n'y a pas de fichiers
    oneFile = y a un fichier
    multipleFiles = y a {2} fichiers
    */

    MessageFormat messageForm = new MessageFormat("");
    messageForm.setLocale(currentLocale);


    double[] fileLimits = {0,1,2};
    String [] fileStrings = {
        bundle.getString("noFiles"),
        bundle.getString("oneFile"),
        bundle.getString("multipleFiles")
    };


    ChoiceFormat choiceForm = new ChoiceFormat(fileLimits, fileStrings);
    String pattern = bundle.getString("pattern");
    messageForm.applyPattern(pattern);

    Format[] formats = {choiceForm, null, NumberFormat.getInstance()};
    messageForm.setFormats(formats);

    // test

    Object[] messageArguments = {null, "XDisk", null};

    for (int numFiles = 0; numFiles < 4; numFiles++) {
        messageArguments[0] = new Integer(numFiles);
        messageArguments[2] = new Integer(numFiles);
        String result = messageForm.format(messageArguments);
        System.out.println(result);
    }

    // output

    currentLocale = en_US
    There are no files on XDisk.
    There is one file on XDisk.
    There are 2 files on XDisk.
    There are 3 files on XDisk.

```

## suppress negative sign ( by having space after semicolon)

```java
    DecimalFormat df = new DecimalFormat();
    df.applyPattern("##,###.##; "); // notice the space after semicolon
    System.out.println(df.format(-32121.21)); //=> " 32,121.21"
```

## Percentage 

```java
    NumberFormat f = NumberFormat.getPercentInstance(); 
    f.setMinimumFractionDigits(3); 
    System.out.println(f.format(0.045317d)); //=> 4.532%
```
 
## 	identify the pattern() used in DecimalFormat instance

```java	
	df.toLocalizedPattern()
	df.toPattern()
```

## Formatter input is "StringBuilder" sample

```java
    StringBuilder sbuf = new StringBuilder();
    Formatter fmt = new Formatter(sbuf);
    fmt.format("%s%n", "Log file debug info started");
//		System.out.print(sbuf.toString());
    sbuf.append("323123\n");
    //...
    sbuf.append("45");
    System.out.println(sbuf.toString());
    
    output:
    ------
    
    Log file debug info started
    323123
    45
```

## String.format

```java 
    String str = String.format("%2$s my age is %1$d", 32, "Hello"); 
    System.out.println(str); // -> Hello my age is 32

    String.format("%d", 93); // prints 93

    String.format("|%20d|", 93); // prints: |                  93|

    String.format("|%-20d|", 93); // prints: |93                  |

    String.format("|%020d|", 93); // prints: |00000000000000000093|

    String.format("|%+20d|", 93); // prints: |                 +93|

    //A space before positive numbers.
    //A “-” is included for negative numbers as per normal.
    String.format("|% d|", 93); // prints: | 93| 
    String.format("|% d|", -36); // prints: |-36|

    //Use locale-specific thousands separator:
    //For the US locale, it is “,”:
    String.format("|%,d|", 10000000); // prints: |10,000,000|

    //Enclose negative numbers within parentheses (“()”) and skip the "-":
    String.format("|%(d|", -36); // prints: |(36)|

    // hex
    String.format("|%x|", 93); // prints: |5d|

    // oct
    String.format("|%o|", 93); // prints: |135|

    String.format("|%#o|", 93);
    // prints: 0135

    String.format("|%#x|", 93);
    // prints: 0x5d

    String.format("|%#X|", 93);
    // prints: 0X5D

    //Prints the whole string.
    System.out.println(String.format("|%s|", "Hello World")); // prints: |Hello World|

    //Specify Field Length
    System.out.println(String.format("|%30s|", "Hello World")); // prints: |                   Hello World|

    // Left Justify Text
    System.out.println(String.format("|%-30s|", "Hello World")); // prints: |Hello World                   |

    //Specify Maximum Number of Characters
    System.out.println(String.format("|%.5s|", "Hello World")); // prints: |Hello|

    //Field Width and Maximum Number of Characters
    System.out.println(String.format("|%30.5s|", "Hello World"));// |                         Hello|
```

## NumberFormat vs Regex ( test for numbers). regex wins!

```java
	NumberFormat nf = NumberFormat.getNumberInstance(); 
	nf.setParseIntegerOnly(true); 

	Number numberA = nf.parse("99.731"); 
	// 99 (not what the user expect) 

	Number numberB = nf.parse("99s.231");
	 // 99 (invalid) 

	Number numberC = nf.parse("9g9"); 
	// 9 (invalid) 
	System.out.println(numberA.toString()); 
	System.out.println(numberB.toString()); 
	System.out.println(numberC.toString()); 

    //regex:
    //------
	int num = Integer.parseInt(str); // ok but conversion happens
	long num = Long.parseLong(str); // ok but conversion happens, and as well unnecessary exception
								   // catching RuntimeException is not desired behavior
	boolean ok = Pattern.compile("-?\\d+").matches(str); 
```
##	digits with space formating

```java
    Double[] numbers = new Double[] {1.1233, 12.4231414, 0.123, 123.3444, 1.1}; 
    for (Double number : numbers) { 
        System.out.println(String.format("%4.3f", number)); 
        System.out.println(String.format("%8.3f", number));
    } 
    
    //output
    //-----
    1.123
        1.123
    12.423
        12.423
    0.123
        0.123
    123.344
        123.344
    1.100
        1.100
```

## to show decimal separator always
```java
	df.setDecimalSeparatorAlwaysShown(true); 
```	

## show only 5 digits (total)
```java
    public class SignificantFormat {
        public static String formatSignificant(double value, int significant) {
            MathContext mathContext = new MathContext(significant,
                    RoundingMode.DOWN);
            BigDecimal bigDecimal = new BigDecimal(value, mathContext);
            return bigDecimal.toPlainString();
        }

        public static void main(String[] args) {
            double[] data = { 1, 100, 100000, 99999, 99999.99, 9999.99, 999.99,
                    23.34324 };
            for (double d : data) {
                System.out.printf("Input: %10s \tOutput: %10s\n",
                        Double.toString(d), formatSignificant(d, 5));
            }
        }
    }

    //output

    Input:        1.0 	Output:          1
    Input:      100.0 	Output:        100
    Input:   100000.0 	Output:     100000
    Input:    99999.0 	Output:      99999
    Input:   99999.99 	Output:      99999
    Input:    9999.99 	Output:     9999.9
    Input:     999.99 	Output:     999.99
    Input:   23.34324 	Output:     23.343
    ---
```

## Convert string to decimal number with 2 decimal places in Java 

```java
    DecimalFormat df = new DecimalFormat("0.00");
    System.out.println(df.format(1221222223.005))
```

## split it out ( chars and digits)
``` java
    String str = "abcd1234"; 
    String[] part = str.split("(?<=\\D)(?=\\d)");
    System.out.println(part[0]); //abcd
    System.out.println(part[1]); //1234
``` 
 
## non breaking space \u00a0 replace
```java
    input = input.replace('\u00a0', '.'); 
```
 
## rounding off technique
```java 
    double value = 1302340.32389782323;
    double value1 = Math.round(value * 1e5) / 1e5;
    System.out.println(value1);

    double value2 = Math.floor(value * 1e5) / 1e5;

    System.out.println(value2);
    
    //output
    //---
    1302340.3239
    1302340.32389
```

## jdk bugs
// TODO
	Java 7 Double.toString () returns 0.005 / java 6 it is 0.0050 
	
	look for minor bugs fixed in jdk 7 ( write blog )

	
https://translate.googleusercontent.com/translate_c?act=url&depth=1&hl=ja&ie=UTF8&prev=_t&rurl=translate.google.co.in&sl=ja&sp=nmt4&tl=en&u=https://stackoverflow.com/questions/tagged/number-formatting%2Bjava%3Fpage%3D2%26sort%3Dfrequent%26pagesize%3D15&usg=ALkJrhiht6waTLqlkoE4htjm2ost5m3TkQ
	





---------------------------



[jekyll-docs]: https://jekyllrb.com/docs/home
[jekyll-gh]:   https://github.com/jekyll/jekyll
[jekyll-talk]: https://talk.jekyllrb.com/