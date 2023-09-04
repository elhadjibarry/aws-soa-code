# Create a role in the target account

1. Select 'another account' as the trusted entity
2. Enter the account ID and check the 'Require external ID' checkbox
3. Enter the external ID
4. Attach the following policy to the role:

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowBucketAccess",
            "Effect": "Allow",
            "Action": [
                "s3:GetObject",
                "s3:PutObject",
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::cross-account-test-122323",
                "arn:aws:s3:::cross-account-test-122323/*"
            ]
        }
    ]
}

# Assume the role in the target account

1. Run the following command to assume the role using the CLI

aws sts assume-role --role-arn arn:aws:iam::291934546285:role/cross-account-test --role-session-name mysession --external-id pass123456

2. Configure the credentials

aws configure set aws_access_key_id ASIAUH6FHVFW54LIAD4X --profile target-account
aws configure set aws_secret_access_key jaDCA5zCpn45M7snNcEWC7yUEcEWPc+LgEqErHdk --profile target-account
aws configure set aws_session_token IQoJb3JpZ2luX2VjEFEaCXVzLWVhc3QtMSJHMEUCIQCuMlOJp3yvIcCoEpYyQKtYlQX55BsY5y2mrEFDi5LnrAIgIerZEw7rJE4l/mMvLqpY2nZFj11Gs1vBRnQs67oRYaEqmgIIKhAAGgwyOTE5MzQ1NDYyODUiDL++YHDbBF+SttlvZSr3AYcVtq9UdUIVWq/EGb8CIJVRJVFuZIfNUarGf0HHDjVyDXFJ2uf7PDmkG+0O0wsiEuLh4+N2Qh5u9LQmut9zANOEFdfCKE95h24VsX0JBvtEaEs+jUCiGMp8jl7tZfwAKuVm8zoFI+uTTGTHvs418BVTRvmS4PnLCn3aQMl0nwwxw2FYFcJ0d+SOR5zAd1KFYmrb5R8Bu2mA6CAg/BHYz8gZzArtVtxXeBiBHr+y+nKsTsYKtG1m3B5ge6oBWIePI7kdc6TTiDem40sGLe5Tljh6Cl0YmN5CzIJ9lni7RntQWA0i46SWi2DY2a6SEeprpjkkq7sCOPcwwYjRpwY6nQH3I+9YaJscVDdhZNndcXNVpxa0cgktapAQhZbaRYuCFoxSAnw/DvAeCiRMck2eMyXtYBkOq2rXAWlfPWYymPDjpFdPV0NhQ2dzJWdNdobQa97d2quZZ1rizjriL72VW5wa39Won8nYD+hP1A93dJt0nWb7tToIikbpuTEc/RgQX254c21gU1f5GwUFyZmbgotJx8OhyM0G9oLyeJyA --profile target-account

3. Run CLI commands against the bucket

aws s3 ls s3://cross-account-test-122323 --profile target-account
