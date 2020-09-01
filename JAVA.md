# Java
#### Content
+ [Input](#input)
+ [Output](#output)
+ [Cycles](#cycles)

+ [JavaFX](#javaFX)




<a name="input"><h2>Input</h2></a>
+ from keyboard:
```
	Scanner in = new Scanner(System.in);
        System.out.print("Input a number: ");
        int num = in.nextInt();
        in.close();
```

  + The Scanner class has a number of other methods that allow you to get user-entered values:
    + next() -- reads the entered line up to the first space
    + nextLine() -- reads the entire input line
    + nextInt() -- reads the entered number int
    + nextByte() -- reads the entered number byte
    + nextFloat() -- reads the entered number float
    + nextShort() -- reads the entered number short
    + nextDouble() -- reads the entered number double
    + nextBoolean() -- reads a boolean value

+ of file:
```
try(FileReader reader = new FileReader("notes3.txt"))
        {
            int c;
            while((c=reader.read())!=-1){
                 
                System.out.print((char)c);
            } 
        }
```
  + To create a FileReader object, we can use one of its constructors:
    + FileReader(String fileName) 
    + FileReader(File file)
    + FileReader(FileDescriptor fd)

<a name="output"><h2>Output</h2></a>
+ in the console:
```
System.out.println("text");
System.out.printf("x=%d; y=%d \n", x, y);
```
  + Specifiers:
    + %d -- to output integer numbers
    + %x -- to output hexadecimal numbers
    + %f -- to output floating point numbers
    + %e -- to output numbers in exponential form, for example 1.3e + 01
    + %c -- to output a single character
    + %s -- to output string values
+ in file
```
            String text = "Hello Gold!";
            writer.write(text);
            writer.append('\n');
            writer.append('E'); 
            writer.flush();
```
  + You can use one of the following constructors to create a FileWriter object:
    + FileWriter(File file)
    + FileWriter(File file, boolean append)
    + FileWriter(FileDescriptor fd)
    + FileWriter(String fileName)
    + FileWriter(String fileName, boolean append) 

<a name="cycles"><h2>Cycles</h2></a>
+ for
`for(int i = 0; i < n; i++){...}`
We don't need to specify all conditions when declaring a loop.

  + foreach
`for (int x : nums) {...}`

+ while

`while (condition) {...}`

+ do ... while
`do {...} while(condition);`
<a name="javaFX"><h2>JAVAFX</h2></a>
--module-path /home/sanchir/javafx-sdk-11.0.2/lib --add-modules javafx.controls,javafx.fxml

+ Operators
  + `break` -- The break statement allows you to exit the loop at any moment.

  + `continue` -- The continue statement allows you to go to the next iteration.
