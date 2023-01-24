<!-- TOC start -->
 # [Python-schedule_library-for-system_admin](#python-schedule_library-for-system_admin)
  * ### [Here's an example of how a system administrator might use a job scheduler in Python to automate regular backups of important files and databases:  ](#heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-automate-regular-backups-of-important-files-and-databases)
  * ### [Here's an example of how a system administrator might use a job scheduler in Python to automate software updates:](#heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-automate-software-updates)
  * ### [Here's an example of how a system administrator might use a job scheduler in Python to monitor disk usage and send notifications if usage exceeds a certain threshold: {#monitor disk usage and send notifications if usage exceeds a certain threshold}](#heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-monitor-disk-usage-and-send-notifications-if-usage-exceeds-a-certain-threshold-monitor-disk-usage-and-send-notifications-if-usage-exceeds-a-certain-threshold)
  * ### [Here's an example of how a system administrator might use a job scheduler in Python to automate log rotation:](#heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-automate-log-rotation)
  * ### [Running load tests on a regular basis is a good way for system administrators to identify system bottlenecks and performance issues.](#running-load-tests-on-a-regular-basis-is-a-good-way-for-system-administrators-to-identify-system-bottlenecks-and-performance-issues)
  * ### [Here are a few more ways that you can use the schedule library in Python:](#here-are-a-few-more-ways-that-you-can-use-the-schedule-library-in-python)
<!-- TOC end -->
<!-- TOC --><a name="python-schedule_library-for-system_admin"></a>
# Python-schedule_library-for-system_admin
The schedule library is a popular choice for scheduling jobs in Python. It allows you to schedule a function to be run at a specific time. Here's an few example of how to use it:

