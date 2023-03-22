## Connector

### Configuration

```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:ConfigurationModel",
  "@id" : "https://w3id.org/idsa/autogen/configurationModel/2ead2f4c-7569-462b-8db9-ea7bef7ed152",
  "ids:connectorStatus" : {
    "@id" : "idsc:CONNECTOR_ONLINE"
  },
  "ids:connectorProxy" : [ {
    "@type" : "ids:Proxy",
    "@id" : "https://w3id.org/idsa/autogen/proxy/8787228d-38ad-437f-b17d-6fb4bc4d794e",
    "ids:proxyAuthentication" : {
      "@type" : "ids:BasicAuthentication",
      "@id" : "https://w3id.org/idsa/autogen/basicAuthentication/abad7f6a-7658-431f-a9d8-be92a1f20a4a"
    },
    "ids:proxyURI" : {
      "@id" : "proxy.dortmund.isst.fraunhofer.de:3128"
    },
    "ids:noProxy" : [ {
      "@id" : "https://localhost:8080/"
    }, {
      "@id" : "http://localhost:8080/"
    } ]
  } ],
  "ids:connectorDescription" : {...},
  "ids:configurationModelLogLevel" : {
    "@id" : "idsc:NO_LOGGING"
  },
  "ids:connectorDeployMode" : {
    "@id" : "idsc:TEST_DEPLOYMENT"
  }
}
```

### Self-description

```
{
    "@type" : "ids:BaseConnector",
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/d7468684-1283-40e5-9242-025fbdd4c78b",
    "ids:publicKey" : {
      "@type" : "ids:PublicKey",
      "@id" : "https://w3id.org/idsa/autogen/publicKey/3d0733f4-a8b9-4ae3-a109-7bb0ffa777f1",
      "ids:keyType" : {
        "@id" : "idsc:RSA"
      },
      "ids:keyValue" : "MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAuw6mFrdflXZTJgFOA5smDXC09SmpJWoGpyERZNEy31pKdsRGhTipR27j9irmmqihv7gIgzCnx6kIRNGI2u0oFQ5FgvO1xxgzcihdpF0CheOf9INgisPkq5hj8Ae/DYXkvjhQ6c6ak/ZYfj0NpqyEPcJ5MLRmYGexMaMZmTbqDJvJl5JG3+bE3Ya21hTZYOxiSicpfFgJ30kn5aUIAtd05IZy7z1sDiVLtTXlLfe/ZQC4pnjFts+tc12sX9ihImnCkd0Wvz3CTZoyBSsc1TdBkb9m0C5tvg0fQP4QgF/zH2QoZnnrI52uAZ8MomWtY2lt3D0kkpR69pfVDJ7y3vN/ewIDAQAB"
    },
    "ids:version" : "v3.0.0",
    "ids:description" : [ {
      "@value" : "IDS Connector with static example resources hosted by the Fraunhofer ISST",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:securityProfile" : {
      "@id" : "idsc:BASE_SECURITY_PROFILE"
    },
    "ids:maintainer" : {
      "@id" : "https://example.com"
    },
    "ids:curator" : {
      "@id" : "https://example.com"
    },
    "ids:title" : [ {
      "@value" : "Dataspace Connector",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:hasDefaultEndpoint" : {
      "@type" : "ids:ConnectorEndpoint",
      "@id" : "https://w3id.org/idsa/autogen/connectorEndpoint/455a1352-e053-4400-ab5b-b99488ff9d01",
      "ids:accessURL" : {
        "@id" : "/api/ids/data"
      }
    },
    "ids:inboundModelVersion" : [ "4.0.0" ],
    "ids:outboundModelVersion" : "4.0.0"
}
```

### Resource

#### DSC Model

```
{
  "title": "ExampleResource",
  "description": "ExampleResourceDescription",
  "policy": "...", 
  "representations": [
    {
      "uuid": "8e3a5056-1e46-42e1-a1c3-37aa08b2aedd",
      "type": "XML",
      "byteSize": 101,
      "name": "Example Representation",
      "source": {
        "type": "local"
      }
    }
  ]
}
```

#### IDS

