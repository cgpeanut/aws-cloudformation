# aws-cloudformation


chapter 1: 
- introduction
  
  YAML for cloudformation:
    <a href="https://aws.amazon.com/blogs/mt/the-virtues-of-yaml-cloudformation-and-using-cloudformation-designer-to-convert-json-to-yaml/"  target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

AWS CloudFormation provides the framework to define infrastructure-as-code in AWS.  Advantages of using YAML over JSON, as well as how to overcome some of the challenges in getting started writing CloudFormation in YAML.

The virtues of YAML
YAML CloudFormation fully supports all of the same features and functions as JSON CloudFormation with some additional features to reduce the length of code and increase readability. Say goodbye to the curly braces and most of the quotation marks of JSON when you use YAML. YAML uses parent nodes, child nodes, and indentation to denote hierarchy rather than curly braces and commas as in JSON.

YAML also supports comments using the # character. CloudFormation templates can get complex. Including key comments in the code can make it easier to understand, especially as teams get started with CloudFormation and develop templates together.

Let’s look at a code sample. The following YAML and JSON CloudFormation templates perform the same function, they deploy an Amazon Linux EC2 instance serving a webpage via Apache HTTP Server.

YAML template

``` 

AWSTemplateFormatVersion: 2010-09-09
Parameters:
  SubnetID:
    Type: AWS::EC2::Subnet::Id
    Description: Subnet to deploy EC2 instance into
  SecurityGroupIDs:
    Type: List<AWS::EC2::SecurityGroup::Id>
    Description: List of Security Groups to add to EC2 instance
  KeyName:
    Type: AWS::EC2::KeyPair::KeyName
    Description: >-
      Name of an existing EC2 KeyPair to enable SSH access to the instance
  InstanceType:
    Description: EC2 instance type
    Type: String
    Default: t2.micro
Mappings:
  AWSRegionToAMI:
    us-east-1:
      AMIID: ami-0b33d91d
    us-east-2:
      AMIID: ami-c55673a0
Resources:
  EC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      ImageId:
        !FindInMap                                 # This is an example of the short form YAML FindInMap function
          - AWSRegionToAMI                         # It accepts three parameters each denoted by a hyphen (-)
          - !Ref AWS::Region
          - AMIID
      InstanceType: !Ref InstanceType
      KeyName: !Ref KeyName
      SecurityGroupIds: !Ref SecurityGroupIDs
      SubnetId: !Ref SubnetID
      UserData:
        Fn::Base64:                                # YAML makes userdata much cleaner
          !Sub |
              #!/bin/bash -ex
              yum install -y httpd;
              echo "<html>I love YAML CloudFormation!!</html>" > /var/www/html/index.html;
              cd /var/www/html;
              chmod 755 index.html;
              service httpd start;
              chkconfig httpd on;
      Tags:                                      # Tags are an example of a sequence of mappings in YAML,
        -                                        # each key/value pair is separated by a hyphen
          Key: Name
          Value: CloudFormation Test - YAML      
        -
          Key: Environment
          Value: Development
```
  cloudformation Designer:
  <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/working-with-templates-cfn-designer.html"  target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

  AWS CloudFormation Designer (Designer) is a graphic tool for creating, viewing, and modifying AWS CloudFormation templates. With Designer, you can diagram your template resources using a drag-and-drop interface, and then edit their details using the integrated JSON and YAML editor. Whether you are a new or an experienced AWS CloudFormation user, AWS CloudFormation Designer can help you quickly see the interrelationship between a template's resources and easily modify templates.

  cloudformation for template formats: 
    <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-formats.html"  target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

You can add comments to the AWS CloudFormation templates you create outside of Designer. The following example shows a YAML template with inline comments. 
```
AWSTemplateFormatVersion: "2010-09-09"
Description: A sample template
Resources:
  MyEC2Instance: #An inline comment
    Type: "AWS::EC2::Instance"
    Properties: 
      ImageId: "ami-0ff8a91507f77f867" #Another comment -- This is a Linux AMI
      InstanceType: t2.micro
      KeyName: testkey
      BlockDeviceMappings:
        -
          DeviceName: /dev/sdm
          Ebs:
            VolumeType: io1
            Iops: 200
            DeleteOnTermination: false
            VolumeSize: 20
```

- files for hands on lab
    github repo: <a href="https://github.com/ACloudGuru/intro-to-CloudFormation_AC" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

    project files: <a href="https://learn.acloud.guru/course/intro-aws-cloudformation/learn/infrastructure-in-the-cloud/42ad57ef-44e6-c08f-370d-b3309735af40/watch" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>


chapter 2: 
- managing infrastructure in the cloud
- infrastructure as code
- what is cloudformation ?
- lab: introducing cloudformation

chapter 3: cloudformation fundamentals
- terminology
- template anatomy: 
<a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/template-anatomy.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

A template is a JSON- or YAML-formatted text file that describes your AWS infrastructure. The following examples show an AWS CloudFormation template structure and its sections.

YAML

The following example shows a YAML-formatted template fragment.

<img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png"/>



chapter 4: cloudformation features
- intrinsic functions: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- lab: intrinsic functions
- multiple resources
- lab: multiple resources
- pseudo parameters: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- lab: pseudo parameters
- mappings: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- lab: mappings
- input parameters:<a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/parameters-section-structure.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- lab: input parameters
- outputs
    <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/outputs-section-structure.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- lab: outputs

chapter 5: setting up EC2 instance
- introduction
- user data
- cloudformation helper scripts
    <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-helper-scripts-reference.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- cloudformation init
- lab: setting up an EC2 instance

chapter 6: Updating our stack with change sets
- introduction
  - cloudformation resource types reference: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
  - cloudformation update behavios of stack resources: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
- lab part 1: change sets: 
  cloudformation resource reference: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>


  cloudformation update behaviors of stack resources: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

- lab part 2: change sets:
- 
  cloudformation update behaviors of stack resources:<a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/using-cfn-updating-stacks-update-behaviors.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

cloudformation resource types reference: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-template-resource-type-ref.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>