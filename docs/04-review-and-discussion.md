# Module 4: Review and Discussion

In the last module we will have a short discussion and discuss exactly what occurred. We will also go over a number of questions to test your knowledge.

<!--and then provide instructions on how to clean up the workshop environment to prevent future charges in your AWS account.
-->

**Agenda**

1. Review & Discussion – 10 min
2. Questions – 10 min

## Architecture Overview
Below is a diagram of the overall workshop setup:
![Part 1 Diagram](./images/03-diagram-attack-v2.png)

## What is Really Going On?

In **Module 1** of the workshop you setup the initial components of your infrastructure including detective controls such as GuardDuty, Inspector, SecurityHub as well as simple notification and remediation pipeline. Some of the steps required manual configuration but you also ran a CloudFormation template which setup some of the components.

In **Module 2** you launched a second CloudFormation template that initiated the attack simulated by this workshop. The CloudFormation template created two EC2 instances. One instance (named **Malicious Host**) had an EIP attached to it that was added to your GuardDuty custom threat list. Although the **Malicious Host** is in the same VPC as the other instance, for the sake of the scenario (and to prevent the need to submit a penetration testing request) we acted as if it is on the Internet and represented the attack's computer. The other instance (named **Compromised Instance**) was your web server and it was taken over by the **Malicious Host**.

In **Module 3** you investigated the attack, remediated the damage, and setup some automated remediations for future attacks.  

**Here is what occurred in the attack:**

1. There are two instances created by the Module 2 CloudFormation template. They are in the same VPC but different subnets. The **Malicious Host** represents the attacker which we pretend is on the Internet. The Elastic IP on the **Malicious Host** is in a custom threat list in GuardDuty. The other instance named **Compromised Instance** represents the web server that was lifted and shifted into AWS.

2. Although company policy is that only key-based authentication should be enabled for SSH, at some point password authentication for SSH was enabled on the **Compromised Instance**.  This misconfiguration is identified in the Inspector scan that is triggered from the GuardDuty finding.

3. The **Malicious Host** performed a brute force SSH password attack against the **Compromised Instance**. The brute force attack is designed to be successful.

	!!! info "**GuardDuty Finding**: UnauthorizedAccess:EC2/SSHBruteForce"

4. The SSH brute force attack was successful and the attacker was able to log in to the **Compromised Instance**.

	!!! info "Successful login is confirmed in CloudWatch Logs (/threat-detection-wksp/var/log/secure)."

<!-- 5. The EC2 Instance that is created in the **Module** 2 CloudFormation template disabled default encryption on the **Data** bucket.  In addition the CloudFormation template made the **Data** bucket public.  This is used for the Macie part of the investigation in Module 3. We pretend that the attacker made the bucket public and removed the default encryption from the bucket.

	!!! info "**Macie Alert**: S3 Bucket IAM policy grants global read rights."
-->
6.  The Compromised Instance also has a cron job that continuously pings the Malicious Host to generate a GuardDuty finding based off the custom threat list.

	!!! info "**GuardDuty Finding**: UnauthorizedAccess:EC2/MaliciousIPCaller.Custom"

7. The API Calls that generated the API findings come from the **Malicious Host**. The calls use the temp creds from the IAM role for EC2 running on the **Malicious Host**. The GuardDuty findings are generated because the EIP attached to the **Malicious Host** is in a custom threat list.

	!!! info "**GuardDuty Finding**: Recon:IAMUser/MaliciousIPCaller.Custom or **GuardDuty Finding**: UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom"

8. A number of CloudWatch Events Rules are invoked by the GuardDuty findings and then these trigger various services.
	1.	**CloudWatch Event Rule**: The generic GuardDuty finding invokes a CloudWatch Event rule which triggers SNS to send an email.
	<!-- 2.	**CloudWatch Event Rule**: The generic Macie alert invokes a CloudWatch Event rule which triggers SNS to send an email. -->
	3.	**CloudWatch Event Rule**: The SSH brute force attack finding invokes a CloudWatch Event rule which triggers a Lambda function to block the attacker IP address of the attacker via a NACL as well as a Lambda function that runs an Inspector scan on the EC2 instance.
	4. **CloudWatch Event Rule**: The Unauthorized Access Custom MaliciousIP finding invokes a CloudWatch Event rule which triggers a Lambda function to block the IP address of the attacker via a NACL.

## Finished!

Congratulations on completing this workshop! This is the workshop's permanent home, so feel free to revisit as often as you'd like.