```
{
  "@type" : "ids:Resource",
  "@id" : "https://w3id.org/idsa/autogen/resource/0573bfc3-4627-4c73-a0db-54745e120485",
  "ids:language" : [ {
    "@id" : "idsc:EN"
  } ],
  "ids:title" : [ {
    "@value" : "ExampleResource",
    "@language" : "en"
  } ],
  "ids:description" : [ {
    "@value" : "ExampleResourceDescription",
    "@language" : "en"
  } ],
  "ids:representation" : [ {
    "@type" : "ids:Representation",
    "@id" : "https://w3id.org/idsa/autogen/representation/8e3a5056-1e46-42e1-a1c3-37aa08b2aedd",
    "ids:instance" : [ {
      "@type" : "ids:Artifact",
      "@id" : "https://w3id.org/idsa/autogen/artifact/8e3a5056-1e46-42e1-a1c3-37aa08b2aedd",
      "ids:fileName" : "Example Representation",
      "ids:byteSize" : 101
    } ],
    "ids:language" : {
      "@id" : "idsc:EN"
    },
    "ids:mediaType" : {
      "@type" : "ids:IANAMediaType",
      "@id" : "https://w3id.org/idsa/autogen/iANAMediaType/0b2389b5-43df-4104-9c4c-9ae774f1cbb6",
      "ids:filenameExtension" : "XML"
    }
  } ],
  "ids:contractOffer" : [ {
    "@type" : "ids:ContractOffer",
    "@id" : "https://w3id.org/idsa/autogen/contractOffer/6ca796f6-a153-4080-9ab2-cd9260cfbfea",
    "ids:permission" : [ {
      "@type" : "ids:Permission",
      "@id" : "https://w3id.org/idsa/autogen/permission/1c8e936f-226a-4642-ab79-5e9e4d66c473",
      "ids:title" : [ {
        "@value" : "Example Usage Policy",
        "@type" : "http://www.w3.org/2001/XMLSchema#string"
      } ],
      "ids:description" : [ {
        "@value" : "provide-access",
        "@type" : "http://www.w3.org/2001/XMLSchema#string"
      } ],
      "ids:action" : [ {
        "@id" : "idsc:USE"
      } ]
    } ],
    "ids:provider" : {
      "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
    }
  } ],
  "ids:keyword" : [ ],
  "ids:created" : {
    "@value" : "2021-01-24T17:43:50.930+01:00",
    "@type" : "http://www.w3.org/2001/XMLSchema#dateTimeStamp"
  },
  "ids:modified" : {
    "@value" : "2021-01-24T17:43:50.930+01:00",
    "@type" : "http://www.w3.org/2001/XMLSchema#dateTimeStamp"
  },
  "ids:resourceEndpoint" : [ {
    "@type" : "ids:ConnectorEndpoint",
    "@id" : "https://w3id.org/idsa/autogen/connectorEndpoint/e5e2ab04-633a-44b9-87d9-a097ae6da3cf",
    "ids:accessURL" : {
      "@id" : "/api/ids/data"
    }
  } ]
}
```

## Policy Patterns

### Provide Access
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/0abdf773-3d1e-48fc-a1e9-b6dd9b61b300",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/ae138d4f-f01d-4358-89a7-73e7c560f3de",
    "ids:description" : [ {
      "@value" : "provide-access",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ]
  } ]
}
```

### Prohibit Access
```
    {
      "@context" : {
        "ids" : "https://w3id.org/idsa/core/"
      },
      "@type" : "ids:ContractOffer",
      "@id" : "https://w3id.org/idsa/autogen/contractOffer/6dc1ca18-1a6b-4cf0-a84a-f374d50fe82d",
      "ids:prohibition" : [ {
        "@type" : "ids:Prohibition",
        "@id" : "https://w3id.org/idsa/autogen/prohibition/ff1b43b9-f3b1-44b1-a826-2efccc199a76",
        "ids:description" : [ {
          "@value" : "prohibit-access",
          "@type" : "http://www.w3.org/2001/XMLSchema#string"
        } ],
        "ids:action" : [ {
          "@id" : "idsc:USE"
        } ],
        "ids:title" : [ {
          "@value" : "Example Usage Policy",
          "@type" : "http://www.w3.org/2001/XMLSchema#string"
        } ]
      } ]
    }
