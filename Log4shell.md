**Remediation steps**

1. Update Log4j <br>
2. Disable Log4j Lookups. Per this [Apache Jira ticket](https://issues.apache.org/jira/browse/LOG4J2-2109), after Log4j version 2.10, passing `‐Dlog4j2.formatMsgNoLookups=true` to the JVM will disable Log4j lookups and protect the system from exploitation. <br>
3. Remove dangerous .class files. Some systems may depend on Log4j lookups for logging, but may not need the JNDI lookup feature. 
It is possible to modify the log4j JAR file and remove these dangerous class files. This can be accomplished by extracting the JAR and deleting the `org/apache/logging/log4j/core/lookup/JndiLookup.class` class file. <br>
Block dangerous requests at the WAF. The JNDI lookups required for exploitation have a required prefix for Log4j to perform the lookup. Any request with with a string like `${jndi:xyz}` will trigger a JNDI lookup. The prefix `${jndi:` could be added to a blocklist in a web application firewall to block exploit attempts. Note that this approach is by its nature incomplete. <br>
