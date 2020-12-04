# IAM 


## Policy document (assume role)

[Assume roleの正体](https://dev.classmethod.jp/articles/iam-role-and-assumerole/)

```js
data "aws_iam_policy_document" "ecs_assume_role" {
  statement {
    effect = "Allow"
    principals {
      type        = "Service"
      identifiers = ["ecs-tasks.amazonaws.com"]
    }
    actions = [
      "sts:AssumeRole"
    ]
  }
}
```

## Policy document

```js
data "aws_iam_policy_document" "ecs" {
  version = "2012-10-17"
  statement {
    effect    = "Allow"
    resources = [
      aws_s3_bucket.bucket.arn
    ]
    actions = [
      "s3:PutObject",
      "s3:GetObject",
      "s3:ListBucket"
    ]
  }
}
```

## Policy (attach policy document)

```js
resource "aws_iam_policy" "ecs" {
  name   = "ecs-policy"
  policy = data.aws_iam_policy_document.ecs.json
}
```

## Role

```js
resource "aws_iam_role" "ecs" {
  name               = "ecs-role"
  assume_role_policy = data.aws_iam_policy_document.ecs_assume_role.json
}
```


## Atttach policy to role

```js
resource "aws_iam_role_policy_attachment" "ecs" {
  role       = aws_iam_role.ecs.name
  policy_arn = aws_iam_policy.ecs.arn
}
```
