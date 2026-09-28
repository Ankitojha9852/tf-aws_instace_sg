# provider.tf
---
provider "aws" {
  region  = "ap-southeast-1"
  profile = "tf-user"
}

---
 ec2.tf
---
resource "aws_instance" "rf-1" {
  ami                    = "ami-095f155a67469a548"
  instance_type          = "t3.micro"
  key_name               = "ojha"
  vpc_security_group_ids = [aws_security_group.sg.id]
  user_data              = <<-EOF
  #!/bin/bash
  sudo -i
  yum update -y
  yum install httpd -y
  systemctl start httpd
  systemctl enable httpd
  echo "HI! Ankit from this side" > /var/www/html/index.html

  EOF


  tags = {
    Name = "first-tf-instance"
  }

}

resource "aws_security_group" "sg" {
  name        = "first-sg-1"
  vpc_id      = "vpc-01fd4a00682336194"
  description = "initial-start"


  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]

  }


  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]

  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

}
---
# Terraform Commands
---
    1  terraform destroy
    2  clear
    3  terraform fmt
    4  terraform validate
    5  terraform plan
    6  terraform apply -auto-approve
    7  history
