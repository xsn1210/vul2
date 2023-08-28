Vulnerability submitted by Shuning Xu

Vulnerability plugin :WordPress File Sharing Plugin

Plugin download link: https://cn.wordpress.org/plugins/user-private-files/

Vulnerability plugin version :2.0.3

Description :Download the WordPress File Sharing Plugin from the WordPress official website and install the latest version of the WordPress File Sharing Plugin. This plugin has a stored XSS vulnerability, that is, a stored cross-site scripting attack. Attackers will send messages to web pages (input forms, URLs, message boards, etc.) Insert malicious JavaScript code, modify the value of the parameter upfp_email_content_field in the post request to payload, and cause the trigger when the administrator/user visits the url /wp-admin/admin.php?page=upvf-free, so as to achieve the purpose of the attack. The essential reason is that the server does not strictly filter the data submitted by the user, causing the browser to treat the user's input as JS code and directly return it to the client for execution. What appears here is stored XSS, which can be stored in the webpage for a long time, and every user who browses this webpage will be hit. Attackers using stored cross-site scripting attacks can steal cookies from administrators and other users, causing cookie leakage, destroying the normal structure and style of pages, redirecting access to malicious websites, changing web content (such as download links), and so on.

Test process:

1. Install the WordPress File Sharing Plugin.
Download address: this website page https://cn.wordpress.org/plugins/ input plugin author name
The User Private Files to download at https://cn.wordpress.org/plugins/user-private-files/

![image-xss1.1[WordPressFile]](images/xss1.1[WordPressFile].png)

![image-xss1.2[WordPressFile]](images/xss1.2[WordPressFile].png)

2.导航到URL:http://www.test.com/wp-admin/admin.php?page=upvf-free

![image-xss1.3[WordPressFile]](images/xss1.3[WordPressFile].png)

3. Launch Burp Suite to capture network traffic. Use Burpsuite to capture and change packets for detection:

![image-xss1.4[WordPressFile]](images/xss1.4[WordPressFile].png)

4.Change the value of the parameter upfp_email_content_field in the post request to </TeXtArEa><sCrIpT>alert(2)</ScRiPt>, and then send the modified request,


![image-xss1.5[WordPressFile]](images/xss1.5[WordPressFile].png)

5.A pop-up window will appear on the web page, and the alarm box showing 2 will trigger XSS, indicating that there is cross-site vulnerability, record vulnerability, logout and re-login will still trigger XSS. Stop testing.
Change the value of the upfp_email_content_field parameter to </TeXtArEa><sCrIpT>alert(2)</ScRiPt>

![image-xss1.6[WordPressFile]](images/xss1.6[WordPressFile].png)

6. When you switch from user name x to user account "bob" (which also has administrator rights), XSS is still triggered when you access the url again, and the alarm box 2 is still displayed. It indicates that the XSS is a Stored XSS.

![image-xss1.7[WordPressFile]](images/xss1.7[WordPressFile].png)

7.View the page source code and find that it has been saved in the page for a long time:

![image-xss1.8[WordPressFile]](images/xss1.8[WordPressFile].png)

8. It is not only the upfp_email_content_field parameter that causes Stored-type XSS. In addition, the upfp_email_subject_field parameter can cause XSS. XSS can also be triggered by setting the POST request parameter upfp_email_subject_field to "OnMoUsEoVeR=prompt(1)//, and then sending the modified request.
Change the value of upfp_email_subject_field to "OnMoUsEoVeR=prompt(1)//, and then send the modified request.

![image-xss1.9[WordPressFile]](images/xss1.9[WordPressFile].png)

An alarm box with 1 is displayed:

![image-xss1.10[WordPressFile]](images/xss1.10[WordPressFile].png)

XSS protection:

1. Set httponly

2.xss filter(Check input, set whitelist)

3. Output check (encoding, escape)

4.CSP Content security policy

Risk level: [High Risk] stored cross-site exists in the application

Fixes: For XSS cross-site vulnerabilities, the following fixes can be used:

Overall repair mode: Verify all input data to effectively detect attacks; Encode all output data appropriately to prevent any scripts that have been successfully injected from running on the browser side. The details are as follows:

Input validation: Before a piece of data is accepted as displayable or stored, a standard input validation mechanism is used to verify the length, type, syntax, and business rules of all input data.

Output encoding: Before data output, ensure that the data submitted by the user has been correctly encoded as an entity. It is recommended that all characters be encoded instead of limited to a certain subset.

Specify the encoding of the output explicitly: Do not allow an attacker to choose the encoding for the user (such as ISO 8859-1 or UTF 8).

Note the limitations of blacklist authentication: simply looking for or replacing a few characters (such as "<" ">" or keywords like "script") can easily be bypassed by XSS variant attacks.

Beware of normalization errors: Before validating input, it must be decoded and normalized to conform to the application's current internal representation. Make sure the application does not decode the same input twice. Filter the data submitted by the client. It is recommended to filter out special characters such as double quotation marks (") and Angle brackets (<, >), or convert special characters contained in the data submitted by the client to entity form, such as "double quotation marks (") into entity form". , < corresponding entity form is &lt; , < corresponding entity form is &gt.
