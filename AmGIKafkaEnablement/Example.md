![Alt text](images/diagram.png)

So here we need to focus on the region which is assigned to us **APAC (SEA)**.
Then we are going to take APAC. So the topic name is given here in **ESB forwarder** and then a listener **listenIMS**. So it means that we need a topic if it is not available, then we need to create a topic and the taxonomy and the naming convention we need to follow in the same way as given below:

```
hqscjde_{{env}}_global_itemmastercdm_pub_v1

<app>_<env>_<global>_<canonical/event>_<pattern>_<ver>
```

Before proceeding, we need to check two things.

First, we need to verify whether the schema is available. To do this, go to the **integration-artifacts** repository and search for **itemMasterCDM**. If it is present, it means the schema is already registered, and there’s no need to create it.

Second, we need to verify whether the application is registered. For this, navigate to the **gi-confluent-kafka-config** repository and check if the **hqscjde** application is available.

Next, we need to create a listener that will listen to the Producer. To do this, we must ensure the correct access is provided, and your service account name, as shown in the diagram above, is wm_apac_gbl_dev.

Now, we need to ensure the correct access rights are granted for the **hqscjde** topic. Before doing so, we should check whether the access is already granted or if it needs to be added. To verify this, search for the topic in the **gi-confluent-kafka-config** repository.

Similarly, we need to check if we have write access to the same topic. If not, we will need to grant the appropriate write access.

Since we are only working on the **APAC** region, we need to focus only on the listener **listenIMS**. The other listener, listenWMS, is managed by the Camel bundle.

To ensure the listener is available, we need to check its presence. For this, go to **wm-platform-config/env_apacdev/group_vars/\_wm_art/wmart_listeners** and verify if **listenIMS** is listed. If it is already present, we will reuse the same listener and simply add our configuration.
