# Template for logging to Splunk HTTP Event Collector

These is an example conf file _(input, filter, output)_ for logstash, that shows how to log events received by logstash, to a Splunk index realtime, and also archived to an S3 bucket.

This is not a common use case since most people who use logstash would use elasticsearch instead, or if they use Splunk, the use the Splunk agent _(especially with windows and linux)_ but this shows that it is possible.

## File info

__logstash_systemctloverride.conf:__ This is the file that contains environment variables for logstash running as a service using systemctl. Simply paste these settings as is into the vi interface that comes up when you run the `system edit logstash` command. This allows you to control variables that change per environment _(index, auth token, certs, etc.)_ without modifying your code.

__logstash-splunk-hec.conf__: All inclusive file for logstash. This specific file is using the __RELP__ input, and filtering haproxy logs, but any alternative input can be used, and any log types can be grabbed. The key is to see how the variables Splunk needs are generated.

## Deployment instructions

- In Splunk, you can setup a new HTTP Event Collector from __Settings -> Data Inputs -> HTTP Event Collector__
- Specify which indexes it should have access to, and note the Token Value. That token is what you assign to the __SPLUNKAUTH__ variable.
- Create an S3 bucket if you choose to use the S3 output (I used IAM roles on my instance, so credentials were not needed)
- Copy the contents of to the config directory for logstash.
- Grab the right service environment file from __logstash_systemctloverride.conf__ and paste it into the `systemctl edit logstash` command or copy into `/etc/systemd/system/logstash.service.d/override.conf`.
- Replace with your desired settings or the service won't start.
- Run `systemctl start logstash` or run it manually `logstash -f ...` but you'll need to set the environment variables manually in the override file _(I have never tried this)_.

## Meta

- Alfonso Brown
- alfonsorbrownATgmailDOTcom
