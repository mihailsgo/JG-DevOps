# JG MD5
PRE-REQUISITRS: SAME ARCHITECTURE AS MD-4 SHOULD BE CREATED WITH TF
###Connection

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

###Instance creation


