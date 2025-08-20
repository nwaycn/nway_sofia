# nway_sofia

##Contact : wechat 18621575908

Optimized SIP Protection for FreeSWITCH with NIP: Enhanced Security on CentOS 7, openEuler 2403, and Debian 12
## Overview

Over the years, we have released various NIP (Nway IP Protection)-based SIP security solutions, including kernel-level protection, OpenSER, and FreeSWITCH integrations. After receiving substantial feedback from our users, we’ve found that the most widely appreciated and effective solution is directly integrating a module into FreeSWITCH. Therefore, we’ve further optimized the SIP protection module to better prevent scanning, fraud, and abuse, offering a comprehensive solution that strengthens your FreeSWITCH deployment.

This article will guide you through the optimized SIP protection module for FreeSWITCH, explaining how to configure it on CentOS 7, openEuler 2403, and Debian 12. We will also cover how to use the new features to enhance the security of your communication system.

New Features and Configuration for the Optimized SIP Protection Module

```
cp libnway_auth_lib.so /usr/lib/.
cp mod_sofia /usr/local/freeswitch/mod/.
```

1. New Configuration Options
To further optimize protection, we have introduced several new configuration options in the FreeSWITCH configuration files. These new parameters allow for more granular control over IP blacklists, whitelists, region-based filtering, and User-Agent blacklisting. These options give you the flexibility to fine-tune your security settings. Below are the new configuration options:
sofia.conf.xml
```
 <global_settings>
    <!-- New custom parameters for enhanced security -->
    <param name="custom_white_ip_file" value="/usr/local/freeswitch/conf/nway/custom_white_ip.txt"/>
    <param name="ip_black_file"        value="/usr/local/freeswitch/conf/nway/ip_black.txt"/>
    <param name="region_white_ip_file" value="/usr/local/freeswitch/conf/nway/region_white_ip.txt"/>
    <param name="src_blacklist_file"   value="/usr/local/freeswitch/conf/nway/src_black.txt"/>
    <param name="dst_blacklist_file"   value="/usr/local/freeswitch/conf/nway/dst_black.txt"/>
    <param name="ua_black_file"        value="/usr/local/freeswitch/conf/nway/ua_black.txt"/>
    <param name="allow_ua_empty"        value="false"/>
    <param name="nip_debug"        value="false"/>
    <param name="license_file"        value="/opt/nway/licenses/sofia.txt"/>

<!-- ... End of custom definitions -->
  </global_settings>
```
You can modify these files to store IP blacklists, whitelists, and User-Agent blacklist information. The module will match incoming requests against these files to determine whether to allow or block them.

2. IP Processing Priority

To ensure accurate protection, we have defined an IP processing priority order. Based on the configuration, the system checks the incoming IP and related information in the following order:

Custom Whitelist: The system first checks if the request IP is in the custom whitelist.

Blacklist: If not in the whitelist, the system checks if the IP is in the blacklist.

Region Whitelist: If the IP is not in the blacklist, it checks the region whitelist.

Default Action: By default, IPs not matching any of the lists are considered blacklisted.

3. IP Matching Examples

In real-world use, you can define these blacklists and whitelists using CIDR ranges or specific IP addresses. For example:

```
10.0.0.0/8
172.16.0.0/12
192.168.0.0/16
127.0.0.0/8
169.254.0.0/16
139.64.8.65
```

These rules give you the flexibility to manage internal networks, local networks, or specific IP addresses that require access. By adjusting the CIDR ranges or specifying specific IPs, you can control which addresses are allowed or denied access to your FreeSWITCH system.

4. Configuring Caller and Called Numbers
Beyond IP addresses, the module allows you to configure specific caller and called numbers to provide additional protection. This helps prevent misuse of certain phone numbers. Below is an example of such configuration:

```
110*
1000
???86
*99
```

In this configuration:

* represents one or more characters.

? represents a single character.

This allows for granular control over which numbers are protected and which are subject to security checks.

5. Configuring User-Agent Blacklist
To prevent malicious devices or non-compliant devices from registering, we have added support for configuring a User-Agent blacklist. Here are some example User-Agent patterns:

```
mic
tmd.*
badua.*
Cisco.*
```

These blacklist rules allow you to block requests from known malicious User-Agents, devices, or emulators, thus preventing abuse and security threats.

Core Features and Advantages of the Optimized SIP Protection Module
The main goal of this optimized module is to enhance FreeSWITCH’s ability to withstand a wide range of malicious attacks. Here are the key features and benefits:

1. Precise IP Address Management
With custom whitelists, blacklists, and region-based whitelists, you can accurately filter legitimate and malicious IP addresses. This granular approach ensures that only authorized traffic can access your FreeSWITCH system.

2. Flexible Number Control
The module allows you to configure protection for specific caller and called numbers, preventing toll fraud and abuse. By specifying protected numbers, you can avoid misuse or unauthorized usage.

3. Detailed User-Agent Filtering
By adding User-Agent blacklists, you can block malicious devices and non-compliant SIP devices from registering, thus avoiding potential misuse and attacks.

4. Improved Performance
The optimized module improves the processing efficiency, reducing false positives and ensuring accurate matching of requests against the defined blacklists, whitelists, and other rules.

5. Enhanced Protection Against Scanning, Fraud, and Abuse
The module protects against scanning, fraud, and misuse in real-time by analyzing SIP requests and applying defined rules, significantly improving the security of your communication system.

How to Configure the Optimized SIP Protection Module
Follow these steps to configure the optimized SIP protection module on your FreeSWITCH deployment:

1. Edit Configuration Files
Edit the FreeSWITCH configuration files to enable the new protection options. Add the necessary paths to the blacklist and whitelist files, and configure the protection rules based on your needs.

vim /usr/local/freeswitch/etc/freeswitch/sip_profiles/external.xml
Ensure that the new configuration items for IP blacklists, whitelists, and User-Agent filters are correctly set.

2. Define Blacklists and Whitelists
Create and configure the necessary files for IP addresses and User-Agent blacklists and whitelists. For example:

/usr/local/freeswitch/conf/nway/custom_white_ip.txt (Custom Whitelist)

/usr/local/freeswitch/conf/nway/ip_black.txt (IP Blacklist)

/usr/local/freeswitch/conf/nway/ua_black.txt (User-Agent Blacklist)

You can define these files using CIDR ranges or specific IP addresses and User-Agent patterns.

3. Restart FreeSWITCH
After configuring the module, restart FreeSWITCH to apply the changes:

systemctl restart freeswitch

4. Verify Protection
You can verify that the optimized protection module is working by checking the FreeSWITCH logs:



tail -f /usr/local/freeswitch/log/freeswitch.log
You should see logs indicating that malicious SIP probes or unauthorized access attempts are being blocked.

Conclusion
With the optimized SIP protection module for FreeSWITCH, powered by NIP-based security, you can significantly improve the defense against scanning, fraud, and abuse. The new configuration options give you fine-grained control over IP addresses, numbers, and User-Agents, allowing you to tailor the protection to meet your specific needs. By using this solution, you can secure your FreeSWITCH deployment and ensure reliable communication for your users.

If you have any questions or need assistance configuring the module, feel free to reach out to us. We're here to help you enhance your FreeSWITCH security!

6. Real-Time Traffic Analysis
The system continuously analyzes SIP traffic in real-time, identifying potential threats as they occur. This proactive approach allows for immediate mitigation of attacks, reducing the risk of service interruptions.
