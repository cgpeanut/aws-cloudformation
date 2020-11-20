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

<img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/yaml-template.png"/>


chapter 4: cloudformation features
- intrinsic functions: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/intrinsic-function-reference.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>

AWS CloudFormation provides several built-in functions that help you manage your stacks. Use intrinsic functions in your templates to assign values to properties that are not available until runtime.

Note: 
You can use intrinsic functions only in specific parts of a template. Currently, you can use intrinsic functions in resource properties, outputs, metadata attributes, and update policy attributes. You can also use intrinsic functions to conditionally create stack resources.

- lab: intrinsic functions
- multiple resources
- lab: multiple resources
- pseudo parameters: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/pseudo-parameter-reference.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
  Pseudo parameters are parameters that are predefined by AWS CloudFormation. You do not declare them in your template. Use them the same way as you would a parameter, as the argument for the Ref function.
- lab: pseudo parameters
- mappings: <a href="https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/mappings-section-structure.html" target="_blank"><img src="https://github.com/cgpeanut/aws-cloudformation/blob/main/images/cloud.png" alt="IMAGE ALT TEXT HERE" width="35" height="25" /></a>
The optional Mappings section matches a key to a corresponding set of named values. For example, if you want to set values based on a region, you can create a mapping that uses the region name as a key and contains the values you want to specify for each specific region. You use the Fn::FindInMap intrinsic function to retrieve values in a map.

You cannot include parameters, pseudo parameters, or intrinsic functions in the Mappings section.

Syntax
The Mappings section consists of the key name Mappings. The keys in mappings must be literal strings. The values can be String or List types. The following example shows a Mappings section containing a single mapping named Mapping01 (the logical name).

Within a mapping, each map is a key followed by another mapping. The key must be a map of name-value pairs and unique within the mapping. The name can contain only alphanumeric characters (A-Za-z0-9).

YAML
Mappings: 
  Mapping01: 
    Key01: 
      Name: Value01
    Key02: 
      Name: Value02
    Key03: 
      Name: Value03

Examples
Basic mapping
The following example shows a Mappings section with a map RegionMap, which contains five keys that map to name-value pairs containing single string values. The keys are region names. Each name-value pair is the AMI ID for the HVM64 AMI in the region represented by the key.

The name-value pairs have a name (HVM64 in the example) and a value. By naming the values, you can map more than one set of values to a key.

YAML

Mappings: 
  RegionMap: 
    us-east-1: 
      "HVM64": "ami-0ff8a91507f77f867"
    us-west-1: 
      "HVM64": "ami-0bdb828fd58c52235"
    eu-west-1: 
      "HVM64": "ami-047bb4163c506cd98"
    ap-southeast-1: 
      "HVM64": "ami-08569b978cc4dfa10"
    ap-northeast-1: 
      "HVM64": "ami-06cd52961ce9f0d85"

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