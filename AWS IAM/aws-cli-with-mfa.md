# Run the following commands

aws sts get-session-token --serial-number arn:aws:iam::291934546285:mfa/elhadji --token-code <token-from-device>

## Use 'set' rather than 'export' for Windows

export AWS_ACCESS_KEY_ID=
export AWS_SECRET_ACCESS_KEY=
export AWS_SESSION_TOKEN=

unset AWS_ACCESS_KEY_ID
unset AWS_SECRET_ACCESS_KEY
unset AWS_SESSION_TOKEN

