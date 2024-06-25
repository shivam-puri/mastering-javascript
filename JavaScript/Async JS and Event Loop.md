
![[Pasted image 20240503121617.png]]

In a web browser environment, many functionalities commonly associated with JavaScript are indeed provided by the browser's APIs rather than being inherently part of the JavaScript language itself. These APIs are commonly referred to as Web APIs.

for example : 
==The services like `fetch setTimeOut DOM APIs localStorage console location` ... are not a part of javascript, they are actually provided by the webAPIs and are actually stored in the global object `window` .== 

> The DOM API contains essential services like `addEventListener()`,  `getElementById()`, `querySelector()` etc...

![[Pasted image 20240503122004.png]]

>So the browser, wraps up all these services and keeps it in the `window` global object and gives the call stack, access to this window global object. and that's how we use these services in javascript.

### How setTimeOut works behind the scenes in the browser


![[BTS setTimeOut|1200]]


Behind the scenes, the `setTimeout` function in the browser leverages the JavaScript runtime environment and the event loop to schedule the execution of a function after a specified delay. Here's how it works:

1. **Scheduling the Timeout**: When you call `setTimeout`, the browser's JavaScript engine (e.g., V8 in Chrome, SpiderMonkey in Firefox) schedules the provided function to be executed after the specified delay. The delay is typically specified in milliseconds.

2. **Event Loop**: ==The browser's JavaScript engine operates within an event loop. The event loop continuously checks for tasks to execute, such as handling user interactions, network requests, and timeouts==.

3. **Timer Management**: When you call `setTimeout`, the JavaScript engine sets up a timer internally. It notes the specified delay and the associated callback function. However, the callback function is not executed immediately.

4. **Waiting Period**: During the waiting period (the delay specified in `setTimeout`), the browser continues to execute other tasks, such as rendering the UI, handling user input, and processing other JavaScript code.

5. **Timeout Trigger**: Once the specified delay has elapsed, the timer associated with the `setTimeout` function expires. At this point, the browser adds the callback function to the task queue.

6. **Event Loop Execution**: The event loop continuously checks the task queue for pending tasks. When it encounters the callback function from the `setTimeout`, it moves it from the task queue to the execution stack for execution.

7. **Function Execution**: Finally, the browser executes the callback function. If the callback function contains any code, such as updating the DOM, making network requests, or performing any other operations, it is executed in this step.

It's essential to note that the callback function provided to `setTimeout` is executed asynchronously. That means it doesn't block the execution of other JavaScript code, allowing the browser to remain responsive while waiting for the timeout to elapse.

Overall, `setTimeout` is a mechanism provided by the browser's JavaScript engine to schedule the execution of code after a specified delay, enabling developers to create time-based behaviors and animations in web applications.


> The same behind the scenes for fetch, with a little difference that, instead of callback queue, fetch tasks after successful data retrieval, or transfer, are sent to the micro-task queue, which has a higher priority than the call stack queue



### So this is how javascript works asynchronously behind the scenes ![[anya_confused.png|right]]