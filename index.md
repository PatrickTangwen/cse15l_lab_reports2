# CSE15L Lab Reports 2

**Part 1 String Server**<br>
**code part**<br>
```
import java.io.IOException;
import java.net.URI;
import java.util.ArrayList;

class Handler implements URLHandler {
    // The one bit of state on the server: a number that will be manipulated by
    // various requests.
    ArrayList<String> words_arr = new ArrayList<String>();
    String output = "";
    public String handleRequest(URI url) {
        System.out.println("Path: " + url.getPath());
        if (url.getPath().contains("/add-message")) {
            String[] parameters = url.getQuery().split("=");
            if (parameters[0].equals("s")) {
                words_arr.add(parameters[1]);
                for (String s : words_arr) {
                    if(output.indexOf(s) == -1) {
                        output += s + "\n"; 
                    }       	
                }
            }
        }		
        return output;
        }
}

class StringServer {
    public static void main(String[] args) throws IOException {
        if(args.length == 0){
            System.out.println("Missing port number! Try any number between 1024 to 49151");
            return;
        }

        int port = Integer.parseInt(args[0]);

        Server.start(port, new Handler());
    }
}
```


**Screenshot 1**<br>
<img width="561" alt="cse15l_lab2_sh" src="https://user-images.githubusercontent.com/102566928/215040885-6468acfa-d6c4-42f9-9fa0-67b1e62b45dd.png">
<br>
* In this screenshot, `handleRequest` and main method in StringServer.java file are called.
* The relevant argument is `4700` and `/add-message?s=Good Morning`. The value are Arraylist `words_arr` and String `output`.
* When the argument is passed into the Hanlder method, `/add-message?s=Good Morning` is break down into path + query. First, the method will check whether the value contains the path `/add-message`. If it does, it will extract the query part which is the `?s=Good Morning`. Specifically, it will extract two parameters: `s` and the content after equal sign. In this example, the first parameter is 's' and the second one is `Good Morning`. Then,`Good Morning` will bed added into into the arraylist `word_arr`. If the element is never found in String `output`, then output will concatenate this element with a newline character `\n`. Finally, `output` is returned.




**Screenshot 2**<br>
<img width="555" alt="cse15l_lab2_sh2" src="https://user-images.githubusercontent.com/102566928/215041009-0104f431-62af-4132-b74b-3d3e175b1a51.png">
<br>
* In this screenshot, `handleRequest` and main method in StringServer.java file are called.
* The relevant argument is `4700` and `/add-message?s=Good Morning`. The value are Arraylist `words_arr` and String `output`.
* When the argument is passed into the Hanlder method, `/add-message?s=Good Night` is break down into path + query. First, the method will check whether the value contains the path `/add-message`. If it does, it will extract the query part which is the `?s=Good Night`. Specifically, it will extract two parameters: `s` and the content after equal sign. In this example, the first parameter is 's' and the second one is `Good Night`. Then,`Good Night` will bed added into into the arraylist `word_arr`. If the element is never found in String `output`, then output will concatenate this element with a newline character `\n`. Right now, String `output` contains two words "Good Morning\n" and "Good Night\n". Finally, `output` is returned.

**Part 2 Bug Analysis**<br>
I choose bugs from reverseInPlace method in Array Methods part.<br>
Failure-inducing input:<br>
```
@Test 
public void testReverseInPlace2() {
    int[] input2 = {1,2,3};
    ArrayExamples.reverseInPlace(input2);
    System.out.println(Arrays.toString(input2));
    assertArrayEquals(new int[]{3,2,1}, input2);
}
```
Input that doesn’t induce a failure: <br>
```
@Test 
public void testReverseInPlace() {
    int[] input1 = {5};
    ArrayExamples.reverseInPlace(input1);
    assertArrayEquals(new int[]{ 3 }, input1);
}
```

The symptom:<br>
<img width="807" alt="image" src="https://user-images.githubusercontent.com/102566928/215232789-ba3c64ef-2e77-400d-b31b-931f229c5d49.png">

Code before fixing:<br>
```
for(int i = 0; i < arr.length; i += 1) {
     arr[i] = arr[arr.length - i - 1];
   }
}
```
Here the bug is that this method traverse the whole array. When half of the elements are swapped, the rest of elements are actually copy the value of first half elements.

Code after fixing:
```
for(int i = 0; i < arr.length / 2; i += 1) {
  int temp  = arr[i];
  arr[i] = arr[arr.length - i - 1];
  arr[arr.length - i - 1] = temp;
} 
```
Instead, to reverse the array in place without creating a new array, we only need to traverse half of the elements. During reversing, we can create a temp variable to store the value before swapping and then swap the first and last element until the index i reaches to the value length/2. In this way, we avoid swap repeated elements after i reaching the value length/2. <br>



