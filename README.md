### Send watchdog notifications to systemd

Lib for java daemons to send keep-alive messages to refresh the systemd watchdog timestamp.
 
#### Simple Usage
 
```java
	SysDNotifier() notifier = new SysDNotifier();
 	notifier.start();
```
 
### Advanced Usage
```java
	SysDNotifier notifier = new SysDNotifier();
	notifier.setAuditor(new Auditor() {
		@Override
		public boolean allGood() {				
			//Do more extensive internal checking of the process, and if it's ok, return true.
			//Returning false will have the effect of suppressing the keep-alive message, causing systemd to kill and restart the process. 
			return true;
		}
	});
	notifier.start();
 ```
 
### .service file layout 
```shell
[Unit]
Description=My Service

[Service]
PIDFile=/tmp/my-service.pid
ExecStart=/usr/lib/jvm/java-1.8.0/bin/java -jar /usr/local/bin/my-service.jar
WatchdogSec=500
Restart=on-failure

[Install]
WantedBy=multi-user.target

```
  