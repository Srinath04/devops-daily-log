### Issue: Jenkins managed by systemd is failing to start.

### Fix: added JAVA_HOME override in systemd

#### Diagnosis:
- I traced the failure from journalctl and found that Jenkins was failing to start due to a Java version mismatch. 
- Jenkins was compiled for Java 11+ (class file 55.0)
- System default Java was 8 (class file 52.0) found via update-alternatives
- Java 21 was installed but not active
- Fix: added JAVA_HOME override in systemd drop-in
  /etc/systemd/system/jenkins.service.d/override.conf
- Lesson: Verify default JAVA_HOME path and application's unit file path when service has trouble starting with Java errors.