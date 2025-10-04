# Process Control and JavaScript Threading

## Process Control Block (PCB)

1. PCB or Process Control Block stays in RAM.

2. Process unit = thread.

3. Context switching within a process is called process concurrency.

4. Context switching within a thread is called thread concurrency.

## Process and Thread Creation

1. Whenever a process gets created, by default it has one thread created in it. All the work is done in the thread.

2. JavaScript is a single-threaded programming language.

3. The thread talks with the processor through the pointing register.

4. JavaScript can never do 2 tasks at a time.

5. Memory component creates an environment where variables are stored. That's why it's called memory component or variable environment.

6. Thread executes code line by line.

7. Code component or thread of execution refers to 1 thread.

8. JavaScript's code component is 1 thread because within our execution context, the code component executes code 1 line at a time, which indicates that our code component is a thread.

9. Only because of the pointing register, code executes line by line. And whichever line our JS code component is on, that line gets executed.

10. Meaning: if there are 5 execution contexts, ultimately the 5th execution context's code component stays in the original thread. The rest of the execution contexts are held.

11. JS code executes line by line in 1 thread.

12. Next class question: loader + data fetch are 2 separate threads. So how is JS single-threaded?

## Browser and Web APIs

1. Browser exists on computer or mobile, which are called devices. Any smart device is a device.

2. That device may have various sensors. We can access these sensors through various software.

3. Device's various sensors are called resources. The entity that uses these resources becomes that.

4. Browser companies have written some code to access these sensors.

5. Browser has its own resources:
   - Timer
   - Geo Location
   - URL
   - Console
   - Local storage
   - and many many others

6. We can see that some resources belong to the browser, some resources belong to the device. For example: camera, audio output, etc.

7. Browser has some code to access device resources. These are called Web APIs.

   Examples:
   - setTimeout()
   - DOM APIs (document.*)
   - fetch()
   - localStorage
   - console.log()
   - location
   - etc

8. The purpose of Web APIs is to access browser resources.

## JavaScript Engine

1. JS only executes within the browser's JS engine.

2. For example, if these sensors are on the device, the browser's code to access them is the APIs.

3. To understand JavaScript code, you need a browser because the JS engine is in the browser.

4. Code can only be executed in the browser's JS engine.

5. The heart of the browser is the JavaScript Engine.

6. The JavaScript engine executes JavaScript code.

7. Inside the JS engine is the call stack.

8. Inside the call stack are various execution contexts.

9. Browser companies make their own JS engines.

10. Because different browsers have different JS engines, the same code may not run properly in different browsers.

11. Web APIs enter the memory component before the global execution context's creation phase starts.

12. Web APIs are never JavaScript.

13. The browser is made in whatever language, the browser's Web APIs are made in the same language.

14. In the execution phase, when a Web API like console.log is called, our code component calls the Web API that we know as console, which does the work. Log is a function that we will learn about later. console.log().

15. We will learn in the next class how JS does multiple tasks with just 1 thread.
