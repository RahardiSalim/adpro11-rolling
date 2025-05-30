### **1. What is the difference between Rolling Update and Recreate deployment strategy?**

The **Rolling Update** deployment strategy updates the application gradually by replacing the old Pods with new ones incrementally. This approach ensures high availability, as some old Pods continue to serve requests while new ones are being started. It is particularly useful for applications that must remain available during updates, such as web services or APIs. In contrast, the **Recreate** strategy stops all the existing Pods before starting new ones. This guarantees that no two versions of the application run simultaneously, which is helpful for apps that can’t tolerate version mismatches or shared resource conflicts. However, Recreate can cause downtime, whereas Rolling Update maintains service continuity.

### **2. Try deploying the Spring Petclinic REST using Recreate deployment strategy and document your attempt.**

To deploy the Spring Petclinic REST application using the **Recreate** strategy, I first edited the `deployment.yaml` file generated earlier. I modified the `strategy` section of the deployment spec to use `Recreate` instead of `RollingUpdate`. After applying the updated manifest using `kubectl apply -f deployment.yaml`, I observed that all existing Pods were terminated before new ones were created. During this transition, the application endpoint was unavailable for a short time, confirming the expected downtime behavior. This experiment clearly demonstrated how Recreate works and helped me understand scenarios where it may or may not be suitable.

### **3. Prepare different manifest files for executing Recreate deployment strategy.**

To implement the Recreate strategy, I modified the `deployment.yaml` file to include the following configuration under the `spec` field:

```yaml
strategy:
  type: Recreate
```

The full manifest retained the necessary metadata, container image, and port configuration but explicitly set the strategy to Recreate. After this change, I saved the updated YAML file and reapplied it to the cluster using `kubectl apply -f deployment.yaml`. The changes were successfully picked up by Kubernetes, and I was able to see the old Pods being removed completely before new ones were brought up. This new manifest allows the same application to be deployed using a different strategy depending on the operational requirement.

### **4. What do you think are the benefits of using Kubernetes manifest files?**

Using Kubernetes manifest files offers several significant benefits compared to deploying manually through command-line tools. First, manifest files act as a **single source of truth**—they capture the full state of your deployment in a version-controlled, declarative format. This makes it easy to **review, audit, and collaborate** on infrastructure changes in a team setting. Second, applying a manifest using `kubectl apply -f` is faster and less error-prone than manually typing commands, which reduces the chance of inconsistencies. Additionally, these files make it simple to **reproduce environments** on different clusters or restore a previous state, which is crucial for reliability and disaster recovery. Overall, they bring automation, clarity, and consistency to Kubernetes operations.
