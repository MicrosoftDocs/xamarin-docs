## Firewall Configuration

In order for tests to be submitted to Xamarin Test Cloud, the computer submitting the tests must be able to communicate with the Test Cloud servers. Firewalls must be configured to allow network traffic to and from the servers located at **testcloud.xamarin.com** on ports 80 and 443. This endpoint is managed by DNS and  the IP address is subject to change. 

In some situations, a test (or a device running the test) must communicate with web servers protected by a firewall. In this scenario the firewall must be configured to allow traffic from the following IP addresses:

* **195.249.159.238**
* **195.249.159.239**