```

### N Times Usage
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:NotMoreThanNOffer",
  "@id" : "https://w3id.org/idsa/autogen/notMoreThanNOffer/05b8b8d6-2e9a-42cc-9637-7b964231e349",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/ee842c6f-ce83-4512-8dd7-487a72f4a2b9",
    "ids:description" : [ {
      "@value" : "n-times-usage",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:constraint" : [ {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/2030a8f2-f03d-4af9-bce5-b9222e129dce",
      "ids:rightOperand" : {
        "@value" : "5",
        "@type" : "xsd:double"
      },
      "ids:operator" : {
        "@id" : "idsc:LTEQ"
      },
      "ids:leftOperand" : {
        "@id" : "idsc:COUNT"
      },
      "ids:pipEndpoint" : {
        "@id" : "https://localhost:8080/admin/api/resources/"
      }
    } ]
  } ]
}
```

### Duration Usage
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/a2f9fa88-7753-4227-8170-9365d20b189f",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/6b8abe49-6a31-4df4-80c6-764ad16d4c29",
    "ids:description" : [ {
      "@value" : "duration-usage",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:constraint" : [ {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/a5aa4243-432f-4360-aff4-c95da99eb266",
      "ids:rightOperand" : {
        "@value" : "PT4H",
        "@type" : "xsd:duration"
      },
      "ids:operator" : {
        "@id" : "idsc:SHORTER_EQ"
      },
      "ids:leftOperand" : {
        "@id" : "idsc:ELAPSED_TIME"
      }
    } ]
  } ]
}
```

### Usage During Interval
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/4cc74797-d45c-4a14-ba9d-f9c7ccb00007",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/ed3103a8-1cd9-44f6-9baa-8dddbcb1c6a5",
    "ids:description" : [ {
      "@value" : "usage-during-interval",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:constraint" : [ {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/0b7c4ca7-1f9e-4e30-8fa1-7551700c1980",
      "ids:rightOperand" : {
        "@value" : "2020-07-11T00:00:00Z",
        "@type" : "xsd:dateTimeStamp"
      },
      "ids:operator" : {
        "@id" : "idsc:AFTER"
      },
      "ids:leftOperand" : {
        "@id" : "idsc:POLICY_EVALUATION_TIME"
      }
    }, {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/9f2e0197-2ad9-442b-806b-5bb4951a2943",
      "ids:rightOperand" : {
        "@value" : "2020-07-11T00:00:00Z",
        "@type" : "xsd:dateTimeStamp"
      },
      "ids:operator" : {
        "@id" : "idsc:BEFORE"
      },
      "ids:leftOperand" : {
        "@id" : "idsc:POLICY_EVALUATION_TIME"
      }
    } ]
  } ]
}
```

### Usage Until Deletion
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/cbb9fbd1-ce14-4513-9cc1-7b98a0355653",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/03d35035-b293-43d9-8194-93776c402031",
    "ids:description" : [ {
      "@value" : "usage-until-deletion",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:constraint" : [ {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/a53b746d-f838-4db0-b5bc-414edec7cee1",
      "ids:rightOperand" : {
        "@value" : "2020-07-11T00:00:00Z",
        "@type" : "xsd:dateTimeStamp"
      },
      "ids:operator" : {
        "@id" : "idsc:AFTER"
      },
      "ids:leftOperand" : {
        "@id" : "idsc:POLICY_EVALUATION_TIME"
      }
    }, {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/7db8bb0b-06d0-4af0-86c7-f23c334c4a7e",
      "ids:rightOperand" : {
        "@value" : "2020-07-11T00:00:00Z",
        "@type" : "xsd:dateTimeStamp"
      },
      "ids:operator" : {
        "@id" : "idsc:BEFORE"
      },
      "ids:leftOperand" : {
        "@id" : "idsc:POLICY_EVALUATION_TIME"
      }
    } ],
    "ids:postDuty" : [ {
      "@type" : "ids:Duty",
      "@id" : "https://w3id.org/idsa/autogen/duty/770e6abb-dbe5-4ea3-bff5-aa4c29d29fb5",
      "ids:action" : [ {
        "@id" : "idsc:DELETE"
      } ],
      "ids:constraint" : [ {
        "@type" : "ids:Constraint",
        "@id" : "https://w3id.org/idsa/autogen/constraint/f2acf67f-bc4c-4e64-87fc-499eec24bc57",
        "ids:rightOperand" : {
          "@value" : "2020-07-11T00:00:00Z",
          "@type" : "xsd:dateTimeStamp"
        },
        "ids:operator" : {
          "@id" : "idsc:TEMPORAL_EQUALS"
        },
        "ids:leftOperand" : {
          "@id" : "idsc:POLICY_EVALUATION_TIME"
        }
      } ]
    } ]
  } ]
}
```

### Usage Logging
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/bd4d0cf0-683d-4770-b0b8-5204e03c3bf4",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/d5e58997-3337-49e9-bc01-d10aae3db52b",
    "ids:description" : [ {
      "@value" : "usage-logging",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:postDuty" : [ {
      "@type" : "ids:Duty",
      "@id" : "https://w3id.org/idsa/autogen/duty/e9728a46-91a2-4bd2-b210-4c142f41c715",
      "ids:action" : [ {
        "@id" : "idsc:LOG"
      } ]
    } ]
  } ]
}
```

