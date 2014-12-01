#Are you curious how many recursive calls you can make on your JavaScript engine? 

How many recursive calls?    
The following function lets you find out (inspired by a gist by Ben Alman). 
```js
    function computeMaxCallStackSize() {
        try {
            return 1 + computeMaxCallStackSize();
        } catch (e) {
            // Call stack overflow
            return 1;
        }
    }
```
Three results:    
-   Node.js: 11034    
-   Firefox: 50994    
-   Chrome: 10402    

What does this number mean? Mr. Aleph pointed out to me that on V8, the number of recursive calls you can make depends on two quantities: the size of the stack and the size of the stack frame (holding parameters and local variables). You can verify the latter by adding local variables to computeMaxCallStackSize() – it’ll return a lower number.
Tail call optimization in ECMAScript 6
ECMAScript 6 will have tail call optimization: If a function call is the last action in a function, it is handled via a “jump”, not via a “subroutine call”. That means that, if you slightly rewrote computeMaxCallStackSize(), it would run forever under ECMAScript 6 (in strict mode):

```js
    function computeMaxCallStackSize(size) {
        size = size || 1;
        return computeMaxCallStackSize(size + 1);
    }
```
    
Did you like this blog post? Check out my book “Speaking JavaScript” (free to read online).

http://www.2ality.com/2014/04/call-stack-size.html
