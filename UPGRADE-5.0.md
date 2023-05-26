# Upgrade from v4.32 to v5.0

Please consult the `examples` directory for reference example configurations. If you find a bug, please open an issue with supporting configuration to reproduce.

#### ⚠️ This guide is under active development. As tasks for v5.0 are implemented, the associated upgrade guidance/changes are documented here. See [here](https://github.com/aws-ia/terraform-aws-eks-blueprints/milestone/1) to track progress of v5.0 implementation.

## List of backwards incompatible changes

-

## Additional changes

### Added

-

### Modified

-

### Removed

-

### Variable and output changes

1. Removed variables:

    -

2. Renamed variables:

    -

3. Added variables:

    -

4. Removed outputs:

    -

5. Renamed outputs:

    -

6. Added outputs:

    -

## Upgrade Migrations

### Before - v4.32 Example

```hcl
module "eks_blueprints" {
  source = "github.com/aws-ia/terraform-aws-eks-blueprints?ref=v4.32.0"

  # TODO

}
```

### After - v5.0 Example

```hcl
module "eks_blueprints" {
  source = "github.com/aws-ia/terraform-aws-eks-blueprints?ref=v5.0.0"

  # TODO

}
```

### Diff of Before vs After

```diff
module "eks_blueprints" {
-  source = "github.com/aws-ia/terraform-aws-eks-blueprints?ref=v4.32.0"
+  source = "github.com/aws-ia/terraform-aws-eks-blueprints?ref=v5.0.0"

  # TODO
}
```

### State Move Commands

In conjunction with the changes above, users can elect to move their external capacity provider(s) under this module using the following move command. Command is shown using the values from the example shown above, please update to suit your configuration names:

```sh
terraform state rm 'module.eks.kubernetes_config_map.aws_auth[0]'

# Control plane
terraform state mv 'module.eks.module.aws_eks.aws_eks_cluster.this[0]' 'module.eks.aws_eks_cluster.this[0]'

# Control plane IAM role
terraform state mv 'module.eks.module.aws_eks.aws_iam_role.this[0]' 'module.eks.aws_iam_role.this[0]'
terraform state mv 'module.eks.module.aws_eks.aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKSClusterPolicy"]' 'module.eks.aws_iam_role_policy_attachment.this["AmazonEKSClusterPolicy"]'
terraform state mv 'module.eks.module.aws_eks.aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKSVPCResourceController"]' 'module.eks.aws_iam_role_policy_attachment.this["AmazonEKSVPCResourceController"]'

# Cluster primary security group tags
terraform state mv 'module.eks.module.aws_eks.aws_ec2_tag.cluster_primary_security_group["Blueprint"]' 'module.eks.aws_ec2_tag.cluster_primary_security_group["Blueprint"]'
terraform state mv 'module.eks.module.aws_eks.aws_ec2_tag.cluster_primary_security_group["GithubRepo"]' 'module.eks.aws_ec2_tag.cluster_primary_security_group["GithubRepo"]'

# Control plane security group
terraform state mv 'module.eks.module.aws_eks.aws_security_group.cluster[0]' 'module.eks.aws_security_group.cluster[0]'

# Node (shared) security group
terraform state mv 'module.eks.module.aws_eks.aws_security_group.node[0]' 'module.eks.aws_security_group.node[0]'

# OIDC provider
terraform state mv 'module.eks.module.aws_eks.aws_iam_openid_connect_provider.oidc_provider[0]' 'module.eks.aws_iam_openid_connect_provider.oidc_provider[0]'

# terraform state mv 'module.eks.module.kms[0].aws_kms_key.this[0]' 'module.eks.module.kms.aws_kms_key.this[0]'
# terraform state mv 'module.eks.module.kms[0].aws_kms_alias.this' 'module.eks.module.kms.aws_kms_alias.this["alias/migration"]'

# Fargate profile(s)
terraform state mv 'module.eks.module.aws_eks_fargate_profiles["fargate"].aws_eks_fargate_profile.eks_fargate' 'module.eks.module.fargate_profile["fargate"].aws_eks_fargate_profile.this[0]'
terraform state mv 'module.eks.module.aws_eks_fargate_profiles["fargate"].aws_iam_role.fargate[0]' 'module.eks.module.fargate_profile["fargate"].aws_iam_role.this[0]'
terraform state mv 'module.eks.module.aws_eks_fargate_profiles["fargate"].aws_iam_role_policy_attachment.fargate_pod_execution_role_policy["arn:aws:iam::aws:policy/AmazonEKSFargatePodExecutionRolePolicy"]' 'module.eks.module.fargate_profile["fargate"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKSFargatePodExecutionRolePolicy"]'

# Managed nodegroup(s)
terraform state mv 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_eks_node_group.managed_ng' 'module.eks.module.eks_managed_node_group["initial"].aws_eks_node_group.this[0]'
terraform state mv 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_iam_role.managed_ng[0]' 'module.eks.module.eks_managed_node_group["initial"].aws_iam_role.this[0]'
terraform state mv 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_iam_role_policy_attachment.managed_ng["arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"]' 'module.eks.module.eks_managed_node_group["initial"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"]'
terraform state mv 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_iam_role_policy_attachment.managed_ng["arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"]' 'module.eks.module.eks_managed_node_group["initial"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"]'
terraform state mv 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_iam_role_policy_attachment.managed_ng["arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"]' 'module.eks.module.eks_managed_node_group["initial"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"]'

# Self-managed nodegroup(s)
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_autoscaling_group.self_managed_ng' 'module.eks.module.self_managed_node_group["self_managed"].aws_autoscaling_group.this[0]'
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_iam_instance_profile.self_managed_ng[0]' 'module.eks.module.self_managed_node_group["self_managed"].aws_iam_instance_profile.this[0]'
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_iam_role.self_managed_ng[0]' 'module.eks.module.self_managed_node_group["self_managed"].aws_iam_role.this[0]'
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_iam_role_policy_attachment.self_managed_ng["arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"]' 'module.eks.module.self_managed_node_group["self_managed"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEC2ContainerRegistryReadOnly"]'
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_iam_role_policy_attachment.self_managed_ng["arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"]' 'module.eks.module.self_managed_node_group["self_managed"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKSWorkerNodePolicy"]'
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_iam_role_policy_attachment.self_managed_ng["arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"]' 'module.eks.module.self_managed_node_group["self_managed"].aws_iam_role_policy_attachment.this["arn:aws:iam::aws:policy/AmazonEKS_CNI_Policy"]'
terraform state mv 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].module.launch_template_self_managed_ng.aws_launch_template.this["self-managed-node-group"]' 'module.eks.module.self_managed_node_group["self_managed"].aws_launch_template.this[0]'

```

These will be destroyed
- 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_iam_instance_profile.managed_ng[0]'
- 'module.eks.module.aws_eks_managed_node_groups["managed"].aws_iam_role_policy_attachment.managed_ng["arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"]'
- 'module.eks.module.aws_eks_fargate_profiles["fargate"].aws_iam_policy.cwlogs[0]'
- 'module.eks.module.aws_eks_fargate_profiles["fargate"].aws_iam_role_policy_attachment.cwlogs[0]'
- 'module.eks.module.aws_eks_self_managed_node_groups["self_managed"].aws_iam_role_policy_attachment.self_managed_ng["arn:aws:iam::aws:policy/AmazonSSMManagedInstanceCore"]'

# OLD


