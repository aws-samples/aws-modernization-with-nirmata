---
title: "Exploring Mutate and Generate Use Cases with Nirmata" 
chapter: false
weight: 69 
---

# Mutate and Generate Policy Use Cases with Nirmata

In this guide, we will explore simple `mutate` and `generate` policy use cases using two basic policies to test deployments. We will also look at how **Nirmata** helps identify and audit these real-time events within the platform.

---

## 📂 Access the Policy Files

Navigate to the policy directory in the Nirmata demo repository:

🔗 [GitHub - Custom Policies Folder](https://github.com/nirmata/demo-ws-nirmata/tree/main/Docs-and-Guides/Nirmata-policies/custom-policies)

Copy the following policy files from the folder:

- `mutate` policy
- `generate` policy

> **Note:** These policies are applied to all cluster resources.

---

## 🚀 Deploy the Generate Policy

The generate policy will create `quote` and `request` resources automatically for every new namespace created.

### Steps:

1. Apply the generate policy to your cluster.
2. Create a new namespace.
3. Observe:
   - `quota` and `request` resources are generated.
   - Each event is captured in Nirmata, which can be used for auditing and troubleshooting.

---

## 🔧 Deploy the Mutate Policy

The mutate policy updates a label on a resource automatically.

### Steps:

1. Apply the mutate policy to your cluster.
2. Create or modify a resource.
3. Verify that the label has been correctly added or updated. All the events are captured in Nirmata

---

## 💡 Advanced Use Cases

You can use different logic and operators to create more advanced scenarios. Policies can also be run **in the background**, meaning they apply not only during admission (resource creation/update) but also to **existing cluster resources**.

---

Happy testing! 🚀
