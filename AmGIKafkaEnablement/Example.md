![Alt text](images/diagram.png)

### 1. Here We will be focusing on the APAC (SEA) region, so we will be working with APAC.

### 2. The topic name is provided in the ESB forwarder, along with the listener "listenIMS."

### 3. If the topic is not already available, we need to create it.

### 4. We must follow the specified naming conventions and taxonomy for the topic:

```json
hqscjde_{{env}}_global_itemmastercdm_pub_v1 The format is:
<app>_<env>_<global>_<canonical/event>_<pattern>_<ver>
```

### 4. Before proceeding, we need to check two things:

- Check if the schema is available: Search for `itemMasterCDM` in the integration-artifacts repository. If found, the schema is already registered.
- Check if the application is registered: Look for the `hqscjde` application in the gi-confluent-kafka-config repository.

### 5. We need to create a listener that listens to the Producer. Ensure the correct access is granted, using the service account name wm_apac_gbl_dev.

### 6. We must verify if the correct access rights for the `hqscjde` topic are granted. Search for the topic in the gi-confluent-kafka-config repository to check if the access is already granted.

### 7. If we don't have write access to the topic, we need to grant the appropriate write access.

### 8. Since we're only working on the APAC region, we'll focus on the `listenIMS` listener. The `listenWMS` listener is managed by the Camel bundle.

### 9. To ensure the listener is available, check if `listenIMS` is listed in wm-platform-config/env_apacdev/group_vars/\_wm_art/wmart_listeners. If it's listed, we need to reuse the existing listener and add our configuration.

---

### cainiaowms ( Local - Singapore ) --> APAC ( Publish ) --> shipconfirmation events --> Global ( Consume ) --> OEBS ( Global Application )

- **cainiaowms_pd_global_shipconfirmcdm_pub_v1**

### Step1:- adding the schema

- **repo: [Integration-artifcats](https://github.com/AmwayCommon/integration-artifacts/blob/main/abgcdm/protobuf/ABGCDM_ShipConfirm_CDM_ShipConfirmCDM.proto)**

### Step2:- Record registration in kafka

- **repo: [gi-confluent-kafka-config](https://github.com/AmwayCommon/gi-confluent-kafka-config/blob/main/records/ABGCDM_ShipConfirm_CDM_ShipConfirmCDM.yml)**

### Step3:- Register an application

- **repo: [gi-confluent-kafka-config](https://github.com/AmwayCommon/gi-confluent-kafka-config/blob/main/applications/cainiaowms.yml)**

### Step4:- Creating a topic

- **repo: [gi-confluent-kafka-config](https://github.com/AmwayCommon/gi-confluent-kafka-config/blob/main/topics/cainiaowms/shipconfirmcdm.yml)**

### Step5:- Defining the topic

- **repo: [gi-confluent-kafka-config](https://github.com/AmwayCommon/gi-confluent-kafka-config/blob/main/env_integration_production/group_vars/_topics/topics_defs/pd.yml)**

### Step6:- Providing the access to consume and publish the data to this topic

- **repo: gi-confluent-kafka-config**
- **[publish permission:](https://github.com/AmwayCommon/gi-confluent-kafka-config/blob/main/env_integration_production/group_vars/_service_accounts/service_accounts/wm_apac_gbl_pd.yml)**

- **[Consume permission:](https://github.com/AmwayCommon/gi-confluent-kafka-config/blob/main/env_integration_production/group_vars/_service_accounts/service_accounts/wm_ent_gbl_pd.yml)**
