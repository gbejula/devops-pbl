# PROJECT 17: AUTOMATE INFRASTRUCTURE WITH IAC USING TERRAFORM PART 2

This is a continuation of project 16.

- Networking
- Private subnets & best practices

- Create 4 private subnets keeping in mind following principles:

  - Make sure you use variables or length() function to determine the number of AZs
  - Use variables and cidrsubnet() function to allocate vpc_cidr for subnets
  - Keep variables and resources in separate files for better code structure and readability
  - Tags all the resources you have created so far. Explore how to use format() and count functions to automatically tag subnets with its respective number.

- _main.tf_

```
# Create private subnets
resource "aws_subnet" "private" {
  count  = var.preferred_number_of_private_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_private_subnets
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 8 , count.index + 2)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]

}
```

- _variables.tf_

- Add the code below to the variables file

```
variable "preferred_number_of_private_subnets" {
  type = number
  description = "Number of private subnets"
}

```

- Add the code below to the _terraform.tfvars_ to take care of the private subnets

```
preferred_number_of_private_subnets = 4
```

- On the terminal run:

  `terraform plan`

- Got an error when I ran **terraform.plan** to add tags

  ![error](images/project-17/tag-error.png)

- There is a need to tags all the resources created so far. Explore how to use format() and count functions to automatically tag subnets with its respective number.

- A little bit more about Tagging

  - Resources are much better organized in ‘virtual’ groups
  - They can be easily filtered and searched from console or programmatically
  - Billing team can easily generate reports and determine how much each part of infrastructure costs how much (by department, by type, by environment, etc.)
  - You can easily determine resources that are not being used and take actions accordingly
  - If there are different teams in the organisation using the same account, tagging can help differentiate who owns which resources.

- **Note:** You can add multiple tags as a default set. for example, in out terraform.tfvars file we can have default tags defined.

Edit the following files:

- _main.tf_

```
- For public subnets

# Create public subnets
resource "aws_subnet" "public" {
  count  = var.preferred_number_of_public_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_public_subnets
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 8 , count.index)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]

  tags = merge(
    var.tags,
    {
      Name = format("%s-publicSubnet-%s", var.name, count.index)
    },
  )
}

- For private subnets

# Create private subnets
resource "aws_subnet" "private" {
  count  = var.preferred_number_of_private_subnets == null ? length(data.aws_availability_zones.available.names) : var.preferred_number_of_private_subnets
  vpc_id = aws_vpc.main.id
  cidr_block              = cidrsubnet(var.vpc_cidr, 8 , count.index + 2)
  map_public_ip_on_launch = true
  availability_zone       = data.aws_availability_zones.available.names[count.index]

  tags = merge(
  var.tags,
    {
      Name = format("%s-privatesubnet-%s", var.name, count.index)
    },
  )

}
```

- _variables.tf_

```
variable "name" {
  type = string
  default = "ACS"
}

variable "tags" {
  description = "A mapping of tags to assign to all resources."
  type = map(string)
  default = {}
}
```

- _terraform.tfvars_

```
tags = {
  Enviroment      = "production"
  Owner-Email = "ogaemmanuel@ymail.com"
  Managed-By = "Terraform"
  Billing-Account = "1234567890"
}
```

- Ran **terraform.plan** and got an error severally due to the region choosen

  ![region error](images/project-17/error.png)

- Changed region of the availability zone to **us-east-1** and the error was corrected

  ![terraform plan](images/project-17/terraform-plan.png)

> ## INTERNET GATEWAYS & FORMAT() FUNCTION

- Create an internet gateway in a separate terraform and the name the file **internet_gateway.tf**

- _internet_gateway.tf_

```
resource "aws_internet_gateway" "ig" {
  vpc_id = aws_vpc.main.id

  tags = merge(
    var.tags,
    {
      Name = format("%s-%s-%s!", var.name, aws_vpc.main.id, "IG")
    },
  )
}
```

- Did you notice how we have used format() function to dynamically generate a unique name for this resource? The first part of the %s takes the interpolated value of aws_vpc.main.id while the second %s appends a literal string IG and finally an exclamation mark is added in the end.

- If any of the resources being created is either using the count function, or creating multiple resources using a loop, then a key-value pair that needs to be unique must be handled differently.

- For example, each of our subnets should have a unique name in the tag section. Without the format() function, we would not be able to see uniqueness. With the format function, each private subnet’s tag will look like this.

> ## NAT GATEWAYS

- Create 1 NAT and 1 Elastic IP (EIP) address. Note that the NAT gateway must be created before the Elastic IP is created.

- The NAT gateways file should be called **natgateway.tf**

