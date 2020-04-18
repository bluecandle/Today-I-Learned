# AWS_IAM
AWS Identity and Access Management (IAM) is a web service that helps you securely control access to AWS resources. You use IAM to control who is authenticated (signed in) and authorized (has permissions) to use resources.

### root user
처음에 계정 생성해서 생기는 계정. 원래 내가 주로 쓰던거 ㅇㅇ
email, 비번으로 로그인 하는 계정. 근데, 이걸로 그냥 daily task 하는건 좋은 습관이 아니라는구만.
대신, best practice 를 소개해주고 있음(https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#create-iam-users)

흠... 그치 원래 root user 그대로 사용하면서도 '이러면 안될거같은데...???' 이런 생각들을 했으니까.
나중에 규모가 커지는 프로젝트를 할 때는 root user 는 최대한 활용하지 않고, 계정별 역할 구분을 잘 해서 작업이 이루어지도록 해봅시다.

