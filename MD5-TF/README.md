# JG MD5

PRE-REQUISITES: SAME ARCHITECTURE AS MD-4 SHOULD BE CREATED WITH TF

### Connection

```sh
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}

provider "aws" {
  region  = "us-east-1"
}
```

### Instance creation

```sh
resource "aws_instance" "TF-Mihails-MD5-APP-SERVER" {
  ami           = "ami-06e46074ae430fba6"
  instance_type = "t2.micro"
  security_groups = [ "Mihails-MD5-APP-SG" ]
  key_name = "JG Mihails access key"
  tags = {
    Name = "Mihails-MD5-APP-SERVER"
    Owner = "Mihails JG"
   }
}

resource "aws_instance" "TF-Mihails-MD5-DB-SERVER" {
  ami           = "ami-06e46074ae430fba6"
  instance_type = "t2.micro"
  security_groups = [ "Mihails-MD5-DB-SG" ]
  key_name = "JG Mihails access key"
  tags = {
    Name = "Mihails-MD5-DB-SERVER"
    Owner = "Mihails JG"
   }
}

```

### Security groups

```sh
resource "aws_security_group" "Mihails-MD5-APP-SG" {
  name = "Mihails-MD5-APP-SG"
  description = "Security group for Mihails MD5 app"

  ingress {
    from_port = 80
    to_port = 80
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 443
    to_port = 443
    protocol = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = ["81.198.157.214/32"]
  }
}


resource "aws_security_group" "Mihails-MD5-DB-SG" {
  name = "Mihails-MD5-DB-SG"
  description = "Security group for Mihails MD5 DB"

  ingress {
    from_port = 22
    to_port = 22
    protocol = "tcp"
    cidr_blocks = ["81.198.157.214/32"]
  }

  ingress {
    from_port = 3306
    to_port = 3306
    protocol = "tcp"
    cidr_blocks = ["172.31.89.42/32"]
  }

}

```
