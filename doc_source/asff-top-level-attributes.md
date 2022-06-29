# Other top\-level attributes<a name="asff-top-level-attributes"></a>

**[`Action`](asff-action.md)**  
Optional  
Provides details about an action that affects or that was taken on a resource\.  
**Type:** Object

**`CompanyName`**  
Optional  
The name of the company for the product that generated the finding\. For control\-based findings, the company is AWS\.  
Security Hub populates this attribute automatically for each finding\. You cannot update it using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html) or [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\. The exception to this is when you use a custom integration\. See [Using custom product integrations to send findings to AWS Security Hub](securityhub-custom-providers.md)\.  
When you use the Security Hub console to filter findings by company name, you use this attribute\.  
When you use the Security Hub API to filter findings by company name, you use the `aws/securityhub/CompanyName` attribute under `ProductFields`\.  
Security Hub does not synchronize those two attributes\.  
**Type:** String

**[`Compliance`](asff-compliance.md)**  
Optional  
Finding details related to a control\. Only returned for findings generated from a control\.  
**Type:** Object  
**Example**  

```
"Compliance": {
    "RelatedRequirements": ["Req1", "Req2"],
    "Status": "PASSED",
    "StatusReasons": [
        {
            "ReasonCode": "CLOUDWATCH_ALARMS_NOT_PRESENT";
            "Description": "CloudWatch alarms do not exist in the account"
        }
    ]
}
```

**`Confidence`**  
Optional  
A finding's confidence\. Confidence is defined as the likelihood that a finding accurately identifies the behavior or issue that it was intended to identify\.  
`Confidence` should only be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
Finding providers who want to provide a value for `Confidence` should use the `Confidence` attribute under [`FindingProviderFields`](asff-findingproviderfields.md)\. See [Using FindingProviderFields](finding-update-batchimportfindings.md#batchimportfindings-findingproviderfields)\.  
**Type:** Integer  
**Minimum value:** 0  
**Maximum value:** 100  
Confidence is scored on a 0–100 basis using a ratio scale, where 0 means 0\-percent confidence and 100 means 100\-percent confidence\.  
However, a data exfiltration detection based on a statistical deviation of network traffic has a much lower confidence because an actual exfiltration hasn't been verified\.  
**Example**  

```
"Confidence": 42
```

**`Criticality`**  
Optional  
The level of importance that is assigned to the resources that are associated with the finding\. A score of 0 means that the underlying resources have no criticality, and a score of 100 is reserved for the most critical resources\.   
`Criticality` should only be updated by [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\. It should not be updated by [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html)\.  
Finding providers who want to provide a value for `Criticality` should use the `Criticality` attribute under [`FindingProviderFields`](asff-findingproviderfields.md)\. See [Using FindingProviderFields](finding-update-batchimportfindings.md#batchimportfindings-findingproviderfields)\.  
**Type:** Integer  
**Minimum value:** 0  
**Maximum value:** 100  
Criticality is scored on a 0–100 basis, using a ratio scale that supports only full integers\. A score of 0 means that the underlying resources have no criticality, and a score of 100 is reserved for the most critical resources\.  
At a high level, when assessing criticality, you need to consider the following:  
+ Which findings impact resources that are more critical than other resources?
+ How much more critical are those resources compared to other resources?
For each resource, consider the following:  
+ Does the affected resource contain sensitive data \(for example, an S3 bucket with PII\)? 
+ Does the affected resource enable an adversary to deepen their access or extend their capabilities to carry out additional malicious activity \(for example, a compromised sysadmin account\)?
+ Is the resource a business\-critical asset \(for example, a key business system that if compromised could have significant revenue impact\)?
You can use the following guidelines:  
+ A resource powering mission\-critical systems or containing highly sensitive data can be scored in the 75–100 range\.
+ A resource powering important \(but not critical systems\) or containing moderately important data can be scored in the 25–75 range\.
+ A resource powering unimportant systems or containing nonsensitive data should be scored in the 0–24 range\.
**Example**  

```
"Criticality": 99
```

**[`FindingProviderFields`](asff-findingproviderfields.md)**  
Optional  
In [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html) requests, finding providers use `FindingProviderFields` to provide values for attributes that should only be updated by [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\. `FindingProviderFields` includes the following attributes:  
+ `Confidence`
+ `Criticality`
+ `RelatedFindings`
+ `Severity`
+ `Types`
`FindingProviderFields` can only be updated by [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html)\. It cannot be updated by [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
For details on how Security Hub handles updates from [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html) to `FindingProviderFields` and to the corresponding top\-level attributes, see [Using FindingProviderFields](finding-update-batchimportfindings.md#batchimportfindings-findingproviderfields)\.  
**Type:** Object  
**Example**  

```
"FindingProviderFields": {
    "Confidence": 42,
    "Criticality": 99,
    "RelatedFindings":[
      { 
        "ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty", 
        "Id": "123e4567-e89b-12d3-a456-426655440000" 
      }
    ],
    "Severity": {
        "Label": "MEDIUM", 
        "Original": "MEDIUM"
    },
    "Types": [ "Software and Configuration Checks/Vulnerabilities/CVE" ]
}
```

**`FirstObservedAt`**  
Optional  
Indicates when the potential security issue captured by a finding was first observed\.  
This timestamp reflects the time of when the event or vulnerability was first observed\. Consequently, it can differ from the `CreatedAt` timestamp, which reflects the time this finding record was created\.   
This timestamp should be immutable between updates of the finding record but can be updated if a more accurate timestamp is determined\.  
**Type: **String  
**Format:** Uses the `date-time` format specified in [RFC 3339 section 5\.6, Internet Date/Time Format](https://tools.ietf.org/html/rfc3339#section-5.6)\. The value cannot contain spaces\.  
**Example**  

```
"FirstObservedAt": "2017-03-22T13:22:13.933Z"
```

**`LastObservedAt`**  
Optional  
Indicates when the potential security issue that was captured by a finding was most recently observed by the security findings product\.  
This timestamp reflects the time when the event or vulnerability was last or most recently observed\. Consequently, it can differ from the `UpdatedAt` timestamp, which reflects when this finding record was last or most recently updated\.   
You can provide this timestamp, but it isn't required upon the first observation\. If you provide the field in this case, the timestamp should be the same as the `FirstObservedAt` timestamp\. You should update this field to reflect the last or most recently observed timestamp each time a finding is observed\.  
**Type:** String  
**Format:** Uses the `date-time` format specified in [RFC 3339 section 5\.6, Internet Date/Time Format](https://tools.ietf.org/html/rfc3339#section-5.6)\. The value cannot contain spaces\.  
**Example**  

```
"LastObservedAt": "2017-03-23T13:22:13.933Z"
```

**[`Malware`](asff-malware.md)**  
Optional  
A list of malware related to a finding\.  
**Type:** Array of malware objects  
**Maximum number of objects:** 5  
**Example**  

```
"Malware": [
    {
        "Name": "Stringler",
        "Type": "COIN_MINER",
        "Path": "/usr/sbin/stringler",
        "State": "OBSERVED"
    }
]
```

**[`Network`](asff-network.md) \(Deprecated\)**  
Optional  
The details of network\-related information about a finding\.  
This object is deprecated\. To provide this data, either map the data to a resource in `Resources`, or use the `Action` object\.  
**Type:** Object  
**Example**  

```
"Network": {
    "Direction": "IN",
    "OpenPortRange": {
        "Begin": 443,
        "End": 443
    },
    "Protocol": "TCP",
    "SourceIpV4": "1.2.3.4",
    "SourceIpV6": "FE80:CD00:0000:0CDE:1257:0000:211E:729C",
    "SourcePort": "42",
    "SourceDomain": "example1.com",
    "SourceMac": "00:0d:83:b1:c0:8e",
    "DestinationIpV4": "2.3.4.5",
    "DestinationIpV6": "FE80:CD00:0000:0CDE:1257:0000:211E:729C",
    "DestinationPort": "80",
    "DestinationDomain": "example2.com"
}
```

**[`NetworkPath`](asff-networkpath.md)**  
Optional  
A network path that is related to the finding\.  
Each entry in `NetworkPath` represents a component of the path\.  
**Type:** Array of objects

**[`Note`](asff-note.md)**  
Optional  
A user\-defined note that is added to a finding\.  
A finding provider can provide an initial note for a finding, but cannot add notes after that\.  
A note can only be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
**Type:** Object  
**Example**  

```
"Note": {
    "Text": "Don't forget to check under the mat.",
    "UpdatedBy": "jsmith",
    "UpdatedAt": "2018-08-31T00:15:09Z"
}
```

**[`PatchSummary`](asff-patchsummary.md)**  
Optional  
Provides a summary of patch compliance\.  
**Type:** Object

**[`Process`](asff-process.md)**  
Optional  
The details of process\-related information about a finding\.  
**Type:** Object  
Example:  

```
"Process": {
    "Name": "syslogd",
    "Path": "/usr/sbin/syslogd",
    "Pid": 12345,
    "ParentPid": 56789,
    "LaunchedAt": "2018-09-27T22:37:31Z",
    "TerminatedAt": "2018-09-27T23:37:31Z"
}
```

**`ProductFields`**  
Optional  
A data type where security findings products can include additional solution\-specific details that are not part of the defined AWS Security Finding Format\.  
For findings generated by Security Hub controls, `ProductFields` includes information about the control\. See [Generating and updating control findings](controls-findings-create-update.md)\.  
**Type:** Map of key\-value pairs  
**Maximum number of pairs:** 50  
**Maximum key length:** 128  
**Maximum value length:** 2048  
This field should not contain redundant data and must not contain data that conflicts with AWS Security Finding Format fields\.  
The "`aws/`" prefix represents a reserved namespace for AWS products and services only and must not be submitted with findings from third\-party integrations\.  
Although not required, products should format field names as `company-id/product-id/field-name`, where the `company-id` and `product-id` match those supplied in the `ProductArn` of the finding\.  
Field names can include the following characters: the following characters: A\-Z, a\-z, 0\-9, blank spaces, and \. : \+ \\ = @ \_ / \- \(hyphen\)\.  
**Example**  

```
"ProductFields": {
    "generico/secure-pro/Count": "6",
    "generico/secure-pro/Action.Type", "AWS_API_CALL",
    "API", "DeleteTrail",
    "Service_Name": "cloudtrail.amazonaws.com",
    "aws/inspector/AssessmentTemplateName": "My daily CVE assessment",
    "aws/inspector/AssessmentTargetName": "My prod env",
    "aws/inspector/RulesPackageName": "Common Vulnerabilities and Exposures"
}
```

**`ProductName`**  
Optional  
The name of the product that generated the finding\. For control\-based findings, the product name is Security Hub\.  
Security Hub populates this attribute automatically for each finding\. You cannot update it using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html) or [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\. The exception to this is when you use a custom integration\. See [Using custom product integrations to send findings to AWS Security Hub](securityhub-custom-providers.md)\.  
When you use the Security Hub console to filter findings by product name, you use this attribute\.  
When you use the Security Hub API to filter findings by product name, you use the `aws/securityhub/ProductName` attribute under `ProductFields`\.  
Security Hub does not synchronize those two attributes\.  
**Type:** String

**`RecordState`**  
Optional  
The record state of a finding\.   
By default, when initially generated by a service, findings are considered `ACTIVE`\.  
The `ARCHIVED` state indicates that a finding should be hidden from view\. Archived findings are not immediately deleted\. You can search, review, and report on them\.  
`RecordState` is intended for finding providers, and can only be updated by [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html)\. It cannot be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
To track the status of your investigation into a finding, do not use `RecordState`\. Instead, use [`Workflow`](asff-workflow.md)\.  
Finding providers update the record state\. Security Hub also automatically archives control\-based findings if the associated resource is deleted, the resource does not exist, or the control is disabled\.  
If the record state changes from `ARCHIVED` to `ACTIVE`, and the workflow status of the finding is either `NOTIFIED` or `RESOLVED`, then Security Hub automatically sets the workflow status to `NEW`\.  
**Type:** Enum  
**Valid values:** `ACTIVE` \| `ARCHIVED`  
**Example**  

```
"RecordState": "ACTIVE"
```

**`Region`**  
Optional  
The Region from which the finding was generated\.  
Security Hub populates this attribute automatically for each finding\. You cannot update it using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html) or [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
**Type:** String

**[`RelatedFindings`](asff-relatedfindings.md)**  
Optional  
A list of related findings\.   
`RelatedFindings` should only be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\. It should not be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchImportFindings.html)\.  
To provide a list of related findings, finding providers should use the `RelatedFindings` object under [`FindingProviderFields`](asff-findingproviderfields.md)\. See [Using FindingProviderFields](finding-update-batchimportfindings.md#batchimportfindings-findingproviderfields)\.  
**Type:** Array of `RelatedFinding` objects  
**Maximum number of objects:** 10  
**Example**  

```
"RelatedFindings": [
    { "ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty", 
      "Id": "123e4567-e89b-12d3-a456-426655440000" },
    { "ProductArn": "arn:aws:securityhub:us-west-2::product/aws/guardduty", 
      "Id": "AcmeNerfHerder-111111111111-x189dx7824" }
]
```

**[`Remediation`](asff-remediation.md)**  
Optional  
The remediation options for a finding\.   
**Type:** Object  
**Example**  

```
"Remediation": {
    "Recommendation": {
        "Text": "Run sudo yum update and cross your fingers and toes.",
        "Url": "http://myfp.com/recommendations/dangerous_things_and_how_to_fix_them.html"
    }
}
```

**`Sample`**  
Optional  
Whether the finding is a sample finding\.  
**Type:** Boolean

**`SourceUrl`**  
Optional  
A URL that links to a page about the current finding in the finding product\.  
**Type:** URL

**[`ThreatIntelIndicators`](asff-threatintelindicators.md)**  
Optional  
Threat intelligence details that are related to a finding\.  
**Type:** Array of threat intelligence indicator objects  
**Maximum number of objects:** 5  
**Example**  

```
"ThreatIntelIndicators": [
  {
    "Type": "IPV4_ADDRESS",
    "Value": "8.8.8.8",
    "Category": "BACKDOOR",
    "LastObservedAt": "2018-09-27T23:37:31Z",
    "Source": "Threat Intel Weekly",
    "SourceUrl": "http://threatintelweekly.org/backdoors/8888"
  }
]
```

**`UserDefinedFields`**  
Optional  
A list of name\-value string pairs that are associated with the finding\. These are custom, user\-defined fields that are added to a finding\. These fields can be generated automatically through your specific configuration\.  
Findings products must not use this field for data that the product generates\. Instead, findings products can use the `ProductFields` field for data that does not map to any standard AWS Security Finding Format field\.  
These fields can only be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
**Type:** Map of key\-value pairs  
**Maximum number of pairs:** 50  
**Format:** The key name can only contain letters, numbers, and the following special characters: \-\_=\+@\./:  
**Example**  

```
"UserDefinedFields": {
    "reviewedByCio": "true",
    "comeBackToLater": "Check this again on Monday"
}
```

**`VerificationState`**  
Optional  
The veracity of a finding\. Findings products can provide the value of `UNKNOWN` for this field\. A findings product should provide this value if there is a meaningful analog in the findings product's system\. This field is typically populated by a user determination or action after they investigate a finding\.  
A finding provider can provide an initial value for this attribute, but cannot update it after that\. This attribute can only be updated using [https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html](https://docs.aws.amazon.com/securityhub/1.0/APIReference/API_BatchUpdateFindings.html)\.  
**Type:** Enum  
**Valid values:**  
+ `UNKNOWN` – This is the default disposition of a security finding unless a user changes it\.
+ `TRUE_POSITIVE` – A user sets this value if the security finding has been confirmed\.
+ `FALSE_POSITIVE` – A user sets this value if the security finding has been determined to be a false alarm\.
+ `BENIGN_POSITIVE` – A user sets this value as a special case of `TRUE_POSITIVE` where the finding doesn't pose any threat, is expected, or both\.

**[`Vulnerabilities`](asff-vulnerabilities.md)**  
Optional  
A list of vulnerabilities that apply to the finding\.  
**Type:** Array of objects

**[`Workflow`](asff-workflow.md)**  
Optional  
Provides information about the status of the investigation into a finding\.  
The workflow status is not intended for finding providers\. The workflow status can only be updated using `BatchUpdateFindings`\. Customers can also update it from the console\. See [Setting the workflow status for findings](finding-workflow-status.md)\.  
Type: Object  
**Example**  

```
Workflow: {
    "Status": "NEW"
}
```

**`WorkflowState` \(Deprecated\)**  
Optional  
This field is being replaced by the `Status` field of the `Workflow` object\.  
The workflow state of a finding\. Findings products can provide the value of `NEW` for this field\. A findings product can provide a value for this field if there is a meaningful analog in the findings product's system\.  
**Type:** Enum  
**Valid values:**  
+ `NEW` – This can be associated with findings in the `Active` record state\. This is the default workflow state for any new finding\.
+ `ASSIGNED` – This can be associated with findings in the `Active` record state\. The finding has been acknowledged and given to someone to review or address\.
+ `IN_PROGRESS` – This can be associated with findings in the `Active` record state\. Team members are actively working on the finding\.
+ `RESOLVED` – This can be associated with findings in the `Archived` record state\. This differs from `DEFERRED` findings\. If the finding were to occur again \(be updated by the native service\) or any new finding matching this, the finding appears to customers as an active, new finding\.
+ `DEFERRED` – This can be associated with findings in the `Archived` record state\. It means that any additional findings that match this finding aren't shown for a set amount of time or indefinitely\.

  Either the customer doesn't consider the finding to be applicable, or it's a known issue that they don't want to include in the active dataset\.
+ `DUPLICATE` – This can be associated with findings in the `Archived` record state\. It means that the finding is a duplicate of another finding\.
**Example**  

```
"WorkflowState": "NEW"
```