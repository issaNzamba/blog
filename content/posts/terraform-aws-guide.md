---
title: "Déployer une infra AWS production-ready avec Terraform"
date: 2026-03-30
description: "VPC, EC2, Security Groups et EIP en IaC pur. Zéro clic dans la console."
tags: ["terraform", "aws", "iac", "devops"]
showToc: true
draft: false
---

## Pourquoi Terraform plutôt que la console AWS ?

Chaque ressource créée à la main dans la console AWS est une ressource que tu ne peux pas reproduire, versionner, ni supprimer proprement.

## Structure du projet
```hcl
infra/
├── main.tf
├── variables.tf
├── outputs.tf
└── modules/
    ├── vpc/
    └── ec2/
```

## Security Group avec le principe du moindre privilège
```hcl
resource "aws_security_group" "app" {
  name   = "${var.project}-sg"
  vpc_id = aws_vpc.main.id

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["${var.my_ip}/32"]
  }
}
```

## Workflow de déploiement
```bash
terraform init
terraform plan -out=tfplan
terraform apply tfplan
terraform output
```

> **Bonne pratique** : tag toujours `ManagedBy = "terraform"` sur toutes tes ressources.
