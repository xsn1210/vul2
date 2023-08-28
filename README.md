Vulnerability submitted by Shuning Xu

Vulnerability plugin :WordPress File Sharing Plugin

Plugin download link: https://cn.wordpress.org/plugins/user-private-files/

Vulnerability plugin version :2.0.3

Description :Download the WordPress File Sharing Plugin from the WordPress official website and install the latest version of the WordPress File Sharing Plugin. This plugin has a stored XSS vulnerability, that is, a stored cross-site scripting attack. Attackers will send messages to web pages (input forms, URLs, message boards, etc.) Insert malicious JavaScript code to trigger when the administrator/user visits, so as to achieve the purpose of the attack. The essential reason is that the server does not strictly filter the data submitted by the user, causing the browser to treat the user's input as JS code and directly return it to the client for execution. What appears here is stored XSS, which can be stored in the webpage for a long time, and every user who browses this webpage will be hit. Attackers using stored cross-site scripting attacks can steal cookies from administrators and other users, causing cookie leakage, destroying the normal structure and style of pages, redirecting access to malicious websites, changing web content (such as download links), and so on.