### Usage Notification
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/76971cd1-2d98-4aee-929c-07091c39ced7",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/467f86d5-d89d-46b2-baa2-bf5ced8151b2",
    "ids:description" : [ {
      "@value" : "usage-notification",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:postDuty" : [ {
      "@type" : "ids:Duty",
      "@id" : "https://w3id.org/idsa/autogen/duty/33c8a7be-6119-4e44-bb33-de4ad22f928a",
      "ids:action" : [ {
        "@id" : "idsc:NOTIFY"
      } ],
      "ids:constraint" : [ {
        "@type" : "ids:Constraint",
        "@id" : "https://w3id.org/idsa/autogen/constraint/7c475c19-7b3a-4e0c-a00c-2d8abdcd466c",
        "ids:rightOperand" : {
          "@value" : "https://localhost:8000/api/ids/data",
          "@type" : "xsd:anyURI"
        },
        "ids:operator" : {
          "@id" : "idsc:DEFINES_AS"
        },
        "ids:leftOperand" : {
          "@id" : "idsc:ENDPOINT"
        }
      } ]
    } ]
  } ]
}
```

### Connector Restricted Usage 
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/"
  },
  "@type" : "ids:ContractOffer",
  "@id" : "https://w3id.org/idsa/autogen/contractOffer/edb9ae16-a3f2-4d09-9f99-7f8408119162",
  "ids:permission" : [ {
    "@type" : "ids:Permission",
    "@id" : "https://w3id.org/idsa/autogen/permission/478b29bd-e85e-4482-8816-0d507dc8683d",
    "ids:description" : [ {
      "@value" : "connector-restricted-usage",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:title" : [ {
      "@value" : "Example Usage Policy",
      "@type" : "http://www.w3.org/2001/XMLSchema#string"
    } ],
    "ids:action" : [ {
      "@id" : "idsc:USE"
    } ],
    "ids:constraint" : [ {
      "@type" : "ids:Constraint",
      "@id" : "https://w3id.org/idsa/autogen/constraint/fe8a40f9-38ad-4772-92c3-bb3eba2cba98",
      "ids:leftOperand" : {
        "@id" : "idsc:SYSTEM"
      },
      "ids:rightOperand" : {
        "@value" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ae",
        "@type" : "xsd:anyURI"
      },
      "ids:operator" : {
        "@id" : "idsc:SAME_AS"
      }
    } ]
  } ]
}
```


## IDS Messages

### Description Request: Self-description

Request Message: 
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:DescriptionRequestMessage",
  "@id" : "https://w3id.org/idsa/autogen/descriptionRequestMessage/cc5afba5-db62-4c68-9858-6fd27c9f521b",
  "ids:senderAgent" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:issuerConnector" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:issued" : {
    "@value" : "2020-10-13T13:55:54.345+02:00",
    "@type" : "http://www.w3.org/2001/XMLSchema#dateTimeStamp"
  },
  "ids:modelVersion" : "4.0.0",
  "ids:securityToken" : {
    "@type" : "ids:DynamicAttributeToken",
    "@id" : "https://w3id.org/idsa/autogen/dynamicAttributeToken/21b0ba17-dfb3-42f2-b7d0-ece4debfa4af",
    "ids:tokenValue" : "...",
    "ids:tokenFormat" : {
      "@id" : "idsc:JWT"
    }
  },
  "ids:recipientConnector" : [ {
    "@id" : "https://localhost:8080/api/ids/data"
  } ]
}
```

Response Message: 
```
--
Content-Disposition: form-data; name="header"
Content-Type: text/plain;charset=UTF-8
Content-Length: 1267

