---
layout: post
title:  "Java IO File"
date:   2016-09-08 08:36:46 +0900
categories: Java IO File 
---

## filereading:

```java
// BufferedReader 
BufferedReader input =  new BufferedReader(new FileReader(aFile));
//String line 
while (( line = input.readLine()) != null){
    //...
}

// Scanner
Scanner scanner = new Scanner(new FileInputStream(fFileName), fEncoding);
while (scanner.hasNextLine()){
    scanner.nextLine() 
}

// java 7
// larger
fFilePath = Paths.get(aFileName);
try (Scanner scanner =  new Scanner(fFilePath, ENCODING.name())){
    while (scanner.hasNextLine()){
        processLine(scanner.nextLine());
    }      
}

// larger (BufferedReader)
Path path = Paths.get(aFileName);
try (BufferedReader reader = Files.newBufferedReader(path, ENCODING)){
    String line = null;
    while ((line = reader.readLine()) != null) {

// smaller (readAllLines at once)
Path path = Paths.get(aFileName);
return Files.readAllLines(path, ENCODING);

//use buffering
Writer output = new BufferedWriter(new FileWriter(aFile));
try {
    //FileWriter always assumes default encoding is OK!
    output.write( aContents ); //String
}
Writer out = new OutputStreamWriter(new FileOutputStream(fFileName), fEncoding);
try {
    out.write(aContents );
}
finally {
    out.close();
}
---
Path path = Paths.get(aFileName);
    Files.write(path, aLines, ENCODING);
 /// large
Path path = Paths.get(aFileName);
    try (BufferedWriter writer = Files.newBufferedWriter(path, ENCODING)){
      for(String line : aLines){
        writer.write(line);
        writer.newLine();
      }
    }

```