terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    random = {
      source  = "hashicorp/random"
      version = "~> 3.0"
    }
  }
}

provider "aws" {
  region = var.aws_region
}

variable "aws_region" {
  description = "AWS region to deploy into"
  type        = string
  default     = "eu-west-2"
}

variable "bucket_name" {
  description = "Base name for the S3 bucket (a random suffix is appended since S3 bucket names must be globally unique)"
  type        = string
  default     = "nexgen-jenkins-bucket"
}

# S3 bucket names must be unique across ALL of AWS, not just your account,
# so we append a random suffix to avoid collisions.
resource "random_id" "suffix" {
  byte_length = 4
}

resource "aws_s3_bucket" "this" {
  bucket = "${var.bucket_name}-${random_id.suffix.hex}"

  tags = {
    Project = "NexGen-Jenkins-Lab"
  }
}

output "bucket_name" {
  description = "Name of the created S3 bucket"
  value       = aws_s3_bucket.this.bucket
}