```
# Create aws elastic IP

resource "aws_eip" "nat_eip" {
  vpc        = true
  depends_on = [aws_internet_gateway.ig]

  tags = merge(
    var.tags,
    {
      Name = format("%s-EIP-%s", var.name, var.environment)
    },
  )
}

# Create aws nat gateway

resource "aws_nat_gateway" "nat" {
  allocation_id = aws_eip.nat_eip.id
  subnet_id     = element(aws_subnet.public.*.id, 0)
  depends_on    = [aws_internet_gateway.ig]

  tags = merge(
    var.tags,
    {
      Name = format("%s-Nat-%s", var.name, var.environment)
    },
  )
}
```

- _variables.tf_

```
variable "environment" {
  type = string
  description = "Environment"
}
```

- _terraform.tfvars_

```
environment = "dev"
```

- Run from the terminal - **terraform.plan**

  ![nat & int gw](images/project-17/nat%26int-gw.png)

> ## AWS ROUTES

- Create a file called **route_tables.tf** and use it to create routes for both public and private subnets, create the below resources. Ensure they are properly tagged.

  - aws_route_table
  - aws_route
  - aws_route_table_association

  <br>

- _route_tables.tf_

```
# create private route table
resource "aws_route_table" "private-rtb" {
  vpc_id = aws_vpc.main.id

  tags = merge(
    var.tags,
    {
      Name = format("%s-Private-Route-Table-%s", var.name, var.environment)
    },
  )
}

# create route for the private route table and attach the nat gateway to it
resource "aws_route" "private-rtb-route" {
  route_table_id         = aws_route_table.private-rtb.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_nat_gateway.nat.id
}

# associate all private subnets to the private route table
resource "aws_route_table_association" "private-subnets-assoc" {
  count          = length(aws_subnet.private[*].id)
  subnet_id      = element(aws_subnet.private[*].id, count.index)
  route_table_id = aws_route_table.private-rtb.id
}

# create route table for the public subnets
resource "aws_route_table" "public-rtb" {
  vpc_id = aws_vpc.main.id

  tags = merge(
    var.tags,
    {
      Name = format("%s-Public-Route-Table-%s", var.name, var.environment)
    },
  )
}

# create route for the public route table and attach the internet gateway
resource "aws_route" "public-rtb-route" {
  route_table_id         = aws_route_table.public-rtb.id
  destination_cidr_block = "0.0.0.0/0"
  gateway_id             = aws_internet_gateway.ig.id
}

# associate all public subnets to the public route table
resource "aws_route_table_association" "public-subnets-assoc" {
  count          = length(aws_subnet.public[*].id)
  subnet_id      = element(aws_subnet.public[*].id, count.index)
  route_table_id = aws_route_table.public-rtb.id
}
```

- On the terminal, run the terraform plan command:

  ![route](images/project-17/route-plan.png)

> ## CREATE CERTIFICATE FROM AMAZON CERTIFICATE MANAGER

- Create **cert.tf** and add the following code

**Note:** Ensure to change the domain name to your own domain that is created.

- Check out the terraform documentation for AWS Certificate Manager

- _**cert.tf**_

```
# The entire section create a certiface, public zone, and validate the certificate using DNS method

# Create the certificate using a wildcard for all the domains created in oche.link
resource "aws_acm_certificate" "oche" {
  domain_name       = "*.oche.link"
  validation_method = "DNS"
}

# calling the hosted zone
data "aws_route53_zone" "oche" {
  name         = "oche.link"
  private_zone = false
}

# selecting validation method
resource "aws_route53_record" "oche" {
  for_each = {
    for dvo in aws_acm_certificate.oche.domain_validation_options : dvo.domain_name => {
      name   = dvo.resource_record_name
      record = dvo.resource_record_value
      type   = dvo.resource_record_type
    }
  }

  allow_overwrite = true
  name            = each.value.name
  records         = [each.value.record]
  ttl             = 60
  type            = each.value.type
  zone_id         = data.aws_route53_zone.oche.zone_id
}

# validate the certificate through DNS method
resource "aws_acm_certificate_validation" "oche" {
  certificate_arn         = aws_acm_certificate.oche.arn
  validation_record_fqdns = [for record in aws_route53_record.oche : record.fqdn]
}

# create records for tooling
resource "aws_route53_record" "tooling" {
  zone_id = data.aws_route53_zone.oche.zone_id
  name    = "tooling.oche.link"
  type    = "A"

  alias {
    name                   = aws_lb.ext-alb.dns_name
    zone_id                = aws_lb.ext-alb.zone_id
    evaluate_target_health = true
  }
}

# create records for wordpress
resource "aws_route53_record" "wordpress" {
  zone_id = data.aws_route53_zone.oche.zone_id
  name    = "wordpress.oche.link"
  type    = "A"

  alias {
    name                   = aws_lb.ext-alb.dns_name
    zone_id                = aws_lb.ext-alb.zone_id
    evaluate_target_health = true
  }
}
```
