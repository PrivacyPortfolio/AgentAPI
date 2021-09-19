## GOOGLE CLOUD PLATFORM - FHIR IMPLEMENTATION GUIDELINE FOR DIGITAL RIGHTS 

===================================================
### Topic: GCP-FHIR implementation - Architectual Overview


Referred to as the 'Google Healthcare API', or 'Cloud Healthcare API', this FHIR implementation hosts the Service: healthcare.googleapis.com.

At its core, services hosted on the Google Cloud Platform are organized into projects, locations, and datasets.

FHIR resources reside in Datasets and Data Stores. Resource names consists of, at minimum, a project ID and a location. It can extend to include a dataset, data store, and any of a data store's child resources.

The format for a resource name for a data store that resides within a Cloud Healthcare API dataset looks like this:

/projects/PROJECT_ID/locations/LOCATION/datasets/DATASET_ID/DATA_STORE_TYPE/DATA_STORE_ID

Operations that access data in a modality-specific data store use a request path that consists of two pieces: the resource name (for identifying the data store to access), and a modality path (for identifying the actual data to retrieve).

Data Stores can be considered as catalogs containing metadata. Actual, raw data is persisted in Google Cloud Storage locations. Some API operations can also direct output from Data Stores to BigQuery destinations.

**This Implementation Guide is based on: GCP-FHIR REST API Discovery.json.**

============================
### Topic: Locations for FHIR Resources

    "Location": {
      "id": "Location",
      "description": "A resource that represents Google Cloud Platform location.",
      "type": "object",
    },

    "ListLocationsResponse": {
      "id": "ListLocationsResponse",
      "properties": { },
      "description": "The response message for Locations.ListLocations.",
      "type": "object"
    },


============================
### Topic: Datasets for FHIR Resources

    "Dataset": {
      "description": "A message representing a health dataset. A health dataset represents a collection of healthcare data pertaining to one or more patients. This may include multiple modalities of healthcare data, such as electronic medical records or medical imaging data.",
      "id": "Dataset",
      "type": "object"
    },

    "ListDatasetsResponse": {
      "id": "ListDatasetsResponse",
      "description": "Lists the available datasets.",
      "type": "object"
    },

============================
### Topic: FHIR Resources

    "Resources": {
      "id": "Resources",
      "type": "object",
      "description": "A list of FHIR resources.",
      "properties": { }
    },

    "SearchResourcesRequest": {
      "type": "object",
      "description": "Request to search the resources in the specified FHIR store.",
      "id": "SearchResourcesRequest"
    },

============================
### Topic: FHIR Stores

    "ListFhirStoresResponse": {
      "properties": { },
      "id": "ListFhirStoresResponse",
      "description": "Lists the FHIR stores in the given dataset.",
      "type": "object"
    },

    "ListConsentStoresResponse": {
      "id": "ListConsentStoresResponse",
      "type": "object",
      "properties": { }
    },

    "ListDicomStoresResponse": {
      "type": "object",
      "properties": { },
      "id": "ListDicomStoresResponse",
      "description": "Lists the DICOM stores in the given dataset."
    },

    "FhirStore": {
      "type": "object",
      "description": "Represents a FHIR store."
    },

    "Hl7V2Store": {
      "description": "Represents an HL7v2 store.",
      "id": "Hl7V2Store",
      "type": "object"
    },

    "DicomStore": {
      "description": "Represents a DICOM store.",
      "id": "DicomStore",
      "type": "object"
    },

    "ConsentStore": {
      "description": "Represents a consent store.",
      "type": "object",
      "id": "ConsentStore"
    },

============================
### Topic: Google Cloud Storage locations

    "GoogleCloudHealthcareV1ConsentGcsDestination": {
      "description": "The Cloud Storage location for export.",
      "type": "object",
      "id": "GoogleCloudHealthcareV1ConsentGcsDestination",
      "properties": { }
    },

    "GoogleCloudHealthcareV1DicomGcsDestination": {
      "description": "The Cloud Storage location where the server writes the output and the export configuration.",
      "properties": { },
      "type": "object",
      "id": "GoogleCloudHealthcareV1DicomGcsDestination"
    },

    "GoogleCloudHealthcareV1DicomGcsSource": {
      "description": "Specifies the configuration for importing data from Cloud Storage.",
      "id": "GoogleCloudHealthcareV1DicomGcsSource",
      "type": "object"
    },

    "GoogleCloudHealthcareV1FhirGcsDestination": {
      "id": "GoogleCloudHealthcareV1FhirGcsDestination",
      "description": "The configuration for exporting to Cloud Storage.",
      "type": "object"
    },

    "GoogleCloudHealthcareV1FhirGcsSource": {
      "type": "object",
      "id": "GoogleCloudHealthcareV1FhirGcsSource",
      "description": "Specifies the configuration for importing data from Cloud Storage.",
    },


