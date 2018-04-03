# CVE-2018-1315: 'COPY FROM FTP' statement in HPL/SQL can write to arbitrary location if the FTP server is compromised (HIVE-18815)

Severity: Important

Vendor: The Apache Software Foundation

Versions Affected: Hive 2.1.0 to 2.3.2

Description:
By using 'COPY FROM FTP' statement provided by HPL/SQL, malicious FTP server is able to write arbitary file to
arbitary location on the machine which invokes hplsql command. This is because FTP client code in HPL/SQL does not verify
the destination location of the downloaded code. This does not affect hive cli user and hiveserver2 user as hplsql
is a separate command line script and need to be invoked differently.

Mitigation:
User who use hplsql with Hive 2.1.0 through 2.3.2 should upgrade to 2.3.3

Credit:
This issue was discovered by Danny Grander of Snyk

# CVE-2018-1282: JDBC driver is susceptible to SQL injection attack if the input parameters are not properly cleaned (HIVE-18788)

Severity: Important

Vendor: The Apache Software Foundation

Versions Affected: This vulnerability affects all versions of Hive JDBC driver from 0.7.1

Description:
Hive JDBC driver does not cleanup the parameters in setString and setBinaryStream. It is possible construct a query
which expand the filter condition specified in prepareStatement, thus allow user get the information not suppose to see.

Mitigation:
It is recommended to upgrade prior version of Hive JDBC driver to 2.3.3. Note Hive JDBC driver is not backward compatible with HiveServer2,
which means newer version of Hive JDBC driver may not talk to older version of HiveServer2. In particluar, Hive JDBC driver 2.3.3 won't talk
to HiveServer2 2.1.1 or prior. If user is using Hive code 2.1.1 or below might need to upgrade the entire Hive code to 2.3.3.

Before upgrade, user needs to check two use cases in Hive JDBC client code:
1. Avoid using setBinaryStream
2. Sanitize the input of setString, replacing all occurrences of \' to '

Credit:
This issue was discovered by Bear Giles of Coyote Song

# CVE-2018-1284: Hive UDF series UDFXPathXXXX are susceptible to XXE attack with embedded external entity (HIVE-18879)

Severity: Important

Vendor: The Apache Software Foundation

Versions Affected: This vulnerability affects all versions from 0.6.0

Description:
Malicious user might use any xpath UDFs (xpath/xpath_string/xpath_boolean/xpath_number/xpath_double/xpath_float/xpath_long/xpath_int/xpath_short)
to expose the content of a file on the machine running HiveServer2 owned by HiveServer2 user (usually hive)
if hive.server2.enable.doAs=false.

Mitigation:
Users who use xpath UDFs in HiveServer2 and hive.server2.enable.doAs=false are recommended to upgrade to 2.3.3, or update UDFXPathUtil.java to the head of branch-2.3 and rebuild hive-exec.jar:
https://git1-us-west.apache.org/repos/asf?p=hive.git;a=blob;f=ql/src/java/org/apache/hadoop/hive/ql/udf/xml/UDFXPathUtil.java;hb=refs/heads/branch-2.3

Credit:
This issue was discovered by Hortonworks
