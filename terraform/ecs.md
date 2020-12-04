# ECS

## Get the latest task definition revision

https://github.com/terraform-providers/terraform-provider-aws/issues/632

```terraform
resource "aws_ecs_task_definition" "main" {
  ...
}
 
data "aws_ecs_task_definition" "main" {
  task_definition = aws_ecs_task_definition.family

  depends_on = [aws_ecs_task_definition.main]
}

resource "aws_ecs_service" "main" {
  task_definition = replace(aws_ecs_task_definition.main.arn, "/:(\\d+)$/", ":${max(aws_ecs_task_definition.main.revision, data.aws_ecs_task_definition.main.revision)}")
}
```