============================
### Topic: Google BigQuery Destinations

    "GoogleCloudHealthcareV1FhirBigQueryDestination": {
      "id": "GoogleCloudHealthcareV1FhirBigQueryDestination",
      "type": "object",
      "description": "The configuration for exporting to BigQuery."
    },


============================
### Topic: API Operations

    "CancelOperationRequest": {
      "properties": {},
      "id": "CancelOperationRequest",
      "type": "object",
      "description": "The request message for Operations.CancelOperation."
    },

    "Operation": {
      "type": "object",
      "id": "Operation",
      "description": "This resource represents a long-running operation that is the result of a network API call."
    },    

    "OperationMetadata": {
      "id": "OperationMetadata",
      "type": "object",
      "description": "OperationMetadata provides information about the operation execution. Returned in the long-running operation's metadata field."
    },

    "ProgressCounter": {
      "type": "object",
      "description": "ProgressCounter provides counters to describe an operation's progress.",
      "id": "ProgressCounter"
    },

    "Status": {
      "id": "Status",
      "description": "The `Status` type defines a logical error model that is suitable for different programming environments, including REST APIs and RPC APIs. It is used by [gRPC](https://github.com/grpc). Each `Status` message contains three pieces of data: error code, error message, and error details. You can find out more about this error model and how to work with it in the [API Design Guide](https://cloud.google.com/apis/design/errors).",
      "type": "object"
    },

