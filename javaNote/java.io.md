# java.io

Java.io 包几乎包含了所有操作输入、输出需要的类。所有这些流类代表了输入源和输出目标。
Java.io 包中的流支持很多种格式，比如：基本类型、对象、本地化字符集等等。

一个流可以理解为一个数据的序列。输入流表示从一个源读取数据，输出流表示向一个目标写数据。
Java为I/O 提供了强大的而灵活的支持，使其更广泛地应用到文件传输和网络编程中。

## 读取控制台输入

Java 的控制台输入由 System.in 完成。
为了获得一个绑定到控制台的字符流，你可以把 System.in 包装在一个 BufferedReader 对象中来创建一个字符流。

    BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

>BufferedReader 对象创建后，我们便可以使用 read() 方法从控制台读取一个字符，或者用 readLine() 方法读取一个字符串。

## 从控制台读取多字符输入

    // 使用 BufferedReader 在控制台读取字符

    import java.io.*;

    public class BRRead {
        public static void main(String args[]) throws IOException
        {
            char c;
            // 使用 System.in 创建 BufferedReader 
            BufferedReader br = new BufferedReader(new 
                                InputStreamReader(System.in));
            System.out.println("输入字符, 按下 'q' 键退出.");
            // 读取字符
            do {
                c = (char) br.read();
                System.out.println(c);
            } while(c != 'q');
        }
    }

## 从控制台读取字符串

从标准输入读取一个字符串需要使用 BufferedReader 的 readLine() 方法。

    // 使用 BufferedReader 在控制台读取字符
    import java.io.*;
        public class BRReadLines {
        public static void main(String args[]) throws IOException
        {
            // 使用 System.in 创建 BufferedReader 
            BufferedReader br = new BufferedReader(new
                                    InputStreamReader(System.in));
            String str;
            System.out.println("Enter lines of text.");
            System.out.println("Enter 'end' to quit.");
            do {
                str = br.readLine();
                System.out.println(str);
            } while(!str.equals("end"));
        }
    }

## 读写文件

如前所述，一个流被定义为一个数据序列。输入流用于从源读取数据，输出流用于向目标写数据。

抽象基本组件是InputStream类。超类InputStream包含从输入流读取数据的基本方法，所有具体类都支持这些方法。

对输入流的基本操作是从其读取数据。 InputStream类中定义的一些重要方法在下表中列出。

### FileInputStream

该流用于从文件读取数据，它的对象可以用关键字 new 来创建。
有多种构造方法可用来创建对象。
可以使用字符串类型的文件名来创建一个输入流对象来读取文件：

在Java I/O中，流意味着数据流。流中的数据可以是字节，字符，对象等。

要从文件读取，我们需要创建一个FileInputStream类的对象，它将表示输入流。

    String srcFile = "test.txt";
    FileInputStream fin  = new FileInputStream(srcFile);

如果文件不存在，FileInputStream类的构造函数将抛出FileNotFoundException异常。要处理这个异常，我们需要将你的代码放在try-catch块中，如下所示：

    try  {
        FileInputStream fin  = new FileInputStream(srcFile);
    }catch  (FileNotFoundException e){
        // The error  handling code  goes  here
    }

如何从文件输入流一次读取一个字节。

    import java.io.FileInputStream;
    import java.io.FileNotFoundException;
    import java.io.IOException;

    public class Main {
        public static void main(String[] args) {
            String dataSourceFile = "asdf.txt";
            try (FileInputStream fin = new FileInputStream(dataSourceFile)) {

            byte byteData;
            while ((byteData = (byte) fin.read()) != -1) {
                System.out.print((char) byteData);
            }
            } catch (FileNotFoundException e) {
            ;
            } catch (IOException e) {
            e.printStackTrace();
            }
        }
    }

### FileOutputStream

该类用来创建一个文件并向文件中写数据。
如果该流在打开文件进行输出前，目标文件不存在，那么该流会创建该文件。
有两个构造方法可以用来创建 FileOutputStream 对象。

    import java.io.*;
    
    public class fileStreamTest {
        public static void main(String args[]) {
            try {
                byte bWrite[] = { 11, 21, 3, 40, 5 };
                OutputStream os = new FileOutputStream("test.txt");
                for (int x = 0; x < bWrite.length; x++) {
                    os.write(bWrite[x]); // writes the bytes
                }
                os.close();
    
                InputStream is = new FileInputStream("test.txt");
                int size = is.available();
    
                for (int i = 0; i < size; i++) {
                    System.out.print((char) is.read() + "  ");
                }
                is.close();
            } catch (IOException e) {
                System.out.print("Exception");
            }
        }
    }

