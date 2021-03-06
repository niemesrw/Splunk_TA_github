# Splunk_TA_github

Installation instructions:

1. Enable syslog output on the GitHub enterprise server, and send to a system running syslog-ng or equivalent

Here's my syslog-ng configuration:

destination d_github    { file("/opt/syslog-ng/github/$HOST"); };
filter f_github         { host("dns-name-github-server"); };
filter f_no_github      { not host("dns-name-github-server"); };
log { source(s_net); filter(f_github);          destination(d_github); };

2. Configure heavy / lightweight forwarder to index these events:

cat /opt/splunk/etc/system/local/inputs.conf

[monitor:///opt/syslog-ng/github]
sourcetype = github
index = github
host_segment = 4

3. Ensure you have created an index = github on your indexer

4. Set the timezone as appropriate

