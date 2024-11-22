Main participant: [Shehab eldin Hany](https://app.nuclino.com/users/95e3890d-bab4-413c-9751-c014e11f6b63?mId=NggvwSJI)

(1)Install required packages

install amazon-cloudwatch-agent

(2)Create and attach role to EC2 instance

Permission: CloudWatch Full Acess.

(3)Define Some Attributes

INSTANCE\_ID , NAMESPACE

(4)create bash script in instance&#x20;

- check url reachability each 30 sec and return back with 100 or 0

  &#x20;  if the url unreachable it will invoke command "service httpd restart"
- define customize metric "url\_status"
- change the script to be executable
- add the script to crontab to run every 30 sec

(5) create sns topic and subscribe to Gmail notification

![827a380f-d481-4335-bb9e-6d20574dcf97.png](https://files.nuclino.com/files/0b6d8fe7-cb3c-43b8-8705-12974ff2c291/827a380f-d481-4335-bb9e-6d20574dcf97.png)

(6) create alarm through cloudformation and link it to defined sns topic "Gmail notification"

![c7516a68-40e8-4c70-8cb9-455fd4b8cab3.png](https://files.nuclino.com/files/83f25aa9-70fb-4ed1-90f9-65880de7b598/c7516a68-40e8-4c70-8cb9-455fd4b8cab3.png)

[a7a7f0c3-060c-4c00-b5c6-60b971ab143b.png](https://files.nuclino.com/files/d37f479d-c57d-4678-9d1a-ab8628b7028a/a7a7f0c3-060c-4c00-b5c6-60b971ab143b.png)(7) Cloud Watch Alarms

![d556b069-2132-43e6-b0c3-586b10278bf6.png](https://files.nuclino.com/files/cd4dd965-797c-402a-a0e3-255987dedf8a/d556b069-2132-43e6-b0c3-586b10278bf6.png)

(8) publish topic to Gmail.

![eb1f710f-5045-4e9c-8904-bfa3fbb28515.png](https://files.nuclino.com/files/df1781d0-da8d-4343-a04a-eae1e6fcd834/eb1f710f-5045-4e9c-8904-bfa3fbb28515.png)

(9) notification received in Gmail inbox.

![105bd12e-8166-49d3-a732-142572c22a53.png](https://files.nuclino.com/files/23f7f0b9-d767-4240-9fc8-0a1604734f9c/105bd12e-8166-49d3-a732-142572c22a53.png)

<br>
