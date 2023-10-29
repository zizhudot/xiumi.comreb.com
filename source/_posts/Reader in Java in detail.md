---
title: Reader in Java in detail
description: Reader in Java in detail
tags:
  - IaaS
  - cloud
  - cloud computing
date: 2023-10-29 19:50:26
---


## Preface




In the process of Java development, we often need to read the data in the file, and the reading of data requires a suitable class for processing.Java's IO package provides a number of classes for reading and writing data, of which Reader is one of them. In this article, we will introduce Reader in Java in detail, and analyze its advantages and disadvantages and application scenarios.




## Abstract




In this article, we will introduce the `Reader` class in Java in detail from the following aspects:




1. Overview of the Reader class
2. Reader class code analysis
3. Application examples of the Reader class
4. Advantages and disadvantages of the Reader class
5. Introduction to the methods and source code analysis of the Reader class.
6. Test Cases of Reader Class
7. Summary and conclusion of the whole paper
8. Attached source code
9. Recommendations




This article explains in detail the Reader in Java, aims to help developers better grasp the use of Reader.




## Reader class




## Overview




The Reader class is an abstract class for reading character streams in Java. It is the superclass of all character input streams and provides basic functionality when reading character input streams.The Reader class is implemented by three main classes, InputStreamReader, FileReader and CharArrayReader.




## Source code analysis




The `Reader` class is an abstract class whose source code is defined as follows:




public abstract class Reader implements Readable, Closeable {
    ...
}




Among them, Reader implements two interfaces: `Readable` and `Closeable`. There is only one method defined in the `Readable` interface:




public interface Readable {
    int read(CharBuffer cb) throws IOException;
}




And there is only one method defined in the `Closeable` interface as well:




public interface Closeable extends AutoCloseable {
    void close() throws IOException; }
}




The purpose of these two interfaces is to provide methods for reading characters and closing resources, respectively.




## Application Scenario Examples




The Reader class is usually used to read data from a text file. For example, we often use BufferedReader is a subclass of Reader class, which is used to read the data in a text file line by line. In addition, Reader can also be used to read network data, read console input and other scenarios.




The following are a few examples of application scenarios using the Reader class, for students' reference only:




### 1\. Reading Text Files




It is very common to use the FileReader class to read text files. For example, you can use the combination of `FileReader` and `BufferedReader` to read a text file and output it line by line:




//1. Read a text file
    public static void testReadFile(){
        FileReader fileReader; BufferedReader bufferedReader
        BufferedReader bufferedReader.
        bufferedReader; bufferedReader; try {
            fileReader = new FileReader(". /template/fileTest.txt"); bufferedReader = new BufferedReader(".
            bufferedReader = new BufferedReader(fileReader); bufferedReader; try { fileReader = new FileReader(".
            bufferedReader = new BufferedReader(fileReader); String line;
            while ((line = bufferedReader.readLine()) ! String line; while ((line = bufferedReader.readLine()) !
                System.out.println(line); }
            }
            fileReader.close();
            bufferedReader.close(); } catch (IOException e); }
        } catch (IOException e) {
            e.printStackTrace(); } catch (IOException e) { e.printStackTrace(); }
        }
    }




With the above case, our local demo, the result can be seen as follows:




! [](https://static001.geekbang.org/infoq/46/4601c49043ccf9f56359b5c5d0af8232.png)




### 2\. Reading Network Resources




You can use InputStreamReader and URL class to read network resources, for example:




//2. Read network resources
    public static void testReadURL() throws IOException {
        URL url = new URL("https://www.baidu.com/");
        URLConnection conn = url.openConnection();
        InputStream is = conn.getInputStream();
        InputStreamReader isr = new InputStreamReader(is); BufferedReader br = new
        




                String line; while ((line = br.read))
        while ((line = br.readLine()) ! line; while ((line = br.readLine())) !
            System.out.println(line); }
        }




        System.out.println(line); }
        isr.close(); isr.close(); isr.close(); isr.close()
        isr.close(); isr.close(); }
    }




    public static void main(String\[\] args) throws IOException {
        testReadURL(); }
    }




With the above case, our local demo, the result can be seen as follows:




! [](https://static001.geekbang.org/infoq/71/712b250c12cb54311de0704bca22f7f7.png)




### 3\. Reading a String




The StringReader class can be used to convert a string to a stream of characters, for example:




//3. Read String
    public static void testReadStr() throws IOException {
        String str = "Hello, World!!!" ;
        StringReader stringReader = new StringReader(str) ;
        int data; while ((data = stringReader))
        while ((data = stringReader.read()) ! = -1) {
            System.out.print((char) data); }
        }
        stringReader.close();
    }




    public static void main(String\[\] args) throws IOException {
        testReadStr(); }
    }




