# Laravel websockets

Laravel WebSockets is a package for Laravel 5.7 and up, built on top of `Ratchet` - WebSockets for PHP, that will get
 your application started with WebSockets in no-time. It has a drop-in Pusher API replacement, has a debug dashboard, realtime statistics and even allows you to create custom WebSocket controllers
 
 On Unix systems, every user that connects to your WebSocket server is represented as a file somewhere on the system. As a security measurement of every Unix based OS, the number of "file descriptors" an application may have open at a time is limited - most of the time to a default value of **1024** - **which would result in a maximum number of 1024 concurrent users on your WebSocket server**.
 
 In addition to the OS restrictions, this package makes use of an event loop called "**stream_select**", which has a
  hard limit of 1024.
  
  #### Changing the event loop
  
  To make use of a different event loop, that does not have a hard limit of 1024 concurrent connections, you can either install the `ev` or `event` PECL extension using
  
 ```
 yum install libev libev-devel php-pecl-ev
```