![5d6e60d9c9361a764d0929b8_system-administrator-1270714194](https://user-images.githubusercontent.com/67478827/214340153-ccb32efe-bc3f-4161-9f0e-34ac2d635029.jpeg)


**Here are a few examples of tasks that a system administrator might schedule using a job scheduler in Python:**

* Running regular backups of important files and databases: By scheduling a script to run at a specific time, the system administrator can ensure that backups are taken on a regular basis without having to manually run the backup script.

* Automating software updates: By scheduling a script to check for updates and install them at a specific time, the system administrator can ensure that all software on the system is up-to-date.

* Monitoring disk usage and sending notifications: By scheduling a script to check disk usage and send an email or text message notification if usage exceeds a certain threshold, the system administrator can be notified of potential issues before they become critical.

* Automating log rotation: By scheduling a script to rotate logs on a regular basis, the system administrator can ensure that logs do not become too large, which can cause performance issues.

* Running regular security scans: By scheduling a script to run regular security scans, the system administrator can identify potential vulnerabilities and take action to address them.

* Automating system maintenance tasks: By scheduling a script to run regular maintenance tasks such as cleaning up temporary files, clearing the swap space and updating the system, the system administrator can ensure the system is running optimally.

* Running load tests to identify system bottlenecks: By scheduling a script to run load tests, the system administrator can identify areas of the system that are causing performance issues and take steps to address them.

* Running regular performance monitoring: By scheduling a script to gather performance metrics and send alerts when certain thresholds are exceeded, the system administrator can quickly identify issues and take action to resolve them.

_**These are just a few examples of how a job scheduler can be used to automate tasks for a system administrator. The specific tasks will depend on the needs of the system and the organization.**_

<!-- TOC --><a name="heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-automate-regular-backups-of-important-files-and-databases"></a>
## Here's an example of how a system administrator might use a job scheduler in Python to automate regular backups of important files and databases:  

```Python
import schedule
import time
import shutil

def backup():
    # Code to backup important files and databases goes here
    shutil.copytree("/path/to/important/files", "/path/to/backup/location")
    print("Backup complete!")

schedule.every().day.at("01:00").do(backup)

while True:
    schedule.run_pending()
    time.sleep(1)
```
In this example, 
* The backup() function contains the code to backup the important files and databases. 
* The shutil.copytree() function is used to copy the files from the source directory to the backup directory. 
* The schedule.every().day.at("01:00").do(backup) line tells the scheduler to run the backup() function every day at 1:00 AM. 
* The while True loop runs the scheduled job repeatedly.
_**If you want to run it on a remote machine you need to use some other libraries like paramiko to run the script on remote machine via SSH.**_
```python
import schedule
import time
import paramiko

def backup():
    # Connect to the remote machine using SSH
    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh.connect('remote_host', username='user', password='password')

    # Run the backup script on the remote machine
    ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command('/path/to/backup/script')

    # Print the output of the backup script
    print(ssh_stdout.read())

    # Close the SSH connection
    ssh.close()

schedule.every().day.at("01:00").do(backup)

while True:
    schedule.run_pending()
    time.sleep(1)
```
In this example,
* The paramiko.SSHClient() is used to create an SSH client and connect to the remote machine.
* The ssh.exec_command() method is used to run the backup script on the remote machine, and the output is captured in the ssh_stdout variable.
* The ssh.close() method is used to close the SSH connection after the script is done.
**It's worth mentioning that using passwords in your scripts could be a security issue. It's recommended to use SSH key pairs as an authentication method.**
```python
import schedule
import time
import paramiko

private_key = paramiko.RSAKey(filename='/path/to/private/key')

def backup():
    # Connect to the remote machine using SSH
    ssh = paramiko.SSHClient()
    ssh.set_missing_host_key_policy(paramiko.AutoAddPolicy())
    ssh.connect('remote_host', username='user', pkey=private_key)

    # Run the backup script on the remote machine
    ssh_stdin, ssh_stdout, ssh_stderr = ssh.exec_command('/path/to/backup/script')

    # Print the output of the backup script
    print(ssh_stdout.read())

    # Close the SSH connection
    ssh.close()

schedule.every().day.at("01:00").do(backup)

while True:
    schedule.run_pending()
    time.sleep(1)
```
* File integrity check: You can use the hashlib library to calculate a checksum of the files before and after the backup process and compare them.
```python
import hashlib

def check_integrity(filename):
    """Check integrity of the file by calculating its checksum"""
    sha256 = hashlib.sha256()
    with open(filename, 'rb') as f:
        while True:
            data = f.read(8192)
            if not data:
                break
            sha256.update(data)
    return sha256.hexdigest()

# Compare the checksum before and after the backup
original_checksum = check_integrity("original_file.txt")
backup_checksum = check_integrity("backup_file.txt")

if original_checksum == backup_checksum:
    print("File integrity is maintained")
else:
    print("File integrity is compromised")
```
<!-- TOC --><a name="heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-automate-software-updates"></a>
## Here's an example of how a system administrator might use a job scheduler in Python to automate software updates:
```python
import schedule
import time
import subprocess

def update():
    # Update the package list
    subprocess.run(["apt-get", "update"])
    # Upgrade all packages
    subprocess.run(["apt-get", "upgrade", "-y"])
    print("Software updates complete!")

schedule.every().day.at("02:00").do(update)

while True:
    schedule.run_pending()
    time.sleep(1)
```
In this example, 
* The update() function uses the subprocess module to run the apt-get update and apt-get upgrade -y commands.
* These commands update the package list and upgrade all installed packages to their latest version.
* The schedule.every().day.at("02:00").do(update) line tells the scheduler to run the update() function every day at 2:00 AM.
* The while True loop runs the scheduled job repeatedly.

_Please keep in mind that this example is for systems that use **apt** package manager. If you are using a different package manager such as **yum** or **dnf**, you will need to use the appropriate commands for that package manager._
<!-- TOC --><a name="heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-monitor-disk-usage-and-send-notifications-if-usage-exceeds-a-certain-threshold-monitor-disk-usage-and-send-notifications-if-usage-exceeds-a-certain-threshold"></a>
## Here's an example of how a system administrator might use a job scheduler in Python to monitor disk usage and send notifications if usage exceeds a certain threshold: {#monitor disk usage and send notifications if usage exceeds a certain threshold}
```python
import schedule
import time
import os
import smtplib

THRESHOLD = 90

def check_disk_usage():
    # Get the total disk space and the used disk space
    total, used, free = os.statvfs("/")
    # Calculate the disk usage percentage
    usage = (used / total) * 100

    if usage > THRESHOLD:
        # Send an email notification
        server = smtplib.SMTP('smtp.example.com')
        server.login("username", "password")
        msg = "WARNING: Disk usage is at {}%".format(usage)
        server.sendmail("sender@example.com", "recipient@example.com", msg)
        server.quit()
        print("Notification sent!")
    else:
        print("Disk usage is at {}%".format(usage))

schedule.every(10).minutes.do(check_disk_usage)

while True:
    schedule.run_pending()
    time.sleep(1)
```
In this example,
* The check_disk_usage() function uses the os.statvfs() function to get the total, used, and free disk space, and then calculates the disk usage percentage.
* If the usage is greater than the THRESHOLD (90% in this example), the function sends an email notification using the smtplib library.

_You can also use other libraries like twilio to send text message notification, or use a third-party service like pagerduty to send notifications._

**Please note that the above example uses an unencrypted SMTP server on port 25.**

If your SMTP server requires encryption or uses a different port, you will need to update the smtplib.SMTP() line accordingly. 

if your server uses SSL encryption on port 465, you can use smtplib.SMTP_SSL() instead. If your server uses TLS encryption on port 587, you can use smtplib.SMTP() and call the starttls() method.
```python
server = smtplib.SMTP_SSL('smtp.example.com', 465)
```
or,
```python
server = smtplib.SMTP('smtp.example.com', 587)
server.starttls()
```
<!-- TOC --><a name="heres-an-example-of-how-a-system-administrator-might-use-a-job-scheduler-in-python-to-automate-log-rotation"></a>
## Here's an example of how a system administrator might use a job scheduler in Python to automate log rotation:

```python
import schedule
import time
import logging
import shutil

def rotate_logs():
    # Get the current log file
    current_log = logging.FileHandler.baseFilename
    # Create a new log file with a timestamp
    new_log = current_log + "." + time.strftime("%Y%m%d-%H%M%S")
    # Rename the current log file
    shutil.move(current_log, new_log)
    # Reopen the log file
    logging.getLogger().addHandler(logging.FileHandler(current_log))
    print("Logs rotated!")

schedule.every().day.at("03:00").do(rotate_logs)

while True:
    schedule.run_pending()
    time.sleep(1)
```
In this example, the rotate_logs() function uses the shutil library to rename the current log file by appending a timestamp to its name, and then reopens the log file. This ensures that the logs are rotated on a regular basis, which prevents them from becoming too large and causing performance issues.

_**Instead of rotating logs on a fixed schedule, you can configure log rotation based on the size of the log files. This way, logs will be rotated when they reach a certain size, rather than at a fixed time.**_
```python
import logging
from logging.handlers import RotatingFileHandler

logger = logging.getLogger()
log_formatter = logging.Formatter('%(asctime)s %(levelname)s %(funcName)s(%(lineno)d) %(message)s')
log_file = "application.log"

my_handler = RotatingFileHandler(log_file, mode='a', maxBytes=5*1024*1024, backupCount=2, encoding=None, delay=0)
my_handler.setFormatter(log_formatter)
my_handler.setLevel(logging.INFO)
logger.addHandler(my_handler)
```
In this example,
* we are using RotatingFileHandler class to handle the rotation of the log files.
* The maxBytes parameter is set to 5MB and backupCount is set to 2, meaning that when the log file reaches 5MB, the current log file will be renamed and a new log file will be created, keeping only 2 previous log files.

* _**This way, you can ensure that logs do not become too large and cause performance issues.**_

<!-- TOC --><a name="running-load-tests-on-a-regular-basis-is-a-good-way-for-system-administrators-to-identify-system-bottlenecks-and-performance-issues"></a>
## Running load tests on a regular basis is a good way for system administrators to identify system bottlenecks and performance issues.
Here's an example of how a system administrator might use a job scheduler in Python to automate load tests using a library like locust:
```python
import schedule
import time
from locust import HttpLocust, TaskSet, task

class UserBehavior(TaskSet):
    @task
    def load_test(self):
        self.client.get("/")

class WebsiteUser(HttpLocust):
    task_set = UserBehavior
    min_wait = 5000
    max_wait = 9000

def run_load_test():
    os.system("locust -f load_test.py")
    print("Load test complete!")

schedule.every().day.at("04:00").do(run_load_test)

while True:
    schedule.run_pending()
    time.sleep(1)
```
By running load tests on a regular basis and analyzing the results, system administrators can identify areas of the system that are causing performance issues and take steps to address them, such as adding more resources, optimizing the code, or implementing caching mechanisms.

In this example,
* The run_load_test() function uses the os library to run the load test script using the command locust -f load_test.py.
* The UserBehavior class defines the behavior of the users during the load test, in this case, it's a simple GET request to the root of the website.
* The WebsiteUser class is the class that will be used to run the test, it will run the UserBehavior class and set the min and max wait time between requests.

**Here are some examples of how the UserBehavior class might define the behavior of users in a load test for the website.**
* Navigation: Users visit the homepage, then navigate to different pages on the website such as the "About" page, the "Contact" page, and the "Blog" page.
    ```python
    class UserBehavior(TaskSet):
        @task
        def homepage(self):
            self.client.get("/")
        @task
        def about(self):
            self.client.get("/about/")
        @task
        def contact(self):
            self.client.get("/contact/")
        @task
        def blog(self):
            self.client.get("/blog/")
    ```
* Searching: Users perform searches on the website using different keywords.
    ```python
    class UserBehavior(TaskSet):
        @task
        def search(self):
            self.client.get("/search?q=python")
        @task
        def search_2(self):
            self.client.get("/search?q=AI")
    ```
* Submitting a form: Users fill out and submit a contact form on the website.
    ```python
    class UserBehavior(TaskSet):
        @task
        def submit_form(self):
            self.client.post("/contact/", {"name": "John Doe", "email": "johndoe@example.com", "message": "This is a test"})
    ```

<!-- TOC --><a name="here-are-a-few-more-ways-that-you-can-use-the-schedule-library-in-python"></a>
## Here are a few more ways that you can use the schedule library in Python:

* Scheduling a script to run at a specific time: You can schedule a script to run at a specific time, for example, every day at 6:00 AM.
    ```python
    schedule.every().day.at("06:00").do(your_function)
    ```
* Scheduling a script to run at a specific interval: You can schedule a script to run at a specific interval, for example, every 2 hours.
    ```python
    schedule.every(2).hours.do(your_function)
    ```
* Scheduling a script to run on specific days: You can schedule a script to run on specific days, for example, every Monday and Wednesday at 8:00 PM.
    ```python
    schedule.every().monday.at("20:00").do(your_function)
    schedule.every().wednesday.at("20:00").do(your_function)
    ```
* Scheduling a script to run at a specific time, with a specific interval: You can schedule a script to run at a specific time, with a specific interval, for example, every day at 9:00 AM and 4:00 PM.
    ```python
    schedule.every().day.at("09:00").do(your_function)
    schedule.every().day.at("16:00").do(your_function)
    ```
* Scheduling a script to run when the system starts: You can schedule a script to run when the system starts, by using the every().minutes schedule, this will run the script every minute, it will check if the script is already running or not, if not then it will start the script.
    ```python
    schedule.every().minute.do(your_function)
    ```
**You can use the pytz library to schedule the task with the respective timezone of the machine where the script is running, this way you can make sure the script runs at the correct time.**
```python
from datetime import datetime
import pytz

schedule.every().day.at("06:00").do(your_function)
schedule.every().day.at("06:00").do(your_function).tag("EST", "PST")

def your_function():
    EST = pytz.timezone("EST")
    PST = pytz.timezone("PST")
    now_EST = datetime.now(EST)
    now_PST = datetime.now(PST)
    print("EST:", now_EST.strftime("%Y-%m-%d %H:%M:%S"))
    print("PST:", now_PST.strftime("%Y-%m-%d %H:%M:%S"))
```



