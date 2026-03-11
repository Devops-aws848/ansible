plugin: amazon.aws.aws_ec2

regions:
  -us-east-1
  -us-east-2
filters:
   instance-state-name: running

hostnames:
  -instance-id

groups:
   virginia: placement.region == 'us-east-1'
   ohilo: placement.region == 'us-east-2'

keyed_groups:
key: tags.Name prefix: "tag_"

compose:
    ansible_host: public_ip_address
    ansible_user: "'ec2-user'"
    ansible_ssh_private_key_file: >
       '/home/ec2-user/ansikey.pem' if placement.region == 'us-east-1'
       else '/home/ec2-user/3tirekey.pem'