>
    //文件名 :fileStreamTest2.java
    import java.io.*;

    public class fileStreamTest2{
        public static void main(String[] args) throws IOException {
            
            File f = new File("a.txt");
            FileOutputStream fop = new FileOutputStream(f);
            // 构建FileOutputStream对象,文件不存在会自动新建
            
            OutputStreamWriter writer = new OutputStreamWriter(fop, "UTF-8");
            // 构建OutputStreamWriter对象,参数可以指定编码,默认为操作系统默认编码,windows上是gbk
            
            writer.append("中文输入");
            // 写入到缓冲区
            
            writer.append("\r\n");
            //换行
            
            writer.append("English");
            // 刷新缓存冲,写入到文件,如果下面已经没有写入的内容了,直接close也会写入
            
            writer.close();
            //关闭写入流,同时会把缓冲区内容写入文件,所以上面的注释掉
            
            fop.close();
            // 关闭输出流,释放系统资源

            FileInputStream fip = new FileInputStream(f);
            // 构建FileInputStream对象
            
            InputStreamReader reader = new InputStreamReader(fip, "UTF-8");
            // 构建InputStreamReader对象,编码与写入相同

            StringBuffer sb = new StringBuffer();
            while (reader.ready()) {
                sb.append((char) reader.read());
                // 转成char加到StringBuffer对象中
            }
            System.out.println(sb.toString());
            reader.close();
            // 关闭读取流
            
            fip.close();
            // 关闭输入流,释放系统资源

        }
    }

## File

File类的对象是文件或目录的路径名的抽象表示。

### 创建文件

构造函数：

    File(String pathname)
    File(File parent, String child)
    File(String parent, String child)
    File(URI uri)

名为test.txt的文件不必存在，以使用此语句创建File对象。

dummyFile对象表示抽象路径名，它可能指向或可能不指向文件系统中的真实文件。

使用File对象，我们可以创建新文件，删除现有文件，重命名文件，更改文件的权限等。

File类中的isFile()和isDirectory()告诉File对象是否表示文件或目录。

我们可以使用File类的exists()方法检查File对象的抽象路径名是否存在。

### 路径

绝对路径在文件系统上唯一标识文件。规范路径是唯一标识文件系统上文件的最简单路径。  
我们可以使用getAbsolutePath()和getCanonicalPath()方法来分别获得由File对象表示的绝对路径和规范路径。

### 创建新文件

我们可以使用File类的createNewFile()方法创建一个新文件：

    File dummyFile = new File("test.txt");
    boolean fileCreated  = dummyFile.createNewFile();

File类的createTempFile()静态方法

    File  tempFile = File.createTempFile("abc", ".txt");

### 文件夹创建

我们可以使用mkdir()或mkdirs()方法创建一个新目录。

仅当路径名中指定的父目录已存在时，mkdir()方法才创建目录。

    File newDir  = new File("C:\\users\\home");

只有当C:\users目录已经存在时，newDir.mkdir()方法才会创建主目录。

newDir.mkdirs()方法将创建users目录（如果它不存在于C：驱动器中），它将在C:\users目录下创建主目录。

    //删除
    File dummyFile = new File("dummy.txt"); 
    dummyFile.delete();
    //重命名
    renameTo()；
    //获取文件的大小(以字节为单位)。
    File.length();

example:

    import java.io.File;

    public class Main {
        public static void main(String[] args) throws Exception {
            File newFile = new File("my_new_file.txt");
            printFileDetails(newFile);

            // Create a new file
            boolean fileCreated = newFile.createNewFile();
            if (!fileCreated) {
            System.out.println(newFile + "  could   not  be  created.");
            }
            printFileDetails(newFile);

            // Delete the new file
            newFile.delete();

            System.out.println("After deleting the new file:");
            printFileDetails(newFile);

            // recreate the file
            newFile.createNewFile();

            printFileDetails(newFile);

            // Let"s tell the JVM to delete this file on exit
            newFile.deleteOnExit();

            System.out.println("After  using deleteOnExit() method:");
            printFileDetails(newFile);

            // Create a new file and rename it
            File firstFile = new File("my_first_file.txt");
            File secondFile = new File("my_second_file.txt");

            fileCreated = firstFile.createNewFile();
            if (fileCreated || firstFile.exists()) {
            printFileDetails(firstFile);
            printFileDetails(secondFile);

            boolean renamedFlag = firstFile.renameTo(secondFile);
            if (!renamedFlag) {
                System.out.println("Could not  rename  " + firstFile);
            }
            printFileDetails(firstFile);
            printFileDetails(secondFile);
            }
        }
        public static void printFileDetails(File f) {
            System.out.println("Absolute Path: " + f.getAbsoluteFile());
            System.out.println("File exists:  " + f.exists());
        }
    }

以下代码显示如何列出所有可用的根目录。

    import java.io.File;

    public class Main {
        public static void main(String[] args) {
            File[] roots = File.listRoots();
            System.out.println("List  of  root directories:");
            for (File f : roots) {
            System.out.println(f.getPath());
            }
        }
    }
我们可以使用File类的list()或listFiles()方法列出目录中的所有文件和目录。  
list()方法返回一个String数组，而listFiles()方法返回一个File数组。


