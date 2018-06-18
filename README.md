# serverless-cognito-add-custom-attributes

This plugin allows you to add custom attributes to an existing CloudFormation-managed Cognito User Pool from serverless without losing all your users. At the time of writing (June 2018) CloudFormation doesn't know how to add custom attributes to a user pool without dropping and re-creating it, thus losing all your users.

This plugin also adds the specified attributes to a User Pool Client, giving that client read and write permissions for the new attribute.

# Requirements
- Node 8+
- serverless 1+

# Usage

```yml
plugins:
    - serverless-cognito-add-custom-attributes

custom:
  CognitoAddCustomAttributes: 
    CognitoUserPoolIdOutputKey: "CognitoUserPoolApplicationUserPoolId" 
    CognitoUserPoolClientIdOutputKey: "CognitoUserPoolApplicationUserPoolClientId" 
    CustomAttributes: 
      - 
        AttributeDataType: String
        DeveloperOnlyAttribute: False
        Mutable: True
        Name: "another" # this will end up being custom:another 
        Required: False

resources:
  Outputs:
    CognitoUserPoolApplicationUserPoolId:
      Value:
        Ref: CognitoUserPoolApplicationUserPool
    CognitoUserPoolApplicationUserPoolClientId:
      Value:
        Ref: CognitoUserPoolApplicationUserPoolClient
```

# Details

1. Output your UserPoolId via `resouces.Outputs`
1. Output your UserPoolClientId via `resouces.Outputs`
2. Add `CognitoAddCustomAttributes` to `custom` with the following structure:
```yml
  CognitoUserPoolIdOutputKey: "UserPool Output Key as a String"
  CognitoUserPoolClientIdOutputKey: "UserPoolClient Output Key as a String"
  CustomAttributes:
    - 
        AttributeDataType: String
        DeveloperOnlyAttribute: False
        Mutable: True
        Name: "another"
        Required: False
```

The names of your attributes supplied here will appear as `custom:{name}` when deployed.

For more information on the schema of attributes see:
https://docs.aws.amazon.com/cognito-user-identity-pools/latest/APIReference/API_SchemaAttributeType.html