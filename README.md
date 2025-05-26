Project Goal
The goal was to investigate a phishing infrastructure hosted on a public Amazon S3 bucket, identify potential misconfigurations in access permissions, and retrieve evidence of user data compromise.

Process
1. Initial inspection of the suspicious website
I came across a suspicious link that led to a fake login page disguised as a webmail service called "Cmail":

http://darkinjector-phish.s3-website-us-west-2.amazonaws.com
Upon visiting the link, I immediately saw a classic phishing login form, clearly designed to steal user credentials.

2. Analyzing the AWS S3 Bucket
From the URL structure, I realized this was a public S3 bucket — meaning its contents could be listed and downloaded without authentication.

Using the AWS CLI, I ran the following command:
aws s3 ls s3://darkinjector-phish/ --region us-west-2 --no-sign-request

The output showed:
2025-05-24 10:23:45      1024 index.html
2025-05-24 10:25:12      5123 captured-logins-093582390
This confirmed that the bucket contained:

index.html — the phishing frontend.

captured-logins-093582390 — likely a log file storing credentials entered by victims.

3. Downloading and Analyzing Files
I downloaded the credentials file using:

aws s3 cp s3://darkinjector-phish/captured-logins-093582390 . --region us-west-2 --no-sign-request
Then I opened it:

cat captured-logins-093582390
Inside, I found:
......

FLAG: THM{this_is_not_what_i_meant_by_public}
Thus, the flag was successfully captured, and the credentials confirmed the phishing operation was functional.

Conclusion
As part of this project, I successfully:

Investigated a phishing page disguised as a webmail service.

Identified and explored a misconfigured public AWS S3 bucket.

Retrieved real user input data from the phishing backend.

Captured the challenge flag, marking the completion of the CTF task.

This case clearly demonstrates how dangerous misconfigured cloud resources can be — and how easily an attacker can impersonate a legitimate service.
