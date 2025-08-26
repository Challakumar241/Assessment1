**Taerraform installation**



**wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg**

**echo "deb \[arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(grep -oP '(?<=UBUNTU\_CODENAME=).\*' /etc/os-release || lsb\_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list**

**sudo apt update \&\& sudo apt install terraform**





**Create ec2**



**resource "aws\_instance" "web" {**

  **ami           = "ami-0f918f7e67a3323f0" # change it**

  **instance\_type = "t3.micro"**



  **tags = {**

    **Name = "HelloWorld"**

  **}**

**}**



**Create ec2 with count**



**resource "aws\_instance" "web" {**

  **count         = 2**

  **ami           = "ami-0f918f7e67a3323f0" # change it**

  **instance\_type = "t3.micro"**



  **tags = {**

    **Name = "HelloWorld"**

  **}**

**}**



**Create ec2 with security group**



**resource "aws\_instance" "web" {**

  **count         = 2**

  **ami           = "ami-0f918f7e67a3323f0" # change it** 

  **instance\_type = "t3.micro"**

  **security\_groups = \["myweb"] # change it**



  **tags = {**

    **Name = "HelloWorld"**

  **}**

**}**



















**S3 Buket creation**



**resource "aws\_s3\_bucket" "example" {**

  **bucket = "my-tf-test-bucket-rohith-20250821" # Make it globally unique**



  **tags = {**

    **Name        = "My bucket"**

    **Environment = "Dev"**

  **}**

**}**



**Load balancer**



**resource "aws\_lb" "simple" {**

  **name               = "simple-lb"**

  **load\_balancer\_type = "application"**

  **internal           = false**

  **subnets = \[**

    **"subnet-0699c3a4977ab2577",# change it**

    **"subnet-0c3d87cc14a743922"# change it**

  **]**

**}**