============================
### Topic: Import-Export Operations

    "ImportResourcesRequest": {
      "properties": {
        "contentStructure": { "enum": [  "CONTENT_STRUCTURE_UNSPECIFIED",  "BUNDLE",  "RESOURCE",  "BUNDLE_PRETTY",   "RESOURCE_PRETTY"],
      },
      "description": "Request to import resources.",
      "id": "ImportResourcesRequest",
      "type": "object"
    },

    "ExportResourcesRequest": {
      "id": "ExportResourcesRequest",
      "properties": {  },
      "type": "object",
      "description": "Request to export resources."
    },

    "ExportResourcesResponse": {
      "type": "object",
      "description": "Response when all resources export successfully. This structure is included in the response to describe the detailed outcome after the operation finishes successfully.",
      "properties": {},
      "id": "ExportResourcesResponse"
    },

    "ImportResourcesResponse": {
      "type": "object",
      "id": "ImportResourcesResponse",
      "properties": {},
      "description": "Final response of importing resources. This structure is included in the response to describe the detailed outcome after the operation finishes successfully."
    },    
    
    "ExportDicomDataRequest": {
      "description": "Exports data from the specified DICOM store. If a given resource, such as a DICOM object with the same SOPInstance UID, already exists in the output, it is overwritten with the version in the source dataset. Exported DICOM data persists when the DICOM store from which it was exported is deleted.",
      "type": "object",
      "id": "ExportDicomDataRequest"
    },

    "ImportDicomDataRequest": {
      "id": "ImportDicomDataRequest",
      "description": "Imports data into the specified DICOM store. Returns an error if any of the files to import are not DICOM files. This API accepts duplicate DICOM instances by ignoring the newly-pushed instance. It does not overwrite.",
      "type": "object"
    },

============================
### Topic: Messaging

    "ParsedData": {
      "description": "The content of a HL7v2 message in a structured format.",
      "id": "ParsedData",
      "type": "object"
    },

    "SchemaGroup": {
      "id": "SchemaGroup",
      "type": "object",
      "description": "An HL7v2 logical group construct."
    },

    "Hl7SchemaConfig": {
      "type": "object",
      "description": "Root config message for HL7v2 schema. This contains a schema structure of groups and segments, and filters that determine which messages to apply the schema structure to.",
      "id": "Hl7SchemaConfig"
    },

    "Empty": {
      "type": "object",
      "description": "A generic empty message that you can re-use to avoid defining duplicated empty messages in your APIs. A typical example is to use it as the request or the response type of an API method. For instance: service Foo { rpc Bar(google.protobuf.Empty) returns (google.protobuf.Empty); } The JSON representation for `Empty` is empty JSON object `{}`.",
      "id": "Empty"
    },

    "Field": {
      "id": "Field",
      "type": "object",
      "description": "A (sub) field of a type."
    },

    "SchemaSegment": {
      "id": "SchemaSegment",
      "description": "An HL7v2 Segment.",
      "type": "object"
    },

    "GroupOrSegment": {
      "id": "GroupOrSegment",
      "description": "Construct representing a logical group or a segment.",
      "type": "object"
    },

    "CreateMessageRequest": {
      "description": "Creates a new message.",
      "type": "object",
      "id": "CreateMessageRequest"
    },

    "SchematizedData": {
      "type": "object",
      "description": "The content of an HL7v2 message in a structured format as specified by a schema.",
      "id": "SchematizedData"
    },

    "IngestMessageRequest": {
      "description": "Ingests a message into the specified HL7v2 store.",
      "type": "object",
      "id": "IngestMessageRequest"
    },

    "ListMessagesResponse": {
      "id": "ListMessagesResponse",
      "type": "object",
      "description": "Lists the messages in the specified HL7v2 store."
    },

    "IngestMessageResponse": {
      "type": "object",
      "description": "Acknowledges that a message has been ingested into the specified HL7v2 store.",
      "id": "IngestMessageResponse"
    },

    "ParserConfig": {
      "description": "The configuration for the parser. It determines how the server parses the messages.",
      "id": "ParserConfig",
      "type": "object"
    },

    "Segment": {
      "type": "object",
      "id": "Segment",
      "description": "A segment in a structured format."
    },

    "HttpBody": {
      "id": "HttpBody",
      "type": "object",
      "description": "Message that represents an arbitrary HTTP body. It should only be used for payload formats that can't be represented as JSON, such as raw binary or an HTML page. This message can be used both in streaming and non-streaming API methods in the request as well as the response. It can be used as a top-level request field, which is convenient if one wants to extract parameters from either the URL or HTTP template into the request fields and also want access to the raw HTTP body. Example: message GetResourceRequest { // A unique request id. string request_id = 1; // The raw HTTP body is bound to this field. google.api.HttpBody http_body = 2; } service ResourceService { rpc GetResource(GetResourceRequest) returns (google.api.HttpBody); rpc UpdateResource(google.api.HttpBody) returns (google.protobuf.Empty); } Example with streaming methods: service CaldavService { rpc GetCalendar(stream google.api.HttpBody) returns (stream google.api.HttpBody); rpc UpdateCalendar(stream google.api.HttpBody) returns (stream google.api.HttpBody); } Use of this type only changes how the request and response bodies are handled, all other features will continue to work unchanged."
    },

    "Message": {
      "description": "A complete HL7v2 message. See [Introduction to HL7 Standards] (https://www.hl7.org/implement/standards/index.cfm?ref=common) for details on the standard.",
      "id": "Message",
      "type": "object"
    },


============================
### Topic: Entities, Identifiers, Roles

    "Entity": {
      "id": "Entity",
      "type": "object",
      "description": "The candidate entities that an entity mention could link to."
    },

    "AnalyzeEntitiesRequest": {
      "properties": {  },
      "type": "object",
      "description": "The request to analyze healthcare entities in a document.",
      "id": "AnalyzeEntitiesRequest"
    },

    "AnalyzeEntitiesResponse": {
      "type": "object",
      "id": "AnalyzeEntitiesResponse",
      "description": "Includes recognized entity mentions and relationships between them."
    },
    
    "EntityMention": {
      "id": "EntityMention",
      "description": "An entity mention in the document. The semantic type of the entity object: UNKNOWN_ENTITY_TYPE, ALONE, ANATOMICAL_STRUCTURE, ASSISTED_LIVING, BF_RESULT, BM_RESULT, BM_UNIT, BM_VALUE, BODY_FUNCTION, BODY_MEASUREMENT, COMPLIANT, DOESNOT_FOLLOWUP, FAMILY, FOLLOWSUP, LABORATORY_DATA, LAB_RESULT, LAB_UNIT, LAB_VALUE, MEDICAL_DEVICE, MEDICINE, MED_DOSE, MED_DURATION, MED_FORM, MED_FREQUENCY, MED_ROUTE, MED_STATUS, MED_STRENGTH, MED_TOTALDOSE, MED_UNIT, NON_COMPLIANT, OTHER_LIVINGSTATUS, PROBLEM, PROCEDURE, PROCEDURE_RESULT, PROC_METHOD, REASON_FOR_NONCOMPLIANCE, SEVERITY, SUBSTANCE_ABUSE, UNCLEAR_FOLLOWUP.",
      "type": "object"
    },

    "EntityMentionRelationship": {
      "description": "Defines directed relationship from one entity mention to another.",
      "type": "object",
      "id": "EntityMentionRelationship"
    },

    "LinkedEntity": {
      "type": "object",
      "description": "EntityMentions can be linked to multiple entities using a LinkedEntity message lets us add other fields, e.g. confidence.",
      "id": "LinkedEntity"
    },

    "PatientId": {
      "description": "A patient identifier and associated type.",
      "type": "object",
      "id": "PatientId"
    },

    "Signature": {
      "type": "object",
      "description": "User signature.",
      "id": "Signature"
    },

    "ArchiveUserDataMappingRequest": {
      "type": "object",
      "properties": {},
      "description": "Archives the specified User data mapping.",
      "id": "ArchiveUserDataMappingRequest"
    },

    "ArchiveUserDataMappingResponse": {
      "properties": {},
      "type": "object",
      "id": "ArchiveUserDataMappingResponse",
      "description": "Archives the specified User data mapping."
    },

    "Binding": {
      "description": "Associates `members` with a `role`.",
      "id": "Binding",
      "type": "object"
    },

    "ListUserDataMappingsResponse": {
      "type": "object",
      "description": "The returned User data mappings. The maximum number of User data mappings returned is determined by the value of page_size in the ListUserDataMappingsRequest.",
      "id": "ListUserDataMappingsResponse"
    },

    "UserDataMapping": {
      "type": "object",
      "description": "Maps a resource to the associated user and Attributes.",
      "id": "UserDataMapping"
    },

============================
### Topic: Consents

    "ConsentList": {
      "id": "ConsentList",
      "type": "object",
      "description": "The resource names of the Consents to evaluate against, of the form `projects/{project_id}/locations/{location_id}/datasets/{dataset_id}/consentStores/{consent_store_id}/consents/{consent_id}`."
    },

    "RevokeConsentRequest": {
      "id": "RevokeConsentRequest",
      "type": "object",
      "properties": { },
      "description": "Revokes the latest revision of the specified Consent by committing a new revision with `state` updated to `REVOKED`. If the latest revision of the given Consent is in the `REVOKED` state, no new revision is committed."
    },

    "ActivateConsentRequest": {
      "type": "object",
      "properties": {  },
      "description": "Activates the latest revision of the specified Consent by committing a new revision with `state` updated to `ACTIVE`. If the latest revision of the given Consent is in the `ACTIVE` state, no new revision is committed. A FAILED_PRECONDITION error occurs if the latest revision of the given consent is in the `REJECTED` or `REVOKED` state.",
      "id": "ActivateConsentRequest"
    },

    "ListConsentRevisionsResponse": {
      "type": "object",
      "properties": { },
      "id": "ListConsentRevisionsResponse"
    },

    "GoogleCloudHealthcareV1ConsentPolicy": {
      "type": "object",
      "description": "Represents a user's consent in terms of the resources that can be accessed and under what conditions.",
      "id": "GoogleCloudHealthcareV1ConsentPolicy"
    },

    "ListConsentArtifactsResponse": {
      "id": "ListConsentArtifactsResponse",
      "description": "The returned Consent artifacts. The maximum number of artifacts returned is determined by the value of page_size in the ListConsentArtifactsRequest.",
      "type": "object"
    },

    "EvaluateUserConsentsRequest": {
      "id": "EvaluateUserConsentsRequest",
      "description": "Evaluate a user's Consents for all matching User data mappings. Note: User data mappings are indexed asynchronously, causing slight delays between the time mappings are created or updated and when they are included in EvaluateUserConsents results.",
      "type": "object"
    },
    
    "RejectConsentRequest": {
      "id": "RejectConsentRequest",
      "type": "object",
      "description": "Rejects the latest revision of the specified Consent by committing a new revision with `state` updated to `REJECTED`. If the latest revision of the given Consent is in the `REJECTED` state, no new revision is committed."
    },

    "ConsentArtifact": {
      "description": "The resource name of the Consent artifact that contains proof of the end user's consent, of the form `projects/{project_id}/locations/{location_id}/datasets/{dataset_id}/consentStores/{consent_store_id}/consentArtifacts/{consent_artifact_id}`.",
      "id": "ConsentArtifact",
      "type": "object"
    },

    "ListConsentsResponse": {
      "description": "The returned Consents. The maximum number of Consents returned is determined by the value of page_size in the ListConsentsRequest.",
      "id": "ListConsentsResponse",
      "type": "object"
    },
    
    "Consent": {
      "type": "object",
      "id": "Consent",
      "description": "Represents a user's consent."
    },

    "ConsentEvaluation": {
      "type": "object",
      "id": "ConsentEvaluation",
      "description": "The detailed evaluation of a particular Consent."
    },

    "AttributeDefinition": {
      "id": "AttributeDefinition",
      "type": "object",
      "description": "A client-defined consent attribute."
    },

    "QueryAccessibleDataRequest": {
      "type": "object",
      "id": "QueryAccessibleDataRequest",
      "description": "Queries all data_ids that are consented for a given use in the given consent store and writes them to a specified destination. The returned Operation includes a progress counter for the number of User data mappings processed. Errors are logged to Cloud Logging (see [Viewing error logs in Cloud Logging] (https://cloud.google.com/healthcare/docs/how-tos/logging) and [QueryAccessibleData] for a sample log entry)."
    },

    "Result": {
      "type": "object",
      "id": "Result",
      "description": "The consent evaluation result for a single `data_id`."
    },

============================
### Topic: GCP Security

    "TestIamPermissionsRequest": {
      "properties": {  },
      "id": "TestIamPermissionsRequest",
      "description": "Request message for `TestIamPermissions` method.",
      "type": "object"
    },

    "CheckDataAccessRequest": {
      "id": "CheckDataAccessRequest",
      "type": "object",
      "description": "Checks if a particular data_id of a User data mapping in the given consent store is consented for a given use."
    },

    "Policy": {
      "type": "object",
      "description": "An Identity and Access Management (IAM) policy, which specifies access controls for Google Cloud resources. A `Policy` is a collection of `bindings`. A `binding` binds one or more `members` to a single `role`. Members can be user accounts, service accounts, Google groups, and domains (such as G Suite). A `role` is a named list of permissions; each `role` can be an IAM predefined role or a user-created custom role. For some types of Google Cloud resources, a `binding` can also specify a `condition`, which is a logical expression that allows access to a resource only if the expression evaluates to `true`. A condition can add constraints based on attributes of the request, the resource, or both.",
      "id": "Policy"
    },

    "TestIamPermissionsResponse": {
      "id": "TestIamPermissionsResponse",
      "description": "Response message for `TestIamPermissions` method.",
      "type": "object"
    },

    "SetIamPolicyRequest": {
      "id": "SetIamPolicyRequest",
      "type": "object",
      "description": "Request message for `SetIamPolicy` method."
    },

============================
### Topic: De-Identification

    "DeidentifyConfig": {
      "properties": {  },
      "id": "DeidentifyConfig",
      "type": "object",
      "description": "Configures de-id options specific to different types of content. Each submessage customizes the handling of an https://tools.ietf.org/html/rfc6838 media type or subtype. Configs are applied in a nested manner at runtime."
    },

    "DeidentifySummary": {
      "type": "object",
      "properties": {},
      "id": "DeidentifySummary",
      "description": "Contains a summary of the Deidentify operation."
    },

    "InfoTypeTransformation": {
      "id": "InfoTypeTransformation",
      "description": "A transformation to apply to text that is identified as a specific info_type.",
      "type": "object"
    },

    "CharacterMaskConfig": {
      "id": "CharacterMaskConfig",
      "description": "Mask a string by replacing its characters with a fixed character.",
      "type": "object",
      "properties": {  }
    },

    "CryptoHashConfig": {
      "id": "CryptoHashConfig",
      "description": "Pseudonymization method that generates surrogates via cryptographic hashing. Uses SHA-256. Outputs a base64-encoded representation of the hashed output (for example, `L7k0BHmF1ha5U3NfGykjro4xWi1MPVQPjhMAZbSV9mM=`).",
      "type": "object"
    },

    "DeidentifyDicomStoreRequest": {
      "type": "object",
      "description": "Creates a new DICOM store with sensitive information de-identified.",
      "id": "DeidentifyDicomStoreRequest"
    },

    "DeidentifyDatasetRequest": {
      "type": "object",
      "description": "Redacts identifying information from the specified dataset.",
      "id": "DeidentifyDatasetRequest"
    },

    "DicomConfig": {
      "description": "Specifies the parameters needed for de-identification of DICOM stores.",
      "id": "DicomConfig",
      "type": "object"
    },
    
    "FhirConfig": {
      "properties": {  },
      "type": "object",
      "id": "FhirConfig",
      "description": "Specifies how to handle de-identification of a FHIR store."
    },

    "ImageConfig": {
      "id": "ImageConfig",
      "description": "Specifies how to handle de-identification of image pixels.",
      "type": "object"
    },

    "RedactConfig": {
      "description": "Define how to redact sensitive values. Default behaviour is erase. For example, \"My name is Jane.\" becomes \"My name is .\"",
      "id": "RedactConfig",
      "type": "object"
    },

    "FieldMetadata": {
      "type": "object",
      "description": "Specifies FHIR paths to match, and how to handle de-identification of matching fields.",
      "id": "FieldMetadata"
    },

    "ReplaceWithInfoTypeConfig": {
      "type": "object",
      "id": "ReplaceWithInfoTypeConfig",
      "description": "When using the INSPECT_AND_TRANSFORM action, each match is replaced with the name of the info_type. For example, \"My name is Jane\" becomes \"My name is [PERSON_NAME].\" The TRANSFORM action is equivalent to redacting."
    },

============================
### Topic: Logging & Notifications

    "NotificationConfig": {
      "description": "Specifies where to send notifications upon changes to a data store.",
      "type": "object",
      "id": "NotificationConfig"
    },    

    "AuditConfig": {
      "type": "object",
      "description": "Specifies the audit configuration for a service. The configuration determines which permission types are logged, and what identities, if any, are exempted from logging. An AuditConfig must have one or more AuditLogConfigs. If there are AuditConfigs for both `allServices` and a specific service, the union of the two AuditConfigs is used for that service: the log_types specified in each AuditConfig are enabled, and the exempted_members in each AuditLogConfig are exempted. Example Policy with multiple AuditConfigs: { \"audit_configs\": [ { \"service\": \"allServices\", \"audit_log_configs\": [ { \"log_type\": \"DATA_READ\", \"exempted_members\": [ \"user:jose@example.com\" ] }, { \"log_type\": \"DATA_WRITE\" }, { \"log_type\": \"ADMIN_READ\" } ] }, { \"service\": \"sampleservice.googleapis.com\", \"audit_log_configs\": [ { \"log_type\": \"DATA_READ\" }, { \"log_type\": \"DATA_WRITE\", \"exempted_members\": [ \"user:aliya@example.com\" ] } ] } ] } For sampleservice, this policy enables DATA_READ, DATA_WRITE and ADMIN_READ logging. It also exempts jose@example.com from DATA_READ logging, and aliya@example.com from DATA_WRITE logging.",
      "id": "AuditConfig"
    },

    "AuditLogConfig": {
      "id": "AuditLogConfig",
      "description": "Provides the configuration for logging a type of permissions. Example: { \"audit_log_configs\": [ { \"log_type\": \"DATA_READ\", \"exempted_members\": [ \"user:jose@example.com\" ] }, { \"log_type\": \"DATA_WRITE\" } ] } This enables 'DATA_READ' and 'DATA_WRITE' logging, while exempting jose@example.com from DATA_READ logging.",
      "type": "object"
    },

    "Hl7V2NotificationConfig": {
      "id": "Hl7V2NotificationConfig",
      "description": "Specifies where and whether to send notifications upon changes to a data store.",
      "type": "object"
    },


## END OF IMPLEMENTATION GUIDE