With the above case, our local demo, the result can be seen as follows:




! [](https://static001.geekbang.org/infoq/74/7468a1631d08ec9fcdb135dedca298f5.png)




Through the introduction and demonstration of the above three common Java application scenarios using the Reader class, you can easily read various types of character stream data by using the subclasses of the Reader class. If you have more cases that are relevant to your life or work, please feel free to share them with us in the comment section, it's better to have fun alone than with others.




## Pros and Cons




### Pros




1. The `Reader` class supports character stream reading and can accurately read data from text files.
2. The `Reader` class handles character encoding automatically, converting encoding methods when reading files. 3.
3. `Reader` class can be realized through the subclasses of different functions , the use of flexible .




### Disadvantages




1. `Reader` class reads data slowly, not suitable for reading binary data. 2.
2. `Reader` class can't access the data in the file randomly, it can only read line by line, and it is less efficient when reading large files. 3.
3. `Reader` class is more cumbersome to use, you need to buffer and other ways to improve reading speed and efficiency.




## Class Code Methods




### Constructor




protected Reader()




The default constructor method for the Reader class.




### Methods




public int read() throws IOException




Usage: Read a single character and return the ASCII code of the character, if it reaches the end of the stream, return -1.




public int read(char\[\] cbuf) throws IOException




Usage: read an array of characters, return the number of characters read.




public int read(char\[\] cbuf, int offset, int length) throws IOException




Usage: read the specified length of the character array, return to read the number of characters.




public long skip(long n) throws IOException




Usage: skip n characters (including spaces), return the number of characters actually skipped.




public boolean ready() throws IOException




Usage: determine whether the characters can be read from the stream, if you can read return true.




public boolean markSupported()




Usage: determine whether the stream supports mark() operation. If support, then return true, otherwise return false.




public void mark(int readAheadLimit) throws IOException




Usage: Set the mark position and point the pointer in the input stream to the mark position. If the stream does not support mark() operation, then throws IOException.




public void reset() throws IOException




Purpose: Redirects the pointer in the input stream to the mark position. If the stream does not support the reset() operation, then an IOException is thrown.




abstract public void close() throws IOException




Purpose: Closes the stream and releases all resources associated with it.




## Test case




The following is a test case for reading a file using the Reader class:




### Test code demo




package com.example.javase.io.reader;




import java.io.
import java.io.FileReader; import java.io.
import java.io.File; import java.io.FileReader; import java.io.
import java.io.Reader; import java.io.




/\*\*Reader; import java.io.FileReader; import java.io.
 \* @author bugs
 \* @version 1.0
 \* @date 2023/10/19 10:34
 \*/
public class ReaderTest {




    public static void main(String\[\] args) throws IOException {
        File file = new File("./template/fileTest.txt");
        Reader reader = new FileReader(file);




        char\[\] buffer = new char\[1024\];
        int len;
        while ((len = reader.read(buffer)) != -1) {
            System.out.println(new String(buffer, 0, len));
        }
        reader.close();
    }
}


According to the above test case, let's execute the main function to test reading the character data of the file, the result is shown in the following screenshot:


The result is shown in the following screenshot. [](https://static001.geekbang.org/infoq/f8/f8fb40ac8922c1b3472ed61c75d91467.png)


By comparing the output of the console with the original text, we can see that the test case utilizes the Reader class to read the file normally.


### Code Analysis


The above test code uses the Reader class to read character data from a file. The following is a step-by-step analysis of the code to help students accelerate their understanding.


First, we create a File object, specify the path of the file to be read, and then use the `FileReader` class to read the file into memory and return the `Reader` object. Then use `char[]` array as buffer, read the data from `Reader` to the buffer, and use `String` class to convert the buffer data into a string and output it to the console, until all the data have been read. Finally, close the Reader object to release the related resources. The whole reading process is very simple, did you learn it?


## Full Summary


This article provides a detailed introduction to the `Reader` class in Java, including its introduction, source code analysis, application scenarios, strengths and weaknesses of the analysis, methodology and test cases. Through the study of this article, we can better grasp the use of `Reader`, and in the development of the `Reader` class reasonable use.


## Summary


The `Reader` class is an abstract class for reading character streams in Java. It has the advantages of reading text data, automatically handling character encoding, etc., and can realize different functions through its subclasses. However, the Reader class reads data slowly, is not suitable for reading binary data, and cannot randomly access data in a file. When using the `Reader` class, pay attention to the use of buffers and other ways to improve reading speed and efficiency. Finally, be careful to close resources to avoid resource leakage problems.
