# 🚀 **Module 4: Creating and Deploying Your First Serverless Function**

**Technology Stack:**

- Python
- Serverless

---

## 🎯 **Scenario**

Instead of managing servers directly, your applications can scale automatically in response to events, scaling down to zero when idle, which can improve resource efficiency and cost management. In this workspace, you will be required to quickly build an application image, upload it to the internal repository and then load balance it with Red Hat Serverless!

---

## 🐾 **Guided Walkthrough**

1. Create a new project/namespace from the openshift ui and name it `<username>-serverless`
2. From the openshift UI, start a new terminal
  - 2.1. Type in `oc project` to see which project you have enabled
  - 2.2. Type in `kn` to download the k-native CLI
3. Create your first Knative service

  ```sh
  kn service create module4-helloworld \
  --image quay.io/pknezevich/module4-dotnet-hellolang \
  --autoscale-window 20s \
  --env countryCode=DE
  ```

4. Verify your service has been created and you can curl/navigate to your application
5. Tag the revision to **green**
6. NOTE: we will update with ENV VAR but typically you would use: `kn service update <service-name> --image <new-image>`

  ```sh
  kn service update module4-helloworld \
  --env countryCode=DE \
  --tag module4-helloworld-00001=blue --untag green \
  --tag @latest=green \
  --traffic blue=100,green=0
  ```

7. Tag the blue revision to **inactive**
8. To create a third revision:
  - 8.1. From OC change the tag from `module4-helloworld-00002` green to blue
  - 8.2. Create a new revision
    - 8.2.1. ENV change (quick revision, this is usually done via an image change)
      - 8.2.1.1. Change a **different country code**
    - 8.2.2. Tag the **old revision to blue**
    - 8.2.3. Do not allow any traffic to new revision until you’re ready to (traffic distribution still stays on blue)

  ```sh
  kn service update module4-helloworld \
  --env countryCode=FR \
  --tag module4-helloworld-00002=blue --untag green \
  --tag @latest=green \
  --traffic blue=100,green=0
  ```

9. Build a 3-revision load-balanced environment with 3 different country languages, distribute the traffic evenly between each (33%)
10. Add a new image as a revision - `quay.io/pknezevich/helloworld-v2`

  ```sh
  kn service update module4-helloworld \
    --image quay.io/pknezevich/helloworld-v2 \
    --env countryCode=IT \
    --tag module4-helloworld-00001=blue --untag green \
    --tag @latest=green \
    --traffic blue=100,green=0
  ```

---

## 🧩 **Challenge**

- [ ] Design your current environment - canary, blue/green, knife edge
  - You can rename the revision tags to whatever you’d like them to be

---

## ✅ **Key Takeaways**

- First time approach to Red Hat’s Serverless Scale-to-zero and auto-scaling 
- Understanding Revisions and Introduced ‘image tagging’ to your container image
- Demonstrated blue-green deployment
- Demonstrated canary deployments (using traffic distribution)
- Demonstrated a knife-edge cutover