{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:DescriptionResponseMessage",
  "@id" : "https://w3id.org/idsa/autogen/descriptionResponseMessage/d4d67aaa-5e3f-479c-a144-6ada7ced91ad",
  "ids:modelVersion" : "4.0.0",
  "ids:issued" : {
    "@value" : "2021-01-24T17:47:55.143+01:00",
    "@type" : "http://www.w3.org/2001/XMLSchema#dateTimeStamp"
  },
  "ids:issuerConnector" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:senderAgent" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:securityToken" : {
    "@type" : "ids:DynamicAttributeToken",
    "@id" : "https://w3id.org/idsa/autogen/dynamicAttributeToken/3fa5130c-6983-4f88-b496-c54f686764c1",
    "ids:tokenValue" : "...",
    "ids:tokenFormat" : {
      "@id" : "idsc:JWT"
    }
  },
  "ids:recipientConnector" : [ {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  } ],
  "ids:correlationMessage" : {
    "@id" : "https://w3id.org/idsa/autogen/descriptionRequestMessage/d5f9dc10-94a3-4d92-a6ba-e4eea14fd463"
  }
}
--
Content-Disposition: form-data; name="payload"
Content-Type: text/plain;charset=UTF-8
Content-Length: 4051

{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:BaseConnector",
  ...
}
--
```

### Description Request: Metadata 

Request Message: 
```
{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:DescriptionRequestMessage",
  "@id" : "https://w3id.org/idsa/autogen/descriptionRequestMessage/bb1384e2-bd4b-4c28-b18a-3c95d476ef5a",
  "ids:senderAgent" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:issuerConnector" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:issued" : {
    "@value" : "2020-10-13T13:54:29.041+02:00",
    "@type" : "http://www.w3.org/2001/XMLSchema#dateTimeStamp"
  },
  "ids:modelVersion" : "4.0.0",
  "ids:securityToken" : {
    "@type" : "ids:DynamicAttributeToken",
    "@id" : "https://w3id.org/idsa/autogen/dynamicAttributeToken/12977a5e-8014-4457-b2a1-ded777e9730e",
    "ids:tokenValue" : "...",
    "ids:tokenFormat" : {
      "@id" : "idsc:JWT"
    }
  },
  "ids:recipientConnector" : [ {
    "@id" : "https://localhost:8080/api/ids/data"
  } ],
  "ids:requestedElement" : {
    "@id" : "https://w3id.org/idsa/autogen/resource/a4212311-86e4-40b3-ace3-ef29cd687cf9"
  }
}
```

Response Message:

```
--
Content-Disposition: form-data; name="header"
Content-Type: text/plain;charset=UTF-8
Content-Length: 1267

{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:DescriptionResponseMessage",
  "@id" : "https://w3id.org/idsa/autogen/descriptionResponseMessage/325d6118-b5aa-4364-a8c0-4b660715aeb9",
  "ids:modelVersion" : "4.0.0",
  "ids:issued" : {
    "@value" : "2021-01-24T17:52:22.760+01:00",
    "@type" : "http://www.w3.org/2001/XMLSchema#dateTimeStamp"
  },
  "ids:issuerConnector" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:senderAgent" : {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  },
  "ids:securityToken" : {
    "@type" : "ids:DynamicAttributeToken",
    "@id" : "https://w3id.org/idsa/autogen/dynamicAttributeToken/8ff9b176-22fd-4fc3-a2ff-0d324bb6c515",
    "ids:tokenValue" : "...",
    "ids:tokenFormat" : {
      "@id" : "idsc:JWT"
    }
  },
  "ids:recipientConnector" : [ {
    "@id" : "https://w3id.org/idsa/autogen/baseConnector/7b934432-a85e-41c5-9f65-669219dde4ea"
  } ],
  "ids:correlationMessage" : {
    "@id" : "https://w3id.org/idsa/autogen/descriptionRequestMessage/0c57495a-c31a-461c-a372-6f5d1412f0bd"
  }
}
--
Content-Disposition: form-data; name="payload"
Content-Type: text/plain;charset=UTF-8
Content-Length: 2527

{
  "@context" : {
    "ids" : "https://w3id.org/idsa/core/",
    "idsc" : "https://w3id.org/idsa/code/"
  },
  "@type" : "ids:Resource",
  ...
}
--
```
